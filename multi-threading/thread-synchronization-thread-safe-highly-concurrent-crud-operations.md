<a href="../">Notebook</a> > <a href="./">Multi-Threading (POSIX Threads)</a> > Thread Synchronization - Thread-Safe Highly Concurrent CRUD Operations

# Thread Synchronization - Thread-Safe Highly Concurrent CRUD Operations



Consider this section as one of the most important section in the **thread synchronization**. In this section we will explore how to implement thread-safe bullet-proof CRUD operations.



## What are CRUD Operations?

* We often need to implement CRUD operations on data structures.

  **C** - **C**reate an object (e.g., inserting a new student in a list)

  **R** - Look-up an object and **R**ead (e.g., displaying students with 80%+ in science)

  **U** - **U**pdate an object (e.g., increase the student marks by 10%)

  **D** - **D**elete an object (e.g., remove a student object from the list whose roll number is $n$)



## Introduction to Thread-Safe Highly Concurrent CRUD Operations

* In a multi-threaded application, multiple threads may attempt to perform CRUD operations on the same instance of a data structure, say for example, a linked list. We will use a linked list of students as a shared object here, but the fundamentals apply to all other types data structures as well.

* Simply locking an entire data structure prior to performing any operations on it to achieve thread synchronization will make the multi-threaded program completely thread-safe.

  However, there are some problems to be considered:

  1. The entire data structure is locked
  2. Application performance degrades
  3. More contention between threads

  For example, why would a reader thread want to lock out other reader threads when they are completely safe to perform operations simultaneously. Why would a writer thread want to lock out other threads that want to perform a completely independent, non-conflicting operations on a different element of the data structure, and so on.

  The critical section must be as concise as possible for the sake of performance of the application.

* Locking an entire data structure is highly discouraged. (It is like locking down an entire city for a plumber to safely fix a bathroom.)



## Goals of Thread-Safe Highly Concurrent CRUD Operations

* The goal is to implement CRUD operations over a data structure which are not only **thread-safe**, but also guarantee the **maximum concurrency** as possible. (Maximum concurrency means that threads must not unnecessarily block each other in an attempt to get an access to the shared resource.)
  * Threads performing **read** operations only must NOT compete/block each other to get an access.
  * If one thread performs a **read** operation and another performs a **write** operation, they must NOT block each other unless they are performing those operations on the same element of the data structure.
  * If there are two threads performing **write** operations, they must NOT block each other unless they are performing the operations on the same element of the data structure.
  * If a thread is performing read/write operations on an object, other threads in the system must NOT free the object from the memory prematurely. (Segmentation fault expected!)
  * If a thread is blocked on a lock, other threads in the system must NOT free the lock from the memory.
* We will implement the thread-safe highly concurrent CRUD operations that meet all the requirements mentioned above.



## Tools Necessary for Thread-Safe Highly Concurrent CRUD Operations

1. Read/write locks (not regular mutexes)
2. Reference count object

* In order to implement thread-safe highly concurrent CRUD operations on a data structure, we need to have:

  1. An **RW lock** at the container level 
  2. An **RW lock** at the container element level
  3. A **reference count object** per container (prevent premature deletion of an object)

  ```c
  /* student list */
  typedef struct stud_lst
  {
      list_head *head;
      pthread_rwlock_t lst_lock;	/* RW lock at the conatainer level */
  } stud_lst_t;
  
  /* student */
  typedef struct stud_
  {
      ...
      pthread_rwlock_t stud_lock; /* RW lock at the container element level */
      ref_count_t ref_count;		/* referece count object (data structure) */
  } stud_t;
  ```

### Reference Counter Object

