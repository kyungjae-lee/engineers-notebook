<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Operating Systems Overview

# Operating Systems Overview



## Purpose of an Operating System



<img src="./img/operating-system-in-a-computer-system.png" alt="operating-system-in-a-computer-system" width="600">



The operating system controls the execution of application programs and acts as an interface between applications and the system hardware.

1. An OS makes a computer system more **convenient** to use.
2. An OS allows the computer's resources to be used **efficiently**.
3. An OS should be constructed in order to permit the OS to **evolve** without interfering with current services.

### 1. Convenience

The OS typically provides services in the following areas:

* **Program development**

  Provide utilities that program developers can use to access the hardware and other portions of the computer via the OS

* **Program execution**

  Load instructions, manipulate data, prepare resources

* **Access to I/O devices**

  Because devices are specialized and differ, the OS provides a uniform interface for applications to use

* **Controlled Access to Files**

* **System access**

  Similar to file access, the OS must control applications access to different parts of the system

* **Error detection and response**

  Internal/external hardware errors, forbidden memory references, etc.

* **Accounting**

  Statistics gather that enables performance monitoring, usage information

* **Various APIs**

  API provides the standard binary system call interface, designed for portability of programs across machines using the same OS. Application programming interface is a high-level language standardization of the interfaces to OS facilities.

### 2. Resource Management / Efficiency

* The OS controls all of the system resources, including:

  * I/O

  * Main and secondary memory

  * Processor execution time (CPU scheduling)

* The OS is itself software that needs to run on the CPU and use the same resources as the applications it manages.

* The Os frequently relinquishes control and must depend on the processor to allow it to regain control.

* A portion of the Os resides in main memory (the **kernel**). The rest of the main memory is made available for applications to use.



## Computer System Structure



## History of OS Design

How have operating systems evolved over the years?

### 1. Serial Processing

* No operating system - Programmers interacted directly with the computer hardware.
* Computers ran from a console with display lights, toggle switches, some form of input device, and a printer.
* Users have access to the computer in "series".
* Scheduling - Most installations used a hard copy sign-up sheet to reserve computer time.
  * Time allocations could run short or long, resulting in wasted computer time.
  * A considerable amount of time was spent just on setting up the program to run.

### 2. Simple Batch & Job Monitor

* Early computers were very expensive.
  * Important to maximize processor utilization
* Monitor
  * User no longer has direct access to processor
  * Job is submitted to computer operator who batches them together and places them on an input device
  * Program branches back to the monitor when finished
* Job monitor software lead to the creation of memory protection, privileged instructions and interrupts which, in turn,  helped create improvements in utilization in the next type of OS.
* Monitor controls the sequence of events
* Resident Monitor is software always in memory
* Monitor reads in job and gives control
* Job returns control to monitor
* Processor time alternates between execution of user programs and execution of the monitor
* Sacrifices:
  * Some main memory is now given over to the monitor
  * Some processor time is consumed by the monitor
* Despite overhead, the simple batch system improves utilization of the computer.
* **Uniprogramming**
  * I/O is slow compared to CPU-involved instructions (arithmetic)
  * The processor spends a certain amount of time executing, until it reaches an I/O instruction; it must then wait until that I/O instruction concludes before proceeding.



<img src="./img/uniprogramming.png" alt="uniprogramming" width="750">



### 3. Multiprogramming

* The development of multiprogramming in batch systems GREATLY increased resource utilization.
* Multiprogramming allows another program to execute while the first is waiting for an I/O other device request to complete.
* When the I/O task is complete, an interrupt is sent to the CPU. The monitor can then interpret the interrupt and return the first program to execution status.
* Original multiprogramming systems required all of the executing programs to reside in memory. Small memory limited the number of running programs.



<img src="./img/multiprogramming-with-three-programs.png" alt="multiprogramming-with-three-programs" width="750">



### 4. Time Sharing Systems

Because batch systems were designed to handle the execution of mainly non-interactive systems, they are not suitable for user-oriented systems.

* Time-sharing systems were developed to enable quick user response time at the sacrifice of compute time.
* This enables many people to share the system and perceive it as dedicated to themselves. It required the ability to load more applications into memory at the same time.
* Academic institutions used these systems to provide programming courses. Students used terminals to interact with the operating systems and write and test programs (BASIC).



## Modern OS Features

Critical areas of development that support modern operating systems.

1. Process

2. Memory management

3. Information protection & security

4. Scheduling and resource management
5. Fault tolerance

### 1. Process

* The concept of a process is central to the design of operating systems.
* A process contains three components:
  * An executable program
  * The associated data needed by the program (variables, work space, buffers, etc.)
  * The execution context (or "process state") of the program - this is essential
    * It is the internal data by which the OS is able to supervise and control the process.
    * Includes the contents of the various process registers
    * Includes information such as the priority of the process and whether the process is waiting for the completion of a particular I/O event.

### 2. Memory Management

* The OS has 5 principal storage management responsibilities:
  * Process isolation
  * Automatic allocation and management
  * Support of modular programming
  * Protection and access control
  * Long-term storage

### 3. Information Protection & Security

* The nature of the thread that concerns an organization will vary greatly depending on the circumstances.
* The problem involves controlling access to computer systems and the information stored in them.

### 4. Scheduling and Resource Management

* The KEY responsibility of an OS is managing resources.
* Resource allocation policies must consider:
  * Efficiency
  * Fairness
  * Responsiveness

### 5. Fault Tolerance

* Another feature of modern systems

* Fault tolerance can be supplied both by:

  * Hardware (e.g., redundant disk arrays)
  * Software (e.g., verification, sanity checks)

* The more reliable a system is, the more costly.

* The extent to which adoption of fault tolerant measures is determined by how critical the system or resource is.

  | Class               | Availability | Annual Downtime |
  | ------------------- | ------------ | --------------- |
  | Continuous          | 1.0          | 0               |
  | Fault Tolerant      | 0.99999      | 5 mins          |
  | Fault Resilient     | 0.9999       | 53 mins         |
  | High Availability   | 0.999        | 8.3 hrs         |
  | Normal Availability | 0.99 - 0.995 | 44 - 87 hrs     |

  




## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.
