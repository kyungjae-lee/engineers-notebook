<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Mutual Exclusion - Semaphores (OS Support)

# Mutual Exclusion - Semaphores (OS Support)



## Introduction

* In order to implement more modular, sophisticated waiting strategies, that eliminate busy-wait and starvation, we need structures that the OS can be involved with. We can allow the OS to assist with wait times using the state-transitions and events rather than relying on processes to idle in inefficient/costly busy wait loops.
* Semaphore, proposed by Edsger Dijktra, is a technique to manage concurrent process by using a special variable type (i.e., **semaphore**) with an integer value. 
* Semaphores are used with shared memory systems, processes can each utilize the shared semaphores.
* Both OS and high-level languages provide support for the semaphore in code.

### Semaphore types

* Depending on the maximum number of processes (or threads) allowed to enter the critical section:

  * **Counting/general semaphore** -  >= 1

  * **Binary semaphore** - 1 (special case of counting/general semaphore)

* Depending on the order in which the processes blocked on the semaphore get moved to the ready queue:

  * **Strong semaphore** - FIFO (fair, starvation-free)

  * **Weak semaphore** - Unspecified order (may suffer from starvation)

  [!] Note: Semaphores each have a queue of waiting (blocked) processes



## Semaphore

* **Semaphore Operations**

  * **Initialize**
    * A semaphore may be initialized to a non-negative integer
    * Binary semaphores start with 1, counting semaphores >= 1

  * **Wait**
    * Decrements the semaphore value as an **atomic operation**
    * If the value becomes negative the process issuing the wait becomes blocked (not a busy wait)
    * Otherwise, the process has gained control of the semaphore and continues its execution

  * **Signal**
    * Increments the semaphore value as an **atomic operations**
    * If the value is still <= 0, then a blocked process that was waiting for the semaphore will be unblocked so it can proceed into the critical section that is under protection

  > A binary semaphore with value 1, means the resource is available
  >
  > A counting semaphore with a value of N means that N instances of the resource are available, or N more processes can enter the critical section
  >
  > If the semaphore value is 0 then the next process will have to block
  >
  > If the semaphore value is < 0, it represents the number of blocked process

* **A Definition of Counting/General Semaphore Primitives**

  ```c
  struct semaphore
  {
      int count;
      queue_type queue;
  }
  
  void sem_wait(semaphore s)
  {
      s.count--;
      if (s.count < 0)
      {
          // place this process in s.queue
          // block this process
      }
  }
  
  void sem_signal(semaphore s)
  {
      s.count--;
      if (s.count <= 0)
      {
          // remove a blocked process from s.queue and place it on the ready queue
      }
  }
  ```

* **A Definition of Semaphore Primitives**

  ```c
  struct binary_semaphore
  {
      enum {ZERO, ONE} value;
      queue_type queue;
  }
  
  void bsem_wait(semaphore s)
  {
      if (s.value == 0)
          s.value = ZERO;
      else
      {
          // place this process in s.queue
          // block this process
      }
  }
  
  void bsem_signal(semaphore s)
  {
      if (s.queue is empty())
          s.value = ONE;
      else
      {
          // remove a blocked process from s.queue and place it on the ready queue
      }
  }
  ```

  > A binary semaphore just checks for 0 or 1 rather than incrementing and decrementing.

* **Mutual Exclusion Using Semaphores**

  ```c
  // mutual exclusion using a semaphore
  const int n = /* number of processes */;
  semaphore s = 1;	// semaphores must be initialized upon creation
  
  void p(int i)
  {
      while (true)
      {
          sem_wait(s);
          // critical section
          sem_signal(s);
          // remainder
      }
  }
  
  void main()
  {
      parbegin(p(1), p(2), ... , p(n));
      // the construct 'parbegin(p1, p2, ... , Pn)' means, suspend the execution of the main program;
      // initiate concurrent execution of procedures P1, P2, ... , Pn;
      // when all of P1, P2, ... , Pn have terminated, resume the main program
  }
  ```
  
  






## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.