* The purpose of the `ref_count_t` data structure is to keep track of the number of entities (i.e., threads or data structures) in the program that are referencing (or using) the object which is being reference-counted. It is nothing but an integer variable on which you can perform only two types of operations; increment and decrement.

  For example:

  ```c
  typedef struct ref_count_
  {
      uint32_t ref_count;
      pthread_spinlock_t spinlock; /* to guarantee mutual exclusion for operations on ref_count */
  } ref_count_t;
  ```

  > Spinlock has been used instead of mutex since the critical section (simply in/decrementing `ref_count`) is very small. (Saves context-switching overhead)

* **Initialization**

  When a reference count object (e.g., `stud_t` in this example) is `malloc`'d, `ref_count` is initialized to zero.

* **Increment rules**

  1. A reference count object is inserted into the container (e.g., linked list, tree, hash tables, queues, etc.)

     $\to$ A reference count object is referenced by a **data structure**

  2. When a thread gets an access to the a reference count object

     $\to$ A reference count object is referenced by a **thread**

  3. Any other data structure in the system points to a reference count object

     $\to$ A reference count object is referenced by a **data structure**

* **Decrement rules**

  1. A reference count object is removed from a container
  2. When a thread decides not to use a reference count object anymore
  3. Any other data structure in the system stops pointing to a reference count object

* **Delete rules**

  1. A reference count object should be permanently destroyed, if ever, its `ref_count` becomes ZERO. This means that the reference count object is no longer accessible by any entities in the system and therefore it will become a leaked memory if not deleted.

  2. You should never invoke `free()` on a reference count object directly in your code, but rather strictly follow the following convention:

     ```c
     /* OK */
     if (after decrementing the value of ref_count it becomes 0)
     {
         object_release_resources(object);
         free(object); /* here 'object' is the pointer to the reference count object */
     }
     ```

     Do NOT ever do the following directly:

     ```c
     /* NOT OK */
     object_release_resources(object);
     free(object); /* here 'object' is the pointer to the reference count object */
     ```

### Implementation of Reference Count Data Structure

* **Interface for Reference Count Data Structure**

  ```c
  /*
   * File Name    : ref_count.h
   * Description  : Interface for reference count data structure
   * Author       : Modified by by Kyungjae Lee (Original: Abhishek Sagar)
   * Date Created : 01/15/2023
   */
  
  #ifndef REF_COUNT_H
  #define REF_COUNT_H
  
  #include <pthread.h>
  #include <stdint.h>
  #include <stdbool.h>
  
  typedef struct ref_count_
  {
      uint32_t ref_count;				/* can't go negative */
      pthread_spinlock_t spinlock;	/* to support provide mutual exclusion */
  } ref_count_t;
  
  void ref_count_init (ref_count_t *ref_count);
  void ref_count_inc (ref_count_t *ref_count);
  bool ref_count_dec (ref_count_t *ref_count); /* returns true if ref_count after dec is zero*/
  void ref_count_destroy (ref_count_t *ref_count);
  void thread_using_object (ref_count_t *ref_count);
  bool thread_using_object_done (ref_count_t *ref_count);
  
  #endif /* REF_COUNT_H */
  ```

* **Implementation of Reference Count Data Structure**

  ```c
  /*
   * File Name    : ref_count.c
   * Description  : Implementation of reference count data structure
   * Author       : Modified by by Kyungjae Lee (Original: Abhishek Sagar)
   * Date Created : 01/15/2023
   */
  
  #include <assert.h>
  #include "ref_count.h"
  
  void ref_count_init(ref_count_t *ref_count)
  {
      /* initialize members */
      ref_count->ref_count = 0;
      pthread_spin_init(&ref_count->spinlock, PTHREAD_PROCESS_PRIVATE);
  }
  
  void ref_count_inc(ref_count_t *ref_count)
  {
      pthread_spin_lock(&ref_count->spinlock);
      ref_count->ref_count++;
      pthread_spin_unlock(&ref_count->spinlock);
  }
  
  /* returns true if ref_count after decrement is zero*/
  bool ref_count_dec(ref_count_t *ref_count)
  {
      bool rc;
      pthread_spin_lock(&ref_count->spinlock);
      assert(ref_count->ref_count); /* ensure that ref_count never goes below zero */
      ref_count->ref_count--;
      rc = (ref_count->ref_count == 0) ? true : false; 
      pthread_spin_unlock(&ref_count->spinlock);
      return rc;
  }
  
  void ref_count_destroy(ref_count_t *ref_count)
  {
      assert(ref_count->ref_count == 0);
      pthread_spin_destroy(&ref_count->spinlock);
  }
  
  void thread_using_object(ref_count_t *ref_count)
  {
      ref_count_inc(ref_count);
  }
  
  bool thread_using_object_done(ref_count_t *ref_count)
  {
      return ref_count_dec(ref_count);
  }
  ```
  
  > Basically, the value of `ref_count` refers to the number of entities in the system that are referencing (or using) that reference count object.

