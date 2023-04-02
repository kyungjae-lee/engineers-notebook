<a href="../">Notebook</a> > <a href="./">Real-Time Operating Systems (RTOS)</a> > Overview of FreeRTOS Memory Management

# Overview of FreeRTOS Memory Management



## RAM & Flash

* In general, MCUs contain RAM and Flash. And, Flash is usually bigger in size than RAM.

### RAM

* Used to store the application **data**. (e.g., Global arrays, global variables, etc.)
* The **code** (i.e., instructions) can be downloaded and run on RAM. (e.g., Patches)
* A part of RAM is used as **stack** to store local variables, function arguments, return address, etc.
* A part of RAM is used as **heap** for dynamic memory allocations.

### Flash

* Mainly used to store the application **code** (i.e., the instructions generated after compilation of the program).

* Also holds constants:

  ```c
  char *message = "Hello World\n";
  const int var = 10;
  ```

* Holds the **vector table** for interrupts and exceptions of the MCU. (e.g., For any ARM Cortex-M based MCUs, the initial address of the code memory, which is typically Flash, is occupied by the vector table.)



## Stack & Heap in Embedded Systems

### Stack

* Last-In-First-Out (LIFO) order access of memory

* Stack pointer (SP) of an MCU is usually initialized to the highest memory address of RAM (i.e., beginning of stack). However, this depends multiple factors such as which stack model (e.g., full-ascending, full-descending, empty-ascending, empty-descending) is used.

  Stack pointer initialization is done in the "Startup code" of a project, so if you are writing the startup code yourself you need to take care of it.

* The following diagram shows, how information gets pushed onto stack upon a function call.



<img src="./img/empty-descending-stack-model.png" alt="empty-descending-stack-model" width="700">



* An example of the usage of stack upon a function call.



<img src="./img/usage-of-stack-upon-a-function-call.png" alt="usage-of-stack-upon-a-function-call" width="800">



### Heap

* Out-of-order access of memory

* Heap is used for dynamic allocation of memory to the application during the run-time.

* A "heap" is a general term used for any memory that is allocated dynamically and randomly.

* So, stack is managed by SP, but how is the heap managed?

  $\to$ It depends on the implementation of the application which utilizes heap.

  C provides `malloc`/`free` and C++ provides `new`/`free` standard library functions and how the heap is managed is completely dependent on the application programmers who utilizes these library functions.

* For, embedded system, `malloc` and `free` APIs are not recommended/suitable because they eat up large amount of code space, lack deterministic nature and incur fragmentation over time as blocks of memory are allocated/deallocated.

  Therefore, embedded systems programmers need to be careful when using the heap management algorithms.





## References

Nayak, K. (2022). *Mastering RTOS: Hands on FreeRTOS and STM32Fx with Debugging* [Video file]. Retrieved from https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/

