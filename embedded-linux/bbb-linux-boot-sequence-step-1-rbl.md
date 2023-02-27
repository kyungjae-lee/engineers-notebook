<a href="../">Notebook</a> > <a href="./">Embedded Linux</a> > BBB Linux Boot Sequence - Step 1: RBL

# BBB Linux Boot Sequence - Step 1: RBL



## BBB Linux Boot Sequence



<img src="./img/software-components-necessary-for-linux-booting.png" alt="software-components-necessary-for-linux-booting" width="800">





## ROM Boot Loader (RBL)

* Stack setup
* WatchDog timer 1 configuration (set to 3 minutes timeout)
* System clock configuration using PLL
* Search memory devices or other bootable interfaces for MLO or SPL
* Copy MLO or SPL into the internal SRAM of the chip
* Execute MLO or SPL





## References

Nayak, K. (2022). *Embedded Linux Step by Step Using Beaglebone Black* [Video file]. Retrieved from https://www.udemy.com/course/embedded-linux-step-by-step-using-beaglebone/