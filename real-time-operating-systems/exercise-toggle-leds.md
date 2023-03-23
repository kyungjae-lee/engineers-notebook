<a href="../">Notebook</a> > <a href="./">Real-Time Operating Systems (RTOS)</a> > Exercise: Toggle LEDs

# Exercise: Toggle LEDs



## Problem Statement

* Toggle the three LEDs of the STM32F407 Discovery board with the frequency as defined below:

  * LED_GREEN (Task1) - 1000 ms

  * LED_ORANGE (Task2) - 800 ms

  * LED_RED (Task3) - 400 ms

* Create 3 FreeRTOS tasks of the same priority to handle three different LEDs.

* Each task will not interfere with one another. (No shared resources)

* Since this is a single-core system, "parallel" processing will be impossible to achieve. Just implement it in a time-sharing fashion!



## Design Model

* All user level code (i.e., tasks) runs in Thread Mode of the processor.
* CPU time-sharing is achieved by the scheduler.
* Task management is required and provided by RTOS kernel.
* Using priority among tasks can achieve prioritized task management.
* Can achieve low power (CPU is not always engaged; run a task and stay idle for some time...)





## References

Nayak, K. (2022). *Mastering RTOS: Hands on FreeRTOS and STM32Fx with Debugging* [Video file]. Retrieved from https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/

