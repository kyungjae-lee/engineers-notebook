<a href="../">Notebook</a> > <a href="./">Real-Time Operating Systems (RTOS)</a> > FreeRTOS Directory Structure

# FreeRTOS Directory Structure



## Download FreeRTOS

* [https://freertos.org/](https://freertos.org/)
* [https://github.com/FreeRTOS/FreeRTOS/releases](https://github.com/FreeRTOS/FreeRTOS/releases) $\to$ GitHub repo (download previous release)



## FreeRTOS Directory Structure

+ The **FreeRTOS/Source** directory contains the FreeRTOS source code, and contains its own readme file.

+ The **FreeRTOS/Demo** directory contains a demo application for every official FreeRTOS port, and contains its own readme file.

+ The **FreeRTOS/Test** directory contains the tests performed on common code and the portable layer code, and contains its own readme file.



## Notes

* See [FreeRTOS/SourceOrganization](http://www.freertos.org/a00017.html) for full details of the directory structure and information on locating the files you require.
* The easiest way to use FreeRTOS is to start with one of the pre-configured demo application projects (found in the FreeRTOS/Demo directory).  That way you will have the correct FreeRTOS source files included, and the correct include paths configured. 
* Once a demo application is building and executing you can remove the demo application file, and start to add in your own application source files.



## See Also

+ [Quick Start Guide](http://www.freertos.org/FreeRTOS-quick-start-guide.html)

+ [FAQ](http://www.freertos.org/FAQHelp.html)





## References

Nayak, K. (2022). *Mastering RTOS: Hands on FreeRTOS and STM32Fx with Debugging* [Video file]. Retrieved from https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/

