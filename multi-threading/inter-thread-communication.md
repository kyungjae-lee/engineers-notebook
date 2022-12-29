<a href="../">Notebook</a> > <a href="./">Multi-Threading (POSIX Threads)</a> > Inter-Thread Communication

# Inter-Thread Communication



A big software system may be a multi-threaded software where multiple modules are running as independent threads of the same process and the threads may need to exchange data with one another. In this case, we need to set up communication between threads (i.e., exchange of data). 

**Inter-Process Communication (IPC)**, the techniques used to set up the data exchange between processes, can also be used for the communication between threads. The followings are the tools used to set up the IPC:

* Sockets
* Message queues
* Pipes
* Shared Memory

However, for **Inter-Thread Communication**, using **callbacks** (or **function pointers**) is preferred to using IPC techniques. This is because callbacks are:

* Very fast
* No actual transfer of data, but only transfer of computation
* No special attention required from the kernel or the OS because they are nothing but the user-space function calls. 
  * Everything is done in the user-space and hence, no kernel resource is required to set up the communication.





## References

Sagar, A. (2022). *Part A - Multithreading & Thread Synchronization - Pthreads* [Video file]. Retrieved from  https://www.udemy.com/course/multithreading_parta/
