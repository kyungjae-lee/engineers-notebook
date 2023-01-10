<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > System Calls

# System Calls



## Introduction to System Calls

* To understand the system calls, let's understand the two modes of operation in which a program can execute first.

  * **User mode**

    A program running in user mode does not have the direct access to the memory, hardware, and such other resources. Therefore, even if a program running in user mode crashes, the entire system does not crash. It is a safe mode, and most of the programs execute in user mode.

  * **Kernel mode (Privileged mode)**

    A program running in kernel mode has the direct access to the memory, hardware, and such other resources. Therefore, if a program running in kernel mode crashes, it is likely that the entire system will crash and come to a halt. (Big problem!)

  A programming running in user mode may need an access to some of the resources managed by the kernel. In such cases, the user mode program **signals (makes a system call to)** the operating system, (context) switch to kernel mode temporarily so that it can use the resources and (context) switch back to user mode when done.

* System calls provide an interface to the services made available by an operating system. System call is the programmatic way in which a computer program requests a service from the kernel of the operating system.

* System calls are generally available as routines written in C and C++.

* Even a simple task may involve a lot of system calls. There are thousands of system calls that are executed per second during the execution of certain program in your system.






## References

Joshy, J. (2021). *Operating System* [Video file]. Retrieved from https://www.nesoacademy.org/cs/03-operating-system
