<a href="../">Notebook</a> > <a href="./">Embedded Systems Programming (ARM)</a> > Implementing a Scheduler

# Implementing a Scheduler



## Introduction

* Implement a scheduler which schedules multiple user tasks (we will use 4 tasks) in a Round-Robin fashion by carrying out the context switch operation. (Round-Robin scheduling algorithm - Time slices are assigned to each task in equal portions and in circular order.)
* Use SysTick handler to carry out the context switch operation between multiple tasks (Later update the code using PendSV for context switching)
* What is a task?
  * A task is nothing but a piece of code, or you can call it a "C function", which does a specific job when it is allowed to run on the CPU.
  * **A task has its own stack** to create its local variables when it runs on the CPU. Also, when the scheduler decides to remove a task from CPU, scheduler first saves the context (state) of the task in task's private stack.
  * In summary, a piece of code or a function is called a task when it is schedulable and never loses its 'state' unless it is deleted permanently.



## Implementation

* **Create 4 user tasks** (i.e., 4 never returning C functions)

* **Stack point selection**

  * MSP - Handler mode (Scheduler; `SysTick_Handler()` / `PendSV_Handler()` )
  * PSP - Thread mode (User tasks)

  Our program will use both MSP and PSP for stack utilization.

* **Stack assessment**

  STM32F407 microcontroller board has 128 KB (SRAM1 + SRAM2) of RAM.

  We will use 5 KB of stack memory, each 1 KB of which will be assigned to each task and a scheduler. (4 tasks + 1 scheduler).

  How much stack to assign is completely dependent on your application design, the number of APIs to be called from the stack and so on.

  ```plain
  RAM_END     +-------------------+ T1_STACK_START
              | Private stack T1  |
              +-------------------+ T1_STACK_END = T2_STACK_BEGIN
              | Private stack T2  |
              +-------------------+ T2_STACK_END             
              | Private stack T3  |
              +-------------------+ 
              | Private stack T4  |
              +-------------------+ 
              |     Scheduler     |
              +-------------------+
              |                   |
              |        ...        |
              |                   |
  RAM_START   +-------------------+
  
  ```

* **Reserve stack areas for all the tasks and scheduler**

* **Scheduling policy selection**

  * Round-Robin preemptive scheduling
  * No task priority
  * Use SysTick timer to generate exception for every 1 ms to run the scheduler code

  > Scheduling
  >
  > * An algorithm which makes the decision of preempting a running task from the CPU and makes the decision about which task to dispatch (i.e., allocate CPU) next
  > * The decision could be based on many factors such as system load, the priority of tasks, share resource access, or a simple Round-Robin method.
  >
  > Context switching
  >
  > * The procedure of switching out the currently running task from the CPU after saving the task's execution context or state and switching in the next task's to run on the CPU by retrieving the past execution context or state of the task.
  >
  > State of a task
  >
  > * Inside a **microcontroller** (e.g., STM32x) is a **processor** (e.g., ARM Cortex-M4) which contains NVIC, MPU, SCP, FPU, Debug Unit, etc.
  >
  >   Inside a processor is a **processor core** which contains ALU, Core registers (General purpose, special purpose, special registers, etc.), etc. 
  >
  > * When a task is running, there will be various resources associated with it that contains the information about the task. In general, the state of a process = General purpose registers + Some special purpose registers + Status register. These are what need to be stored and retrieved during the context switching.
  >
  >   * General purpose registers
  >
  >   * PC - Will be holding the next instruction the preempted task needs to execute when it is dispatched again.
  >
  >   * LR - Return address
  >
  >   * PSP - Stack pointer for the user tasks (contains information about how each task is using the stack)
  >
  >   * PSR - Snapshot of the status flags (N, V, Z, C, etc.)
  >
  >   * Other special registers such as PRIMASK, FAULTMASK, BASEPRI, CONTROL registers are privileged registers and they will NOT be stored! (Most of the time user tasks will be running with unprivileged access level so storing these registers wouldn't make any sense. Kernel should touch these registers not the user tasks.)
  >
  > 
  >
  >   <img src="./img/state-of-a-user-task.png" alt="state-of-a-user-task" width="700">
  >
  > 
  >
  > * Case study of context switching: T1 switching out, T2 switching in
  >
  >   1. Running T1
  >   
  >   2. Scheduler (Context Switching)
  >   
  >      1. Context saving 
  >   
  >         * `push` the context of T1 onto T1's private stack
  >   
  >         * Save the PSP value of T1 using a global variable
  >   
  >      2. Context retrieving 
  >   
  >         * Retrieve the PSP value of T2 from the corresponding global variable
  >   
  >         * `pop` the context of T2 from T2's private stack
  >   
  >   3. Run T2
  >   
  > * As an exception entry sequence, the following registers will get pushed onto stack automatically by the processor:
  >
  >   * R0, R1, R2, R3, R12, LR, PC, xPSR (we'll call it Stack Frame 1 or SF1)
  >
  >   So, our job in implementing scheduler is to make sure that the rest of the task state will get pushed onto stack as well.
  >
  >   * R4 - R11 (we'll call it Stack Frame 2 or SF2)
  >
  > * Task's stack area initialization and storing of dummy stack frame
  >
  >   * Each task can consume a maximum of 1 KB of memory as a private stack.
  >
  >   * This stack is used to hold tasks' local variables and context (SF1 + SF2)
  >
  >   * When a task is getting scheduled for the very first time, it doesn't have any context. So, it is the programmer's responsibility to store dummy SF1 and SF2 in the task's stack area as a part of "Task initialization" sequence before launching the scheduler.
  >
  >     * Set all the general purpose registers to 0 (R0 - R12)
  >
  >     * PSR - 0x01000000
  >
  >       Only the T bit is important (bit[24]) and it must be set (1) for ARM Cortex-M4
  >
  >     * PC - Address of the `task2_handler()` make sure that lsb of the address is 1; for T bit.
  >
  >     * LR - A special value EXC_RETURN which controls the exception exit.
  >
  >       0xFFFFFFFD is appropriate for our design. (Return to Thread mode, exception return uses non-floating-point state from the PSP and execution uses PSP after return.)
  
* **Configure the SysTick timer to produce exception every 1 ms.**

  * Processor Clock = 16 MHz

  * SysTick timer count clock = 16 MHz

  * 1 ms is 1 KHz in frequency domain (in 1 sec, a thousand context switches will happen)

  * So, to bring down SysTick timer count clock from 16 MHz to 1 KHz use a divisor (i.e., reload value)

    1 KH is the TICK_HZ (desired exception frequency)

    $\therefore$ Reload value = 16000


* **Initialize scheduler stack pointer (MSP)**
  * To secure MSP we need, naked function will be used for initialization.
* **Initialize tasks' stack memory**
  * Store dummy SF1 and SF2

* (After the first attempt to control 4 LEDs with x4 elongation) **Introducing blocking state for tasks**.
  * When a task has got nothing to do, it should simply call a delay function which should put the task into the blocked state from running state until the specified delay is elapsed.
  * We should now maintain 2 states for a task; RUNNING an BLOCKED.
  * The scheduler should schedule ONLY those tasks which are in RUNNING state.
  * The scheduler also should unblock the blocked tasks if their blocking period is over and put them back to running state.





## References

Nayak, K. (2022). *Embedded Systems Programming on ARM Cortex-M3/M4 Processor* [Video file]. Retrieved from  https://www.udemy.com/course/embedded-system-programming-on-arm-cortex-m3m4/
