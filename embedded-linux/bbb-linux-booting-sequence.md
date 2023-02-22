<a href="../">Notebook</a> > <a href="./">Embedded Linux</a> > BBB Linux Booting Sequence

# BBB Linux Booting Sequence



## Linux Boot Requirements

### Hardware 

* e.g., AM335x SoC Functional Blocks



<img src="./img/am335x-soc-functional-blocks.png" alt="am335x-soc-functional-blocks" width="800">



### Software



<img src="./img/software-components-necessary-for-linux-booting.png" alt="software-components-necessary-for-linux-booting" width="800">



* **ROM Boot Loader (RBL)**
  * The very first piece of code (tiny boot loader) to run on the SoC when the power is supplied to the board.
  * This boot loader is written by the SoC vendor (e.g., Texas Instrument) and stored in ROM of the SoC during the production of this chip. 
  * This code cannot be modified and is not open to public.
  * The main responsibility of the RBL is to load/run the second stage boot loader such as SPL (or MLO) from the internal memory.
* **Secondary Program Loader (SPL)** - a.k.a. **Memory LOader (MLO)**
  * The responsibility of the second stage boot loader is to load/run the third stage boot loader such U-boot from the DDR memory of the board.
* **U-boot**
  * The responsibility of the third stage boot loader is to load/run the Linux kernel from the DDR memory of the board.
* **Linux kernel**
* **Root File System (RFS)**





## References

Nayak, K. (2022). *Embedded Linux Step by Step Using Beaglebone Black* [Video file]. Retrieved from https://www.udemy.com/course/embedded-linux-step-by-step-using-beaglebone/