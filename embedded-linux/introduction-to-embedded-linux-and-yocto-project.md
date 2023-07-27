<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Embedded Linux</a> > Introduction to Embedded Linux & Yocto Project

# Introduction to Embedded Linux & Yocto Project



## 4 Elements of Embedded Linux

* **Toolchain**
  * The compiler and other tools needed to create code for your target device.
  * Everything else depends on the toolchain.
* **Bootloader**
  * The program that initializes the board and loads the Linux kernel.
* **Kernel**
  * The heart of the system which manages system resources and interfaces with the underlying hardware.
* **Root** filesystem
  * Contains the libraries and programs that are run once the kernel has completed its initialization.
* Another element can be a collection of programs specific to your embedded application which makes the device do its job (e.g., weighing groceries, displaying movies, controlling a robot, etc.).



## What is Yocto Project?

* The term "yocto" means the smallest SI metric system prefix. (y = 10^-24^)

* The Yocto Project is an open-source collaboration project that provides tools and templates for creating custom Linux-based operating systems for embedded devices, such as IoT devices and embedded systems.
* It allows developers to build, customize, and maintain their own Linux distributions tailored to the specific requirements of their hardware and software.
* Yocto Project streamlines the process of building and configuring a Linux distribution from source code, enabling developers to create efficient and optimized systems for embedded applications. It is widely used in the embedded industry due to its flexibility, reusability, and extensive community support.
* Yocto is also a project working group of the Linux Foundation.
* Collaborators:
  * Many hardware manufacturers
  * Open source operating systems vendors
  * Electronics companies, etc.

### Input & Output of Yocto Project

* Input
  * A set of data that describes what we want, i.e., our specification.
  * e.g., Kernel configuration, hardware names, packages/binaries to be installed, etc.
* Output
  * A Linux-based embedded product
  * e.g., Linux kernel image, root file system (RFS), bootloader, device tree, toolchain, etc.





## Reference

Linux Trainer. (2020). *Embedded Linux using Yocto* [Video file]. Retrieved from https://www.udemy.com/course/embedded-linux-using-yocto/