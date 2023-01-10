<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Operating System services

# Operating System Services



An OS provides an environment for the execution of programs. It provides certain services to programs and to users of those programs.

* User interface
* Program execution
* I/O Operations
* File system management (manipulation)
* Resource allocation
* Accounting
* Protection and security



## User Interface

* Almost all operating systems support user interface and the user interface allows a user to interact with an operating system or a computer itself.

* Two types of user interface:

  * **Command Line Interface (CLI)**

    Some operating systems include the command interpreter as a part of the kernel. Others, such as Windows and UNIX, treat the command interpreter as a special program. 

    On systems with multiple command interpreters to choose from, the interpreters are known as **shells**.

    * e.g., Bourne shell, C shell, Bourne-Again shell (BASH), Korn shell, etc.

    A command interpreter is a program that allows users to directly enter commands that are to be performed by the kernel. There are two approaches in which the command interpreter executes a task:

    1. The code for performing the task is implemented inside the command interpreter itself.
    2. The code for performing the task exists as a separate program outside the command interpreter. In this case, the command interpreter will run the program corresponding to the command the user entered.

  * **Graphical User Interface (GUI)**

    Uses icons, menus and a mouse (to click on the icon or pull down the menus) and so on to manage the user interaction with the computer system.



## Program Execution

* An operating system must be able to load the program into memory, and run the program.



## I/O Operations

* A user cannot directly control the input/output devices. An operating system must control the usage of the I/O devices between the user and the device.
* Input devices:
  * Keyboard
  * Mouse
  * Microphone
  * Camera
* Output devices:
  * Monitor
  * Printer
  * Speaker



## File System Management (Manipulation)

* An operating system must be able to manage the files in the system such as:

  * Create, move, copy, search, delete files (or directories)
  * Control access (permission)

  

## Communication

* An operating system must support the communication between processes that may be running on the same computer or different computers connected by the network.
* Communication between processes is necessary for the processes to synchronize with each other and execute in an efficient way.



## Error Detection

* Errors can always occur in the course of computing, and an operating system must be constantly aware of the possible errors. (e.g., Errors occur in the CPU, memory, I/O devices, network, etc.)
* An operating system must also be able to manage those errors so that your computing is consistent and it is still carried on even if some error are encountered.



## Resource Allocation

* An operating system must be capable of allocating resources to different processes (or users) in an efficient way such that all the processes get the resources they need and no process keeps waiting for the resource and never gets it.
* Resource here can be CPU, files, I/O devices, main memory, etc.



## Accounting

* An operating keeps track of which user use how much and what kind of computer resources.
* This information is for accounting or accumulating usage statistics which can help researchers that wish to reconfigure the system or to improve the computing services.



## Protection & Security

* Our data should be secure and everything we do on a computer system must be protected.
* In terms or processes, the protection means that access to the system resources must be controlled. It should not be possible for one process to interfere with the others or with the operating system when several different processes are executing simultaneously.
* In terms of outside access, the protection means the system is not accessible to outsiders who are not allowed to access the system.






## References

Joshy, J. (2021). *Operating System* [Video file]. Retrieved from https://www.nesoacademy.org/cs/03-operating-system
