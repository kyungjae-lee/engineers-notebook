<a href="../">Home</a> > <a href="./">Projects</a> > Bare-Metal RTOS

# Bare-Metal RTOS

==[!] Note: This project is currently IN PROGRESS. Contents will be updated!==



## Introduction

Developing a real‐time operating system on the bare‐metal STM32F407‐Discovery board without using libraries

This project is designed to be completed in the following 3 steps:

1. Implement the scheduler **using a full-fledged IDE** (STM32 Cube IDE).
2. Compile and debug the program implemented in Step 1 **without using an IDE**. 
3. Build the scheduler **from scratch** without an IDE (bare-metal).



## Objective

- Understand and be able to implement a scheduler, one of the core part of the OS,  from scratch.
- Be able to set up incremental build system using Make utility.
- Be able to choose, install and use appropriate Cross-Compiler Toolchain.
- Be able to compile a C program for an embedded target without using an IDE.
- Be able to write a microcontroller startup file for STM32F MCU.
- Understand the different sections of the relocatable object file (i.e., `.o` files).
- Be able to write a linker script file from scratch.
- Be able to load the final executable on the target using OpenOCD and GDB client.



## Expected Outcome

Following is the demonstration of the test application for the 'Bare-Metal RTOS' project in action. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/MYxrrz4UWkc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- Scheduling Algorithm: Round-Robin 
- 4 tasks blinking each LED at a defined frequency:
  - Green: 1000 ms 
  - Orange: 500 ms 
  - Blue: 250 ms
  - Red: 125 ms



## Development Environment

* OS - Ubuntu 22.04.1 LTS (Kernel version: 5.15.0-52-generic)
* Tool Chain - GNU Arm Embedded Toolchain
* Debugger - OpenOCD, GDB client

