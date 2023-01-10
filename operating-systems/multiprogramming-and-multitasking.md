<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Multiprogramming & Multitasking

# Multiprogramming & Multitasking



Operating systems vary greatly in their makeup internally. However, two common features that every operating system must support are:

* Multiprogramming
* Multitasking (Time Sharing)



## Multiprogramming

* Multiprogramming refers to the management of multiple processes within a uniprocessor system.

* Multiprogramming increases CPU utilization by organization jobs (code and data) so that the CPU always has one to execute. Programs to be run are loaded onto the main memory in the form of processes and they are assigned CPU taking turns as necessary.

  Without multiprogramming, a single user cannot, in general, keep either the CPU or the I/O devices busy at all times. For example, even when a process occupying the CPU carries out an I/O operation, the CPU is held by that process in idle state. All other processes waiting to be executed cannot utilize the CPU until the process completes its job and releases the CPU.

* Multiprogrammed systems provide an environment in which the various system resources (e.g., CPU, memory, peripheral devices) are utilized effectively, but they **do not provide for user interaction with the computer system**.



## Multitasking (Time Sharing)

* CPU executes multiple processes by switching among them.
* Switches between multiple processes occur so fast and frequently that the **users can interact with each program** while it is running. (This wasn't possible in the multiprogramming systems.)
* Time sharing requires an interactive (or hands-on) computer system, which provides direct communication between the user and the system.
* A time-shared operating system **allows many users to share the computer simultaneously**. And gives each user an illusion that the entire system belongs to him/her.
* A time-shared operating system uses CPU scheduling and multiprogramming to provide each user with a small portion of a time-shared computer.
* Each user has at least one separate process in main memory.
* A program loaded into memory and executing is called a **process**.








## References

Joshy, J. (2021). *Operating System* [Video file]. Retrieved from https://www.nesoacademy.org/cs/03-operating-system
