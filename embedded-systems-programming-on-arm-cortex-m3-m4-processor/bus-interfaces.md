<a href="../">Notebook</a> > <a href="./">Embedded Systems Programming on ARM Cortex-M3/M4 Processor</a> > Bus Interfaces

# Bus Interfaces



## Bus Interfaces

* ARM Cortex Mx Processors' bus interfaces are based on advanced microcontroller bus architecture (AMBA) specification.

* AMBA is a specification designed by ARm which governs the standard for on-chip communication inside the system-on-chip (SoC).

* AMBA specification supports several bus protocols.

  * **AHB Lite (AMBA High-performance Bus)**

    Mainly used for the main bus interfaces on the microcontroller.

    High-speed communication with peripherals that demand high operation speed.

  * **APB (AMBA Peripheral Bus)**

    Used for Private Peripheral Bus (PPB) access and some on-chip peripheral access using an AHB-APB bridge.

    Low-speed communication compared to AHB. 

    Most of the peripherals which don't require high operation speed are connected to this bus.








## References

Nayak, K. (2022). *Microcontroller Embedded C Programming: Absolute Beginners* [Video file]. Retrieved from  https://www.udemy.com/course/microcontroller-embedded-c-programming/

Nayak, K. (2022). *Embedded Systems Programming on ARM Cortex-M3/M4 Processor* [Video file]. Retrieved from  https://www.udemy.com/course/embedded-system-programming-on-arm-cortex-m3m4/
