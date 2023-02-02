<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Mutual Exclusion - Software Emulation

# Mutual Exclusion - Software Emulation



## Introduction

* Software approaches can be implemented for concurrent processes that execute on a single-processor or a multiprocessor machine with shared main memory. These approaches usually assume elementary mutual exclusion at the memory access level. That is, simultaneous access (reading and/or writing) to the same location in main memory are serialized by some sort of memory arbiter, although the order of access granting is not specified ahead of time. Beyond this, no support in the hardware, operating system, or programming language is assumed.



## Dekker's Algorithm

* An algorithm for mutual exclusion for two processes, designed by the Dutch mathematician Dekker.

* **Description of the algorithm** (for two processes)

  When P0 wants to enter its critical section, it sets its flag to `true`. It then checks the flag of P1. If that is `false`, P0 may immediately enter its critical section. Otherwise, P0 consults `turn`. If P0 finds that `turn = 0`, then it knows that it is its turn to insist and periodically checks P1's flag. P1 will at some point note that it is its turn to defer and set its flag to `false`, allowing P0 to proceed. After P0 has used its critical section, it sets its flag to `false` to free the critical section, and sets `turn` to 1 to transfer the right to insist to P1.

* **Algorithm in code**

  ```c
  // Dekker's algorithm for two processes
  
  boolean flag[2];
  int turn;
  
  void p0()
  {
      while (true)
      {
          flag[0] = true;
          while (flag[1])
          {
              if (turn == 1)
              {
                  flag[0] = false;
              	while (turn == 1);	// do nothing
              	flag[0] = true;
              }
          }
          // critical section
          turn = 1;
          flag[0] = false;
          // remainder
      }
  }
  
  void p1()
  {
      while (true)
      {
          flag[1] = true;
          while (flag[0])
          {
              if (turn == 0)
              {
                  flag[1] = false;
              	while (turn == 0);	// do nothing
              	flag[1] = true;
              }
          }
          // critical section
          turn = 0;
          flag[1] = false;
          // remainder
      }
  }
  
  void main()
  {
      flag[0] = false;
      flag[1] = false;
      turn = 1;
      parbegin(p0, p1);
      // the construct 'parbegin(p1, p2, ... , Pn)' means, suspend the execution of the main program;
      // initiate concurrent execution of procedures P1, P2, ... , Pn;
      // when all of P1, P2, ... , Pn have terminated, resume the main program
  }
  ```



## Peterson's Algorithm

* Peterson has provided a simpler, more elegant solution for mutual exclusion.

* **Description of the algorithm** (for two processes)

  Consider process P0. Once it has set `flag[0]` to `true`, P1 cannot enter its critical section. If P1 is already in its critical section, then `flag[1] = true` and P0 is blocked from entering its critical section. On the other hand, mutual blocking is prevented. Suppose that P0 is blocked in its `while` loop. This means that `flag[1]` is true and `turn = 1`. P0 can enter its critical section when either `flag[1]` becomes `false` or `trun` becomes 0.

  Now, consider three exhaustive cases:

  1. P1 has no interest in its critical section. This case is impossible, because it implies `flag[1] = false`.
  2. P1 is waiting for its critical section. This case is also impossible, because if `turn = 1`, P1 is able to enter its critical section.
  3. P1 is using its critical section repeatedly and therefore monopolizing access to it. This cannot happen, because P1 is obliged to give P0 an opportunity by setting `turn` to 0 before each attempt to enter its critical section.

  Thus, we have a simple solution to the mutual exclusion problem for two processes. This algorithm can easily be generalized to more than two processes.

* **Algorithm in code**

  ```c
  // Peterson's algorithm for two processes
  
  boolean flag[2];
  int turn;
  
  void p0()
  {
      while (true) 
      {
          flag[0] = true;
          turn = 1;
          while (flat[1] && turn == 1);	// do nothing
          // critical section
          flag[0] = false;
          // remainder
      }
  }
  
  void p1()
  {
      while (true)
      {
          flag[1] = true;
          turn = 0;
          while (flat[0] && turn == 0);	// do nothing
          // critical section
          flag[1] = false;
          // remainder
      }   
  }
  
  void main()
  {
      flag[0] = false;
      flag[1] = false;
      parbegin(p0, p1);
      // the construct 'parbegin(p1, p2, ... , Pn)' means, suspend the execution of the main program;
      // initiate concurrent execution of procedures P1, P2, ... , Pn;
      // when all of P1, P2, ... , Pn have terminated, resume the main program
  }
  ```

  




## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.