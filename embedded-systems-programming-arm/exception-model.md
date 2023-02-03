<a href="../">Notebook</a> > <a href="./">Embedded Systems Programming (ARM)</a> > Exception Model

# Exception Model

This section applies to ARM Cortex M0/M1/M3/M4/M7 processors.



## ARM Cortex Mx Processor Exception Model

* What is exception?

  Anything that disturbs the normal operation of the program by changing the operation mode of the processor

* Two major types of exception:

  1. **System exceptions**  (Generated **by the processor**; total 15)
  2. **Interrupts**  (Generated from **outside the processor**; total 240)

  $\therefore$ ARM Cortex Mx processors support total 255 exceptions.

* Whenever the processor core encounters an exception, it changes the operation mode to "Handler mode".



## System Exceptions

* There is a room for 15 system exceptions.
* Exception no. 1 is the "Reset exception (or system exception)".
* Only 9 system exceptions are implemented. (6 are reserved for future implementation)
* Exception no. 16 is "Interrupt 1 (IRQ 1)"





## References

Nayak, K. (2022). *Embedded Systems Programming on ARM Cortex-M3/M4 Processor* [Video file]. Retrieved from  https://www.udemy.com/course/embedded-system-programming-on-arm-cortex-m3m4/
