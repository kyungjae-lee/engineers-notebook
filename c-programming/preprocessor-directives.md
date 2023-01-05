<a href="../">Notebook</a> > <a href="./">C Programming</a> > Preprocessor Directives

# Preprocessor Directives



## Introduction to Preprocessor Directives

* In C programming preprocessor directives are used to affect compile-time settings.

* Preprocessor directives are also used to create macros used as a textual replacement for numbers and other things.

* Preprocessor directives begin with the `#` symbol.

* Preprocessor directives are resolved or taken care of during the preprocessing stage of compilation.


* Preprocessor Directives Supported in C:

  | Objective               | Usage                                              | Syntax                                                       |
  | ----------------------- | -------------------------------------------------- | ------------------------------------------------------------ |
  | Macros                  | Used for textual replacement                       | `#define <identifier> <value>`                               |
  | File inclusion          | Used for file inclusion                            | `#include <std lib file name>` <br> `#include "user defined file name"` |
  | Conditional compilation | Used to direct the compiler about code compilation | `#ifdef`, `#endif`, `#if`, `#else`, `#ifndef`, `#undef`      |
  | Others                  |                                                    | `#error`, `#pragma`                                          |



## Macros in C (`#define`)

* Macros are written in C using `#define` preprocessor directive.

* Macros are used for textual replacement in the code, commonly used to define constants.

* Syntax:

  ```c
  #define <identifier> <value>	/* note that there is NO ';' at the end */
  ```

  Example:

  ```c
  #define MAX_RECORD 10
  ```

* In embedded systems programming (in C), macros are frequently used to define pin numbers, pin values, crystal speed, peripheral register addresses, memory addresses and for other configuration values. 

  For example,

  ```c
  #define PIN_8				8
  #define GREEN_LED			PIN_8
  #define LED_STATE_ON		1
  #define Led_state_off		0
  #define XTAL_SPEED			8000000UL
  #define	FLASH_BASE_ADDR		0x08000000UL
  #define SRAM_BASE_ADDR		0x20000000ul
  ```

  



## References

Nayak, K. (2022). *Microcontroller Embedded C Programming: Absolute Beginners* [Video file]. Retrieved from  https://www.udemy.com/course/microcontroller-embedded-c-programming/
