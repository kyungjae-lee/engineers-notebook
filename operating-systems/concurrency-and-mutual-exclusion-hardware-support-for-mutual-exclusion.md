<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Concurrency & Mutual Exclusion - Hardware Support for Mutual Exclusion

# Concurrency & Mutual Exclusion - Hardware Support for Mutual Exclusion



## Interrupt Disabling

* In a uniprocessor (multiprogramming) system, 

  * Concurrent processes cannot have overlapped execution; they can only be interleaved. 
  * A process will continue to run until it invokes an OS service or until it is interrupted.

  Therefore, to guarantee mutual exclusion, it is sufficient to prevent a process from being interrupted.

* This capability can be provided in the form of primitives defined by the OS kernel for disabling and enabling interrupts. A process can then enforce mutual exclusion in the following way:

  ```c
  while (true)
  {
      // disable interrupts
      // critical section
      // enable interrupts
      // remainder
  }
  ```

  > Since the critical section cannot be interrupted, mutual exclusion is guaranteed.

* Drawbacks of interrupt disabling:

  * Degradation of execution efficiency due to the processor's limited ability to interleave processes.
  * Not applicable to the multiprocessor architecture (interrupt disabling does not guarantee mutual exclusion)



## Atomic Operations (Special Machine Instructions)

### Test & Set Instruction

### Compare & Swap Instruction (a.k.a Compare & Exchange)








## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.
