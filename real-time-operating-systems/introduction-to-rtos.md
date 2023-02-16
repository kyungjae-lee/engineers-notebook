<a href="../">Notebook</a> > <a href="./">Real-Time Operating Systems (RTOS)</a> > Introduction to RTOS

# Introduction to RTOS



## What is "Real-Time"?

* Myth - The Real-Time Computing is equivalent to fast computing (X)

  Truth - The Real-Time Computing is equivalent to **predictable** computing (O)

* A real-time system guarantees **timeliness execution** of given tasks (i.e., response time is guaranteed), NOT the raw speed of execution. Having more processors, more RAM, faster bus interfaces does NOT make a system real-time.

* In a real-time system, the correctness of the computations depends upon both of the followings:

  * The logical correctness of the computation
  * The time at which the result is produced 

  Note that failing to meet the timing constraints will also be considered a FAILURE in a real-time system.



## What is a Real-Time Application (RTA)?

* RTAs are not necessarily the fast-executing applications.

* RTAs are **time-deterministic** applications, which means that their **response time to events is almost constant**.

* **Soft real-time applications** (Tolerant to small delays)

  There could be a small deviation in response time, in terms of milliseconds or seconds, and it will not be considered a failure.

  e.g., VoIP application, Stock market website

* **Hard real-time applications** (Not tolerant to any delays)

  MUST complete within a given time limit and failure to do so will result in an absolute failure of the system. 

  e.g., Airbag deployment system, Anti-lock breaking system, Missile guidance and control system

* To run a real-time application, a real-time operating system is necessary.



## What is a Real-Time Operating System (RTOS)?

* An RTOS is an OS specially designed to run applications with very precise timing and a high degree of reliability.
* To be considered as "real-time", an OS must have a known maximum time for each of the critical operations that it performs. Some of these operations include:
  * Handling of interrupts and internal system exceptions
  * Handling of critical sections
  * Scheduling mechanisms, etc.



## GPOS vs. RTOS

* General-Purpose Operating System (GPOS)

  e.g., Windows, Linux, iOS, Android, etc.

* Real-Time Operating System (RTOS)

  e.g., VxWorks (Windriver), QNX (Blackberry), FreeRTOS, Integrity, etc.

### Task Scheduling

* **GPOS (Higher Throughput)**

  * GPOS is programmed to handle scheduling in such a way that it manages to achieve high throughput.

    Throughput - the total number of processes that complete their execution per unit time

  * Sometimes execution of a high priority process will get delayed in order to serve 5 or 6 low priority tasks. High throughput is achieved by serving 5 low priority tasks than by serving a single high priority one. (If 5 or 6 low priority applications are waiting to run, then the GPOS may delay 1 or 2 high priority task in order to increase the throughput.)

  * In GPOS, the scheduler typically uses a fairness policy to dispatch threads and processes onto the CPU. Such a policy enables the high overall throughput required by desktop and server applications, but offers no guarantees that high-priority, time critical threads or processes will execute in preference to lower-priority threads.

* **RTOS (Higher Time-predictability)**

  * In RTOS, thread execute in the order of their priority. If a high priority thread becomes ready to run, it will take over the CPU from any lower-priority thread that may be executing.

  * Here a high-priority thread gets executed over the low priority ones. All "low priority thread execution" will get paused. A high-priority thread execution will get overridden only if a request comes from an even higher-priority threads.

  * RTOS may yield less throughput than the GPOS because it always favors the high priority tasks, but that does not necessarily mean that it has poor throughput. A quality RTOS will still deliver a decent overall throughput but, can sacrifice throughput for being deterministic or to achieve time predictability.

  * For an RTOS, achieving predictability or time deterministic nature is more important than achieving high throughput.

### Latency

* **GPOS ()**
* **RTOS ()**







## References

Nayak, K. (2022). *Mastering RTOS: Hands on FreeRTOS and STM32Fx with Debugging* [Video file]. Retrieved from https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/

