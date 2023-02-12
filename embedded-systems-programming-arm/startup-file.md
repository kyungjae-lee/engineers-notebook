<a href="../">Notebook</a> > <a href="./">Embedded Systems Programming (ARM)</a> > Startup File

# Startup File



## Importance of Startup File

* A **startup file** is responsible for setting up the proper environment for the main user code to run.
* Code written in startup file runs before `main()` and `main()` gets called by the startup code.
* Some part of the startup file is target (i.e., processor) dependent. (e.g., Vector table, initializing the stack, turning on/off the processor-specific peripherals such as FPU, etc.)
* Startup code takes care of the vector table placement in code memory as required by the ARM Cortex-M processor.
* Startup code may also take care stack reinitialization. (You can alter the stack placement.)
* Startup code is responsible for `.data` and `.bss` sections initialization in main memory. It needs to properly copy the `.data` section from FLASH to SRAM, and properly initialize the `.bss` section to SRAM.
* Only after the initial environment is properly setup, the startup code can call the `main()`.



## Write the Startup File

* Instruction:

  1. Create a vector table for your microcontroller. Vector tables are MCU-specific.
  2. Write a startup code which initializes `.data` and `.bss` section in SRAM.
  3. Call `main()`

  Startup file can be a C file (.c) or an assembly file (.s).





## References

Nayak, K. (2022). *Embedded Systems Programming on ARM Cortex-M3/M4 Processor* [Video file]. Retrieved from  https://www.udemy.com/course/embedded-system-programming-on-arm-cortex-m3m4/
