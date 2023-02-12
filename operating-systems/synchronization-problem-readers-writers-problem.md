<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Synchronization Problem - Readers/Writers Problem

# Synchronization Problem - Readers/Writers Problem



## Introduction

* In dealing with the design synchronization and concurrency mechanisms, it is useful to be able to relate the problem at hand to known problems, and to be able to test any solution in terms of its ability to solve these  known problems.
* One of the several problems that appear frequently is the **readers/writers problem**. (Another is, producer/consumer problem)



## Readers/Writers Problem

* **Definition**

  There is a data area shared among a number of processes. The data area could be a file, a block of main memory, or even a bank of processor registers. There are a number of processes that only read the data area (readers) and a number that only write to the data area (writers). The condition that must be satisfied are as follows:

  1. Any number of readers may simultaneously read the file.
  2. Only one writer at a time may write to the file.
  3. If a writer is writing to the file, no reader may read it.

* Different variations of the problem include:

  * Readers have priority (can starve writers)
  * Writers have priority
  * Alternate priorities (i.e., don't allow too many new readers to enter if a writer is waiting, etc.)






## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.

