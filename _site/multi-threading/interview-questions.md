<a href="../">Notebook</a> > <a href="./">Multi-Threading (POSIX Threads)</a> > Interview Questions

# Interview Questions



* Explain the difference between concurrent threads and parallel threads.

* An embedded system is running 2 threads on 2 cores (CPUs). What should be the condition the two threads must meet to call them:

  a. Concurrent threads

  b. Parallel threads

* Explain any typical process/application design which has to be multi-threaded to accomplish its functionality.

* A process P is a multi-threaded process which has two threads. Process P has two arrays each of which has 1 million integers. The two threads have a responsibility to find the sum of each array. The two threads is assigned their respective array for computation. 

  Is this computation concurrent or parallel under each of the following constraints:

  a. System has 2 CPUs, and no other thread is present in the system other than these two threads.

  b. System has 1 CPU, and no other thread is present in the system other than these two threads.

  If we change the responsibility of the worker threads, that instead of finding the sum, they have to find the largest number present in the two arrays combined. Then is the computation concurrent or parallel if:

  a. System has 2 CPUs, and no other thread is present in the system other than these two threads.

  b. System has 1 CPU, and no other thread is present in the system other than these two threads.

* Consider a doubly linked list containing 1 million integers as `node->data`. A process P is created to replace the data in all nodes of the linked list with the squared of the integers. 

  For example, if the list is: 2 4 5 7 8 9

  Final list should be: 4 16 25 49 64 81

  A process P fork two threads to solve this problem. Assign the responsibility to the two threads so that threads can be independently executed on different processors of the system without having a need for any coordination. 

  Assume that the number of nodes in the linked list is already known, say, N.

  Assume that the system has at least 2 CPUs, and no other thread is running on the system.

* Which of the following is/are true? 

  a. T1 and T2 are said to be concurrent threads if they execute only 1 at a time on a uni-processor system

  b. Very fast context switching creates an illusion to human as if concurrent threads are executing in parallel

  c. Concurrency refers to things that appear to happen at the same time, but actually not!

  d. Concurrency must ensure independent progression





## References

Sagar, A. (2022). *Part A - Multithreading & Thread Synchronization - Pthreads* [Video file]. Retrieved from  https://www.udemy.com/course/multithreading_parta/
