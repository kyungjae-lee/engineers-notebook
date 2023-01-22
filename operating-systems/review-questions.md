<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Review Questions

# Review Questions



## Computer System Overview

* **What is an interrupt? How do operating systems detect them?**

  An interrupt is a mechanism by which other modules (I/O, memory) may interrupt the normal sequencing of the processor. The process checks for an interrupt flag at the end of each fetch-execute cycle to determine if something needs to be dealt with.

* **What are the benefits of organizing memory in a hierarchy?**

  Cache memory is a memory that is smaller and faster than main memory and that is interposed between the processor and main memory. The cache acts as a buffer for recently used memory locations. There can be multiple caches, the faster and closer to the CPU the more costly it is. Having a hierarchy of memory allows the hardware to assist in speeding up memory accesses without the CPU having to intervene as much, as well as spread out the cost.

* **What is the difference between a multiprocessor and a multicore system?**

  A multicore computer is a special case of a multiprocessor, in which all of the processors are on a single chip.

* **What does it mean to say that a process/program is I/O bound?**

  The process spends a much greater fraction of its time waiting for I/O instead of computing instructions on the CPU/ALU.

* **What does it mean to say that a process/program is CPU bound?**

  The process spends a much greater fraction of its time computing instructions than waiting for I/O.

* **Consider the following code segment, and give one example of spatial locality in the code:**

  ```c
  for (i = 0; i < 20; i++)
  	for (j = 0; j < 10; j++)
          arr[i] = arr[i] * j;
  ```

  Spatial locality refers to the tendency of execution to involve a number of memory locations that are clustered. In the above code, while looping the outerloop, the accesses to elements in array `arr` are physically close to each other in memory. This is a characteristic of contiguously stored data structures such as arrays.



## Operating System Overview

* **What is multiprogramming?**

  Multiprogramming is the method of allowing more than one program to be interleaving their execution on a single processor.

* **What is multiprocessing?**

  Multiprocessing is the method of allowing the physical computation of processes in parallel, on different processors or computing hardware.

* **Explain the difference between a monolithic kernel and a microkernel design.**

  The kernel is a portion of the operating system that includes the most heavily used portions of software. Generally, the kernel is maintained permanently in main memory.

  Microkernel implements the most important core features in the memory resident portion of the operating system whereas Monolithic kernels include all possible OS features in the core. Microkernels use memory more efficiently by allowing some operating system features to be managed as if they were normal processes and can be swapped in/out of memory. This makes it easier to change/add-to/update an operating system.



## The Process

* **What does it mean to preempt a process?**

  Answer

* **What is swapping and what is its purpose?**

  Answer

* **What is the difference between a mode switch and a process switch?**

  Answer

* **Assume that at time 5, no system resources are being used except for the processor and memory. Now consider the following events:**

  At time 5: P1 executes a command to read from disk unit 3
  At time 15: P5’s time slice expires
  At time 18: P7 executes a command to write to disk unit 3
  At time 20: P3 executes a command to read from disk unit 2
  At time 24: P5 executes a command to write to disk unit 3
  At time 28: P5 is swapped out
  At time 33: An interrupt occurs from disk unit 2; P3’s read is complete
  At time 36: An interrupt occurs from disk unit 3; P1’s read is complete
  At time 38: P8 terminates
  At time 40: An interrupt occurs from disk unit 3; P5’s write is complete
  At time 44: P5 is swapped back in
  At time 48: An interrupt occurs from disk unit 3; P7’s write is complete

  **For each time 22, 37 and 47, identify which state each process is in. If a process is blocked, indicate why it is blocked.**

  Answer

* **Consider the 7-state transition diagram. Suppose it is time for the OS to dispatch a process and there are processes in both the Ready State and the Ready/Suspend state. At least one process in the Ready/Suspend state has higher priority than any of the Ready processes. Two extreme policies are as follows:**

  **a. always dispatch from a process in the Ready State to minimize swapping**

  **b. always give prefrence to the highest priority process even though that may mean swapping**

  **Suggest an intermediate policy that tries to balance the concerns of priority and performance.**

  Answer

* **Consider the 7-state transition diagram. It is the theoretically possible to have a transition between any two states. For example, the transition from READY to RUNNING is the dispatch action. What are the impossible transitions between states, and explain each.**

  Answer






## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.
