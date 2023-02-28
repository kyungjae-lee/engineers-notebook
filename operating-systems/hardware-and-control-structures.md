<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Hardware & Control Structures

# Hardware & Control Structures



## Introduction to Virtual Memory

* Two important characteristics of simple paging and simple segmentation:

  1. All memory refrences within a process are logical address that are dynamically translated into physical addresses at run-time. (A process may be swapped in and out of main memory such that it occupies different regions of main memory at different times during the course of execution.)
  2. A process may be broken up into a number of pieces (pages or segments) and these pieces need not be contiguously located in main memory during execution. The combination of dynamic run-time address translation and the use of a page or segment table permits this.

  These lead to the breakthrough in memory management; **it is not necessary that all of the pages or all of the segments of a process be in main memory during execution**.

* Example scenario (c.f., "piece" refers to a "page" or "segment" depending on which scheme is used)

  1. The OS begins by bringing in only one or a few pieces to include the initial program (code + data) piece .

     The portion of a process that is in main memory at a certain point in time is called the **resident set** of the process at that moment.

  2. Process executes smoothly as long as all memory references are to locations in the resident set.

     Page table or segment table is used during this process.

  3. If the processor encounters a logical address that is not in main memory, it generates an interrupt indicating a memory access fault.

  4. The OS puts the interrupted process in a Blocking state.

  5. The OS issues a disk I/O read request to bring into main memory the piece of the process that contains the missed logical address.

  6. The OS dispatches another process to run while the disk I/O is performed.

  7. Once the desired piece has been brought into main memory, an I/O interrupt is issued, giving control back to the OS, which places the affected process back into a Ready state.

* Two implications of this new strategy that lead to improved system utilization:

  * More processes may be maintained in main memory.
  * A process may be larger than all main memory.

* Because a process executes only in main memory, that memory is referred to as **real memory**. But a programmer or user perceives a potentially much larger memory (i.e., **virtual memory**) that is allocated on disk. 



## Locality and Virtual Memory



## Paging



## Segmentation



## Paging & Segmentation Combiend



## Protection & Sharing







## Virtual Memory Terminology

* **Virtual memory**

  A storage allocation scheme in which secondary memory can be addressed as though it were part of main memory. The addresses a program may use to reference memory are distinguished from the addresses the memory system uses to identify physical storage sites, and program-generated addresses are translated automatically to the corresponding machine addresses. The size of virtual storage is limited by the addressing scheme of the computer system, and by the amount of secondary memory available and not by the actual number of main storage locations.

* **Virtual address**

  The address assigned to a location in virtual memory to allow that location to be accessed as though it were part of main memory.

* **Virtual address space**

  The virtual storage assigned to a process.

* **Address space**

  The range of memory addresses available to a process.

* **Real address**

  The address of a storage location in main memory.



<img src="./img/tbd.png" alt="tbd" width="650">







## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.

