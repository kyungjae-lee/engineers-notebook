<a href="../">Notebook</a> > <a href="./">C Programming</a> > `volatile` Type Qualifier

# `volatile` Type Qualifier



## What is `volatile` Type Qualifier?

`volatile` is a type qualifier in C used with variables to **instruct the compiler not to perform any optimization** on the variable operation.

It tells the compiler that the value of the variable may change at any time with/without the programmer's consent. Even with the higher optimization level (> `-O0`) setting, the compiler will turn off the optimization of the read/write operations on those variables that are declared using `volatile` type qualifier.

`volatile` is very helpful in embedded systems code.

### Optimization Levels

* `-O0`

  No optimization (default)

  Generates unoptimized code but has the fastest compilation time. (Note that many other compilers do substantial optimization even if "no optimization" is specified. With gcc, it is very unusual to use `-O0` for production if execution time is of any concern, since `-O0` means (almost) no optimization. This difference between gcc and other compilers should be kept in mind when doing performance comparisons.)

* `-O1`

  Moderate optimization

  Optimizes reasonably well but does not degrade compilation time significantly.

* `-O2`

  Full optimization

  Generates highly optimized code and has the slowest compilation time.

* `-O3`

  Full optimization as in `-O2`

  Also uses more aggressive automatic inlining of subprograms within a unit and attempts to vectorize loops.

* `-Os`

  Optimize space usage (code and data) of resulting program.



## When to Use `volatile` Type Qualifier?

A variable must be declared using a `volatile` type qualifier when there is a possibility of unexpected changes (e.g., by user) in the variable value.

The unexpected changes in the variable value may happen from within the code or from outside the code (i.e., from the hardware).

Use `volatile` when your code is dealing with the following scenarios:

* Memory-mapped peripheral registers of the microcontrollers
* Multiple tasks accessing global variables (read/write) in an RTOS multi-threaded application
* When a global variable is used to share data between the main code and an ISR code



## How to Use `volatile` Type Qualifier?

### Case 1: `volatile` data

**Syntax:**

```c
uint8_t volatile my_data;	/* my_data is a volatile variable of type unsigned integer_8 */
volatile uint8_t my_data;
```

### Case 2: non-`volatile` pointer to `volatile` data

**Syntax:**

```c
uint8_t volatile *pStatusReg;	/* pStatusReg is a non-volatile pointer pointing to a volatile
								   data of the type unsigned integer_8 */
								/* instructs the compiler not to do any optimization on data 
								   read/write operations using pStatusReg */
```

This is a perfect case of accessing memory-mapped registers. Use this syntax generously whenever you are accessing memory-mapped registers in your microcontroller code.

### Case 3: `volatile` pointer to non-`volatile` data

**Syntax:**

```c
uint8_t *volatile pStatusReg;	/* pStatusReg is a volatile pointer pointing to a non-volatile
								   data of type unsigned integer_8 */
```

Rarely used.

### Case 4: `volatile` pointer to `volatile` data

**Syntax:**

```c
uint8_t volatile *volatile pStatusReg;	/* pStatus Reg is a volatile pointer pointint to a volatile
										   data of type unsigned integer_8 */
```

Rarely used.
