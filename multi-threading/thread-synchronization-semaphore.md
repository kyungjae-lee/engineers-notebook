<a href="../">Notebook</a> > <a href="./">Multi-Threading (POSIX Threads)</a> > Thread Synchronization - Semaphore

# Thread Synchronization - Semaphore



## Introduction to Semaphores

* Proposed by Edsger Dijkstra, technique to manage concurrent threads by using a simple integer value which is known as a semaphore.

* A sempahore allows $n$ ($n\ge1$) threads to enter a critical section and execute concurrently. From the functionality's perspective, a mutex is a special case of semaphore where $n$ is 1. This is why mutexes are also called as **binary semaphores**.

  Here, $n$ is referred to as the **permit number**. Permit number is defined by the user.

* We have an assumption that the critical section protected by a semaphore is of a special property that it can be executed by $n$ number of execution units (i.e., threads) concurrently without causing any problems.






## References

Sagar, A. (2022). *Part A - Multithreading & Thread Synchronization - Pthreads* [Video file]. Retrieved from  https://www.udemy.com/course/multithreading_parta/

