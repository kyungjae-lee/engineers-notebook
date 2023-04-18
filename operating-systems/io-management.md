<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > I/O Management

# I/O Management



## Techniques for Performing I/O

* **Programmed I/O**
  * In early systems, the processor issued commands, individual steps, to an I/O device. The process issuing the commands busy waited for them to execute.
  * Very inefficient use of the processor to control I/O as well as general computation in processes, because I/O devices are comparatively slow.
* **Interrupt-Driven I/O**
  * The processor issues the I/O commands to the device, while the process waits in a blocked state and can be reactivated when the I/O device interrupts the system to indicate the I/O request is finished.
* **DMA (Direct Memory Access)**
  * Processor sends a request for a block of data to be transferred into a frame or frames in memory and is only interrupted when the DMA device has completed the entire transfer.



## Evolution of the I/O Function

1. Processor directly controls a peripheral device or directly commands an I/O module attached to that device. Programmed I/O.
2. Processor sends instructions to devices as in (1), but interrupts are now used to eliminate busy waiting for those instructions to complete.
3. The I/O module/device is given direct access to the memory via DMA. It can now move an entire block of data without involving the processor, once started.
4. The I/O module is enhanced by giving it a processor with a specialized instruction set for the device it manages. Now, a single command can initiate a sequence of instructions.
5. The I/O module is enhanced with its own local memory and ability to implement customizable driver software to control a device. This is a specialized processor we mentioned in the context of multiprocessing architectures.



## Operating System Design Goals

* We want our OS to be able to manage I/O efficiently and generally.
* In other words, the OS can use techniques to speed up I/O (read/write) and it should have general purpose behavior for classes of devices.
* For example, using available memory frames to buffer I/O speeds up a process' ability to use some of the data while waiting for the rest.
* For example, Linux categorizes devices as block or stream oriented. Block devices read/write entire blocks at a time, while stream oriented handles one byte at a time, regardless of the actual device type.



## I/O Buffering

* It is sometimes convenient to perform input transfers in advance (anticipation) of requests being needed and to perform output transfers some time after the data is ready.
* Buffering data into memory frames is a technique that allows this.
* Buffering helps smooth out delays associated with bursts of I/O in a process.

### I/O Buffering Techniques

* **Single Buffering**
  * When a process issues an I/O request, the OS transfers data into the OS. I/O is transferred in/out of that buffer, then the buffer is exchanged into the process' frame(s) of memory.
  * Meanwhile, the OS can signal another buffer to begin transfer (looking ahead, prefetching, etc.).
  * The user process can be working with one block of data while the next block is being read for (or older block is being written out).
  * At any point in time, the user process is working with a single buffer and the OS has a single buffer on hand to dedicate to that process' next transaction.
* **Double Buffering**
  * The OS reserves two system buffers for transacting I/O on behalf of a user process. When one buffer fills and is being transferred to the user process' memory space, the 2^nd^ buffer can be used to continue working with the I/O devices.
  * This allows the OS to continue filling a buffer while the user buffer is being transferred over.
* **Circular Buffering**
  * This is basically an extension from 2 and N buffers.
  * The OS reserves N system buffers for transacting I/O on behalf of a user process. When one buffer fills and is being transferred to the user process' memory space, the next buffer in line can be used to continue working with the I/O devices.
  * This works well for systems with I/O devices of varying speeds. If the OS has a large reserve of memory to work with, it can allocate several buffers to each process.





## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.

