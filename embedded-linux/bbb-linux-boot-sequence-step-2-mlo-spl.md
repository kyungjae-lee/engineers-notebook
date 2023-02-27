<a href="../">Notebook</a> > <a href="./">Embedded Linux</a> > BBB Linux Boot Sequence - Step 2: MLO/SPL

# BBB Linux Boot Sequence - Step 2: MLO/SPL



## BBB Linux Boot Sequence



<img src="./img/software-components-necessary-for-linux-booting.png" alt="software-components-necessary-for-linux-booting" width="800">





## **Secondary Program Loader (SPL)** / **Memory LOader (MLO)**

* The main responsibility of MLO/SPL is to initialize the SoC to a point where U-boot can be loaded into external RAM (DDR memory). 

  * It does UART console initialization to print out the debug messages.

  * Reconfigures the PLL to desired value.

  * Initializes the DDR registers to use the DDR memory.

  * Does muxing configurations of boot peripherals pin, because its next job is to load the u-boot from the boot peripherals.

    e.g., If MLO is going to get the U-boot from the MMC0 or MMC1 interfaces, then it will do the mux configurations to bring out the MMC0 or MMC1 functionalities on the pins.

* Q) Is there any way we can make RBL directly load the U-boot into internal SRAM without going through the second stage (SPL/MLO)?

  * In order to load and run U-boot from the internal SRAM, you have to squeeze U-boot image into a memory <128 KB of size which is not possible. This is the reason why wee use MLO/SPL.

* Q) Is there any way we can use RBL to load U-boot directly into DDR memory of the board and skip using MLO/SPL?

  * The reason why ROM boot loader of the AM335x SOC cannot load Uboot directly to DDD3 by skipping SPL is, ROM code won’t be having any idea about what kind of DDR RAM being used in the product to initialize it.

    DDR RAM is purely product/ board specific.

    Let’s say there are 3 board/product manufacturing companies X,Y,Z. X may design a product using AM335x SOC with DDR3. In which lets say DDR3 RAM is produced by microchip. Y may design its product using AM335x SOC with DDR2 produced by Transcend and Z may not use DDR memory at all for its product.

    So, RBL has no idea in which product this chip will be used and what  kind of DDR will be used, and what are DDR tuning parameters like speed, bandwidth, clock augments, size, etc. Because to read ( or write) anything from/to DDR RAM , first, the  tuning parameters must be set properly and DDR registers must be  initialized properly.

    Every different manufacture will have different parameters for their  RAM. So, that’s the reason RBL never care about initializing DDR  controller of the chip and DDR RAM which is connected to chip.

    RBL just tries to fetch the SPL found in memory devices such as eMMC and SD card or peripherals like UART,EMAC,etc. And in the SPL/MLO, you should know, what kind of DDR is connected to your product and based on that you have to change the SPL code ,  rebuild it and generate the binary to use it as the second stage boot  loader.

    For example the Beaglebone black uses DDR3 from Kingston and if your  product uses DDR3 from transcend, then if the turning parameters are  different then you have to change the DDR related header files and the  tuning parameter macros of the SPL , rebuild and generate the binary . 





## References

Nayak, K. (2022). *Embedded Linux Step by Step Using Beaglebone Black* [Video file]. Retrieved from https://www.udemy.com/course/embedded-linux-step-by-step-using-beaglebone/