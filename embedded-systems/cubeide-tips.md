<a href="../">Notebook</a> > <a href="./">Embedded Systems</a> > CubeIDE Tips

# CubeIDE Tips



## Initial Setup

* To resolve FPU warning ("FPU is not initialized, but the project is compiling for an FPU. Please initialize the FPU before use."):

  Project $\to$ Properties $\to$ C/C++ Build $\to$ Settings $\to$ MCU Settings $\to$ Tool Settings

  Set `Floating-point unit` to `None`

  Set `Floating-point ABI` to `Software implementation (-mfloat-abi=soft)`



## Debugging Tips

* To see the snapshot of CPU registers (RCC registers, GPIO peripheral registers, etc.):

  Window $\to$ Show View $\to$ SFRs





## References

Nayak, K. (2022). *Microcontroller Embedded C Programming: Absolute Beginners* [Video file]. Retrieved from  https://www.udemy.com/course/microcontroller-embedded-c-programming/
