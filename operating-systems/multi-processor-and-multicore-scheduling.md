<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Multiprocessor and Multicore Scheduling

# Multiprocessor and Multicore Scheduling



## Introduction

* New issues arise in scheduling when more than 1 processor or core is available.
* Some systems are capable of treating threads as individually schedulable units (like processes).
* First, we will review the categories of multiprocessors and types of granularity (concurrent programming). Then we'll review process scheduling options for the moderately concurrent systems.



## Multiprocessor System Classification

* **Loosely Coupled (a.k.a. Distributed Multiprocessor or Cluster)**
  * Consists of a collection of relatively autonomous system.
  * Each processor has its own main memory and I/O channels.
  * Distributed systems are managed by special purpose distributed operating systems (including cloud systems). 
* **Functionally Specialied Processors**
  * Specialized processors are controlled by the main processor and provide services to it.
  * Examples are I/O processors (disk controller) and graphics processors.
* **Tightly Coupled Multiprocessor**
  * A set of processors that share a common main memory and are under the integrated control of a single operating system.



## Granularity

* A term related to th level of synchronization required to compute parallel tasks. It is also a measure of **concurrency** in software systems.

  | Grain Size  | Description                                                  | Synchronization Interval (Instructions) |
  | ----------- | ------------------------------------------------------------ | --------------------------------------- |
  | Fine        | Parallelism inherent in a single instruction stream          | < 20                                    |
  | Medium      | Parallel processing or multitasking within a single application | 20 - 200                                |
  | Coarse      | Multiprocessing of concurrent processes in a multiprogramming environment | 200 - 2000                              |
  | Very Coarse | Distributed processing across network nodes to form a single computing environment | 2000 - 1M                               |
  | Independent | Multiple unrelated processes                                 | Not applicable                          |

### Defining Granularity

* **Independent Parallelism**

  * There is no explicit synchronization among processes.
  * Each represents a separate, independent application or job.
  * This is a typical time-sharing system or general purpose desktop system.
  * A multiprocessor provides the same services as a multiprogrammed uniprocessor (i.e., you can schedule processes to run, only now >1 CPU)
  * Improves average response time to users due to having extra capacity.

* **Coarse / Very Coarse_Grained Parallelism**

* **Medium-Grained Parallelism**

* **Fine-Grained Parallelism**

  

<img src="./img/tbd.png" alt="tbd" width="650">







## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.

