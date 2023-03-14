<a href="../">Notebook</a> > <a href="./">Projects</a> > Bare-Metal Task Scheduler

# Bare-Metal Task Scheduler

[!] Note: This project is currently in progress. Contents will be updated.



## Introduction

A task scheduler built from scratch on bare-metal STM32F microcontroller (ARM Cortex M4) board. 

This project is designed to be completed in the following 3 steps:

1. Implement the scheduler **using a full-fledged IDE** (STM32 Cube IDE).
2. Compile and debug the program implemented in Step 1 **without using an IDE**. 
3. Build the scheduler **from scratch** without an IDE (bare-metal).



## Objective

- Be able to set up incremental build system using Make utility.
- Be able to choose, install and use appropriate Cross-Compiler Toolchain.
- Be able to compile a C program for an embedded target without using an IDE.
- Be able to write a microcontroller startup file for STM32F MCU.
- Understand the different sections of the relocatable object file (i.e., `.o` files).
- Be able to write a linker script file from scratch.
- Be able to load the final executable on the target using OpenOCD and GDB client.



## Development Environment

* OS - Ubuntu 22.04.1 LTS (Kernel version: 5.15.0-52-generic)
* Tool Chain - GNU Arm Embedded Toolchain
* Debugger - OpenOCD, GDB client

