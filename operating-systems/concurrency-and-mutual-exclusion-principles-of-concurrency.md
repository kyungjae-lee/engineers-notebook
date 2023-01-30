<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Concurrency & Mutual Exclusion - Principles of Concurrency

# Concurrency & Mutual Exclusion - Principles of Concurrency



## Introduction

* Operating systems are concerned with the management of processes and threads.

  * **Multiprogramming** - the management of multiple processes within a <u>uniprocessor</u> system
  * **Multiprocessing** - the management of multiple processes within a <u>multiprocessor</u> system
  * **Distributed processing** - the management of multiple processes executing on multiple, distributed computer systems (e.g., clusters)

  Concurrency is fundamental to all of these types of computing.

* Concurrency arises in different contexts:
  * **Multiple applications** - multiprogramming was invented to handle this, and allow processing time to be share among multiple active applications
  * **Structured applications** - modular design sometimes encompasses developing problem solutions built from multiple concurrent processes
  * **Operating system structure** - OS services can be developed as a set of concurrent processes as well
* An integral requirement to programming with concurrency is the ability to enforce <u>mutual exclusion</u>.



## Key Terms Related to Concurrency

* **Atomic operation**

  An action implemented as an uninterpretable instruction or function.

* **Critical section**

  A section of code in a process that requires access to shared resources and must not be executed when another process is in a corresponding section of code.

* **Deadlock**

  A situation in which two or more processes are unable to proceed because each is waiting for the other to finish.

* **Livelock**

  A situation in which two or more processes continuously change their states in response to the other process(es) without doing any useful work.

* **Mutual exclusion**

  The requirement that when one process is in a critical section that accesses shared resources, no other process may be in a critical section that accesses any of those shared resources.

* **Race condition**

  A situation in which multiple threads or processes read and write a shared data item, and the final result depends on the relative timing of their execution. For example,

  ```c
  int count = 10;	// shared variable
  ```

  ```c
  // producer thread 
  count++;
  
  // internal operation
  // 1. register1 = count (LOAD)
  // 2. register1 = register1 + 1
  // 3. count = register1 (STORE)
  ```

  ```c
  // consumer thread
  count--;
  
  // internal operation
  // 1. register2 = count (LOAD)
  // 2. register2 = register2 - 1
  // 3. count = register2 (STORE)
  ```

  > Depending on how the instructions of producer and consumer gets interleaved, the resultant value of `count` may be 9, 10, or 11.

* **Starvation**

  A situation in which a runnable process is overlooked indefinitely by the scheduler; although it is able to proceed, it is never chosen.



## Process Interaction

* We can classify the way s in which processes interact on the basis of the degree to which they are aware of each other's existence.

  | Degree of Awareness                                          | Relationship                 | Influence that One Process Has on the Other                  | Potential Control Problems                                   |
  | ------------------------------------------------------------ | ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | Processes unaware of each other                              | Competition                  | Results of one process independent of the action of others<br><br>Timing of process may be affected | Mutual exclusion<br><br>Deadlock (renewable resource)<br><br>Starvation |
  | Processes indirectly aware of each other (e.g., shared object) | Cooperation by sharing       | Results of one process may depend on information obtained from others<br><br>Timing of process may be affected | Mutual exclusion<br/><br/>Deadlock (renewable resource)<br/><br/>Data coherence |
  | Processes directly aware of each other (have communication primitives available to them) | Cooperation by communication | Results of one process may depend on information obtained from others<br/><br/>Timing of process may be affected | Deadlock (renewable resource)<br/><br/>Starvation            |



## Requirements for Mutual Exclusion

1. Mutual exclusion must be enforced - only one process at a time is allowed into its critical section, among all processes that have critical sections for the same resource or shared object.
2. A process that halts in its noncritical section must do so without interfering with other processes.
3. It must not be possible for a process requiring access to a critical section to be delayed indefinitely - no deadlock or starvation.
4. A process must not be denied or delayed access to a critical section when no other processes are using.
5. No assumptions are made about relative process speeds or number of processors.
6. A process remains inside its critical section for a finite amount of time only.






## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.