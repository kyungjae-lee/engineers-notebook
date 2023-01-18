<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > The Process

# The Process



## Introduction to the Process

* All modern multi-programming operating systems, including single-user desktops and multi-user mainframes, are built around the concept of **the process**.

* A process supports the goal of an OS as a resource manager.

  Recap the OS goals from earlier sections:

  1. Maximize processor utilization
  2. Provide reasonable response times
  3. Fairness
  4. Avoid failure
  5. Support development
  6. Be able to enhance with new devices and features

* Process supports some goals especially in the area of utilization, fairness, and throughput.

  1. Operating system must interleave execution of multiple processes.
  2. Operating system must allocate resources to processes in a way that is fair and avoids deadloccks.
  3. Operating system must support user creation of processes.
  4. Operating system must support inter-process communication.



## What is a Process?

* All modern operating systems rely on a concept in which the execution of an application corresponds to the existence of one or more processes.

* A process is comprised of:

  - A program in execution (the program's code)
  - Associated data in memory
  - The process control block (PCB)

  **Process image** -  The collection of program instructions, data, stack, and attributes required to store the state of a process.



## Process Control Block (PCB)

* The PCB is a data structure created by an managed by the operating system software.

* Contains sufficient information about a process so it is possible to interrupt a running process and resume its execution as if the interruption did not happen.

* This supports multi-programming:

  - When a process is interrupted, current values related to the process' current status are saved (context)
  - The process is moved to a non-running state
  - The OS is free to choose another PCB to load and resume the execution of a different process

* Typical Contents of a PCB

  | Contents        | Description                                                  |
  | --------------- | ------------------------------------------------------------ |
  | Identifier      | Unique ID to distinguish from other processes                |
  | State           | The current state (e.g., ready, running, blocked, new, exit) |
  | Priority        | Level of priority with respect to other processes            |
  | Program Counter | Address of next instruction to load and execute in the program code |
  | Memory Pointers | Pointers to code, data and share data                        |
  | Context Data    | Current values of system registers and other hardware flags  |
  | I/O Status      | Pending I/O requests, devices dedicated to process, list of files in use |
  | Accounting Data | Processor time used, time limits, user info, etc.            |

* Process Details Stored in or Linked to PCB

  * **Process location**

    How much physical memory does it need, what portion of the process image is actually loaded into real memory.

  * **Process attributes**

    - ID + user, owner
    - State information - Registers, stack pointers, hardware flags
    - Control information - State, priority, event identities, data pointers, communication buffers, privileges, resource holds in use (files, etc), memory locations



## Control Structure

* What information does the OS need in order to control the execution of process?
* Much information is dynamic and sizes differ. For example, a process may need 1 file or a 100 files, and this can be unpredictable in nature.
* There are 4 main categories of detailed control structure information that the OS needs to maintain in addition to the PCB for each process.
  1. Memory Tables
     - Allocation of main memory to processes
     - Allocation of secondary memory to processes
     - Protection attributes of the memory blocks
     - Additional information for managing virtual memory
  2. I/O Tables
     - Devices available or assigned to processes
     - Status of I/O operations in progress
     - Memory locations used for data transfers
  3. File Tables
     - File status
     - Location on secondary memory
  4. Process Tables
     - OS storage for information about each process, including PCB



## Process Modes

* Sometimes a user process needs to execute privileged instructions, such as accessing a device or a password system.

* Processors and OS support different modes of execution for this purpose.

  * **User Mode**

    A process is executing normally.

  * **Kernel Mode**

    A process is executing some instructions via the OS that are privileged. Often, when a process calls an OS utility, it enters kernel mode to execute the instructions, and returns to user mode when the operation exists.

* The current mode is stored in the PDB.





<img src="./img/tbd.png" alt="tbd" width="600">










## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.
