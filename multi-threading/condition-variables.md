<a href="../">Notebook</a> > <a href="./">Multi-Threading (POSIX Threads)</a> > Condition Variables

# Condition Variables



## Introduction to Condition Variables

* A condition variable is used to allow the threads waiting in a blocking state to wake up when a particular condition is met. 

* Condition variables must be used in conjunction with a mutex lock. (Condition variables cannot be used without mutex.)

* Condition variables allow us to have finer control over the decision making on when and which competing thread to block or resume.

* Mutex cannot implement the condition-based blocking and wake up of threads. For example, mutex cannot implement such a conditional logic as "If queue is empty, wait until the queue has some elements in it.". Without condition variables, a programmer has very little control over which competing thread should go next; the lock will be granted to the thread chosen by the kernel.

  For this reason, condition variables are required.

* **Control variables** combined with **mutex** is what all it takes to implement any advanced thread synchronization schemes such as monitors, solutions to producer-consumer problem, solutions to dining philosopher problem, thread scheduler, semaphores, wait queues, barriers, etc.



## Condition Variables vs. Mutex

* Mutex **grants** a thread **an access** to a resource if the resource has not been locked already.

* Condition variables allow a thread to **inspect a resource state** and **decide if it wants to wait** until the resource turns into a favorable state. 

* Condition variables vs. mutex comparison:

  | Mutex                                                        | Condition Variables                                          |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | Access the laptop if it is not in use                        | Access the laptop only if it is not in use,<br>and if it has Internet connection |
  | Mutual exclusion ONLY                                        | Mutual exclusion + Custom condition                          |
  | pthred_mutex_lock(&laptop->mutex);<br><br>/* use laptop */<br><br>pthread_mutex_unlock(&laptop->mutex); | pthred_mutex_lock(&laptop->mutex);<br/><br>if (!laptop->internet_connection) {<br/>wait(cv, &laptop->mutex);<br>}<br>/* use laptop */<br><br>pthread_mutex_unlock(&laptop->mutex); |

  > As you can see in the code snippet above, condition variables cannot be used without mutex.

* Condition variables are not used for mutual exclusion, but they are used for **coordination** (**signaling**).

* In summary, using condition variables

  * A thread can choose to block itself until a certain condition is met. (API: `pthread_cond_wait()`)
  * A thread can signal already blocked thread (blocked by the condition variables) to resume. (API: `pthread_cond_signal()`)



<img src="./img/condition-variables.png" alt="condition-variables" width="470">





## Using Condition Variables - Wait & Signal

### Wait (Block)

* `pthread_cond_wait()` blocks the thread until the condition is signaled.

* Blocking a thread using a condition variable involves the following 2 steps:

  1. Lock a mutex
  2. Invoke `pthread_cond_wait()`

  ```c
  /* thread T1 - waiting thread */
  pthread_mutex_t mutex;
  pthread_cond_t cv;
  
  pthread_mutex_init(&mutex, NULL);
  pthread_cond_init(&cv, NULL);
  
  ...
  printf("T1 transitions to blocking state.");
  pthread_mutex_lock(&mutex);		/* step 1: lock the mutex */
  pthread_cond_wait(&cv, &mutex);	/* step 2: transition to blocking state, releasing the mutex */
  printf("T1 wakes up"); /* critical section */
  pthread_mutex_unlock(&mutex);	
  ...
  ```

  > When T1 invokes `pthread_cond_wait()`:
  >
  > 1. T1 transitions to blocking state (job of CV).
  > 2. Mutex is unlocked by the kernel behind the scenes and it is declared available.
  >
  > When T1's condition variable gets signaled:
  >
  > 1. T1 slips into **ready** state and waits for the mutex to be unlocked. 
  >
  >    Note that T1 does not immediately resume execution at this point. 
  >
  > 2. T1 automatically obtains the mutex (with the help of the kernel) as soon as T2 releases the mutex.
  >
  > 3. Only then, T1 transitions from **ready** state to **execute** state and resumes its execution.

### Signal (Resume)

* `pthread_cond_signal()` wakes up the thread that is blocked in `wait()`.

* Signaling a blocked thread using a condition variable involves the following 3 steps:

  1. Lock mutex
  2. Invoke `pthread_cond_signal()`
  3. Unlock mutex

  ```c
  /* thread T2 - signaling thread */
  pthread_mutex_lock(&mutex);		/* step 1: lock mutex */
  pthread_cond_signal(&cv);		/* step 2: signal */
  pthread_mutex_unlock(&mutex);	/* step 3: unlock mutex */
  ```

  > To be accurate, `pthread_cond_signal()` is signaling the condition variable, not the thread itself.






## References

Sagar, A. (2022). *Part A - Multithreading & Thread Synchronization - Pthreads* [Video file]. Retrieved from  https://www.udemy.com/course/multithreading_parta/

