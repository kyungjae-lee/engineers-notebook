<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Memory Management Requirements

# Memory Management Requirements



## Introduction

* In a **uniprogramming** system, main memory is simply divided into 2 parts:

  * The OS (resident monitor, kernel)
  * The user (the program in execution)

* In a **multiprogramming** system, main memory is still divided into 2 parts:

  * The OS
  * The user 

  But, now, the user part of memory must be further subdivided to accommodate multiple processes. This subdivision carried out dynamically by the OS is called **memory management**.

* Effective memory management is vital in a multiprogramming system. Memory needs to be allocated to ensure a reasonable supply of ready processes to consume available processor time.



## Memory Management Requirements

* The requirements memory management is intended to satisfy.
  * Relocation
  * Protection
  * Sharing
  * Logical organization
  * Physical organization

### Relocation

* Processes must be relocatable to maximize the main memory utilization.



## Important Terms

* **Frame**

  A fixed-length block of main memory.

* **Page**

  A fixed-length block of data that resides in secondary memory (i.e., disk). A page of data may temporarily be copied into a frame of main memory.

* **Segment**

  A variable-length block of data that resides in secondary memory. An entire segment may temporarily be copied into an available region of main memory (segmentation) or the segment may be divided into pages, which can be individually copied into main memory (combined segmentation and paging.)






## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.