### Usage of Reference Count Data Structure

* Problem statement:

  Copy the student object whose `roll_no` is 2 from the list 1 to list 2.

* **Interface for Student List**

  ```c
  /*
   * File Name    : student_list.c
   * Description  : Interface for student list
   * Author       : Modified by by Kyungjae Lee (Original: Abhishek Sagar)
   * Date Created : 01/15/2023
   */
  
  #include <stdint.h>
  #include <pthread.h>
  #include "ref_count.h"
  #include "LinkedList/LinkedListApi.h"
  
  typedef struct student_ 
  {
      uint32_t roll_no;
      uint32_t total_marks;
      ref_count_t ref_count;
      pthread_rwlock_t rw_lock;
  } student_t;
  
  typedef struct stud_lst_
  {
      ll_t *lst;
      pthread_rwlock_t rw_lock;
  } stud_lst_t;
  
  student_t* student_malloc(uint32_t roll_no);
  void student_destroy(student_t *stud);
  student_t* student_lst_lookup(stud_lst_t *stud_lst, uint32_t roll_no);
  bool student_lst_insert(stud_lst_t *stud_lst, student_t *stud);
  student_t* student_lst_remove(stud_lst_t *stud_lst, uint32_t roll_no);
  ```

* **Implementation of Student List**

  ```c
  /*
   * File Name    : student_list.c
   * Description  : Implementation of student list
   * Author       : Modified by by Kyungjae Lee (Original: Abhishek Sagar)
   * Date Created : 01/15/2023
   */
  
  #include <assert.h>
  #include "re_fcount.h"
  
  void ref_count_init (ref_count_t *ref_count)
  {
      ref_count->ref_count = 0;
      pthread_spin_init(&ref_count->spinlock, PTHREAD_PROCESS_PRIVATE);
  }
  
  void ref_count_inc (ref_count_t *ref_count)
  {
      pthread_spin_lock(&ref_count->spinlock);
      ref_count->ref_count++;
      pthread_spin_unlock(&ref_count->spinlock);
  }
  
  bool ref_count_dec (ref_count_t *ref_count)
  {
      bool rc; 
      pthread_spin_lock(&ref_count->spinlock);
      assert(ref_count->ref_count);
      ref_count->ref_count--;
      rc = (ref_count->ref_count == 0) ? true : false; 
      pthread_spin_unlock(&ref_count->spinlock);
      return rc; 
  }
  
  void ref_count_destroy (ref_count_t *ref_count)
  {
      assert(ref_count->ref_count == 0); 
      pthread_spin_destroy(&ref_count->spinlock);
  }
  
  void thread_using_object (ref_count_t *ref_count)
  {
      ref_count_inc(ref_count);
  }
  
  bool thread_using_object_done (ref_count_t *ref_count)
  {
      return ref_count_dec(ref_count);
  }
  ```

  




## References

Sagar, A. (2022). *Part A - Multithreading & Thread Synchronization - Pthreads* [Video file]. Retrieved from  https://www.udemy.com/course/multithreading_parta/

