<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Computer Architecture & Organization</a> > ARM Assembly Language

# ARM Assembly Language



## C/C++ Build Process and Beyond



<img src="./img/c-cpp-build-process.png" alt="c-cpp-build-process" width="1000">



* At the **linking** phase, external references in the programs (e.g., `.global main`, `.global printf`, `.global scanf` in the assembly program) are resolved because the linker now has access to all those pieces of code.

  The last thing the linker will do is to load the address for the start of your code (main) into the *Program Counter (PC)*. At that time your program takes over the CPU.




## Assembler

* An **assembler** is a program that converts the assembly code into the machine code (binary).

    - The output of the assembler is directly executed by the CPU (mostly).

* **Two-Pass Assembler**

  - First pass
    - A symbol table is built.
    - The *labels* are put in a table and the assembler determines the address assigned to that label.
    - Memory allocations are made for the instructions and data.
    - Any incorrect instructions, syntax and referenced labels that are not defined are flagged as **errors**.    
        - Extra spaces, all comments are ignored by the assembler.
        - Most modern assemblers are NOT case sensitve.
  - Second pass
    - All symbol references in the code are resolved and the final binary code is generated.

* Typical instruction in memory will look something like this:

  ```plain
  Opcode | Addressing Mode | Operands
  ```

  Even with RISC machines there are several instruction formats depending on the instruction, associated addressing mode and other options like shifting of registers, etc. 

  The detailed manual on the ARM assembler provides all this information.

* **Assembler Directives**

  - Assembler directives tell the assembler how to assemble your code. They do NOT get translated into machine code but CAN affect the way the code is created.

    [!] Note: **Assembly instructions** or **executable instructions** are translated into machine code which are executed by the processor. This is your program.

  - Assemblers vary and sometimes quite a lot. In most cases:

    - Ignore extra spaces between operands.
      e.g., `r1,r2,r3` and  `r1,    r2,  r3` are the same.

    - Not case sensitive for instructions.

      e.g., `ADD`, `add`, `Add` are the same.

    - Comments can start one space after the last operand, and can be `/* */` (multi-line) or `@` (single-line) delimited.

    - Ignore extra spaces between operands.

      e.g., `r1,r2,r3` is identical to `r1,    r2,  r3`.

    - Labels are case sensitive.
      e.g., `Loop:` and `loop:` are different labels.

    - Macros 

    - Last line in code file has to be a blank line.

  - Things you can do with assembler directives:

    - Where you want your code to be located in memory, when it is loaded.
    - Allocate storage space to variables
    - Initialize variables
    - Tell the assembler where your assembly code stops and do NOT assemble anything after this point (`END` or `STOP`) 

  - Examples:

    ```plain
    .balign 4         @ Forces a word boundary.
                      @ 4 specifies the number of bytes that must be aligned to.
                      @ (this number must be power of 2).
                      @ It means, the next piece of memory that I declare after this directive
                      @ need to start at a memory location that is divisible by 4. It has to be
                      @ aligned with the memory locations that are divisible by 4.
                      @
                      @ When you declare an array, this is important because you want
                      @ each of those elements to have an individual address that is
                      @ divisible by 4. If it is not divisible by 4 (word size), 1, 2,
                      @ or 3 bytes of an individual element can be partially stored before
                      @ the array, after that particular location.
    
    Q:  .word 9       @ Defines a label 'Q' at current memory location as word-size and
                      @ sets it to a decimal 9.
                      @ .word  allocates 4 byte memory space to hold data.
                      @ .hword allocates 2 byte memory space to hold data.
                      @ .byte  allocates 1 byte memory space to hold data.
    
    Array: .skip 4x10 @ Defines a label 'Array' at current memory location and reserves
                      @ 40 bytes of memory.
                      @ Note that '.skip' does NOT initialize the allocated memory; Thus
                      @ 'Array' will contain whatever garbage values it was previously 
                      @ set to.
    
    str1: .asciz     "This is a sample string.\n"
                      @ .asciz puts a terminating null character at the end of the string.
    
    str2: .ascii     "This is a sample string.\0\n"
                      @ .ascii does NOT put a terminating null character at the end.
                      @ (It must be explicitly added by the programmer.)
                      @
                      @ Note that using the null character \0 as the terminator
                      @ for strings is a C construct, not an assembly's.
    ```

    To use the string defined in the `.data` section:

    ```plain
    LDR r1, =str1     @ Put the address of the start of the str1 in r1.
                      @ The = (equal sign) in front of a label reference will use
                      @ the address of the label NOT the contents of the memory
                      @ reference.
    ```

    Good example of using the literal declaration in conjunction with the arrays:

    ```plain
    .text
    ...
    
    @ Even though the assembler allows you to put these declarations anywhere
    @ in the code, it is a good practice to put them at the top of your code
    @ for the better readability and maintainability.
    
    Monday    = 1         @ .equ Monday,      1
    Tuesday   = 2         @ .equ Tuesday,     2
    Wednesday = 3         @ .equ Wednesday,   3
    Thursday  = 4         @ .equ Thursday,    4
    Friday    = 5         @ .equ Friday,      5
    Saturday  = 6         @ .equ Saturday,    6
    Sunday    = 7         @ .equ Sunday,      7
    
    ...
    
    .data
    
    DaysOfWeek:           @ Now this is an array containing  
    .word Monday          @ 1, 2, 3, 4, 5, 6, 7
    .word Tuesday   
    .word Wednesday 
    .word Thursday  
    .word Friday    
    .word Saturday  
    .word Sunday    
    
    WeekendDays:          @ Now this is an array containing
    .word Saturday        @ 6, 7
    .word Sunday    
    
    WeekDays:             @ Now this is an array containing
    .word Monday          @ 1, 2, 3, 4, 5
    .word Tuesday   
    .word Wednesday 
    .word Thursday  
    .word Friday          
    ...
    ```

* **Location Counter**

  - Location counter is the pointer to the next location in memory that the assembler maintains when a program is being assembled. This pointer is necessary for the assembler to do the proper memory allocation, variable
    initialization, etc.
  - Similar in concept to the program counter except the **location counter** is used during the *assembly process* and the **program counter** is used during the *program execution process*.



## Structure of an ARM Program (for the Raspberry Pi)

* There are differences between the textbook and the Raspberry Pi assembler directives. (We will be using the ones for the Raspberry Pi.)

* Structure of an ARM Program for the Raspberry Pi: (Try to follow this format.)

  ```plain
  @ Structure of an ARM Program for the Raspberry Pi 
  
  @ --------------------------------------------------------------------------------------
  .text         @ Defines a read-only section. (Code starts here)
                @ Everything from here to .data (read/write) section, you don't
                @ want the code to be modified or overwritten by accident.
                @ Attempt to write in this section will produce 'seg fault'.
                @ .text tells the assembler to switch to the text segment
                @ (where code goes).
                
  @ --------------------------------------------------------------------------------------
  .global main  @ Defines the location of the first instruction to be executed.
                @ This is the memory address that will be loaded into the PC.
  main:         
  
                @ Make sure to have 'exit:' statement in this area so your program
                @ terminates at some point.
  
  @ --------------------------------------------------------------------------------------
  .data         @ Defines a read/write section.
                @ .data tells the assembler to switch to the data segment
                @ (where data goes).
  
  .balign 4     @ Forces a word boundary.
                @ 4 specifies the number of bytes that must be aligned to.
                @ (this number must be power of 2).
                @ It means, the next piece of memory that I declare after this directive
                @ need to start at a memory location that is divisible by 4. It has to be
                @ aligned with the memory locations that are divisible by 4.
  
  Q:  .word 9   @ Defines a label Q at current memory location as word-size and
                @ sets it to a decimal 9.
                @ .word  allocates 4 byte memory space to hold data.
                @ .hword allocates 2 byte memory space to hold data.
                @ .byte  allocates 1 byte memory space to hold data.
  
  Str: .asciz "This is a sample string.\n"
                @ '.asciz' adds terminating null character at the end so you don't have to
                @ include it manually.
                @
                @ `.ascii` does NOT add terminating null character so you have to
                @ explicitly add it at the end of the string.
                @ (e.g., .ascii "This is a sample string.\0\n")
                @ ∴ Just use '.asciz' and make your life eaiser.
  
  Array: .skip 4x10 @ Defines a label 'Array' at current memory location and reserves
                    @ 40 bytes of memory.
                    @ Note that '.skip' does NOT initialize the allocated memory; Thus
                    @ 'Array' will contain whatever garbage values it was previously 
                    @ set to.
  
  @ --------------------------------------------------------------------------------------
  .global XXX   @ Defines any external calls like printf, scanf, etc.
  
  @ --------------------------------------------------------------------------------------
  .end          @ Defines the end of the assembly code. 
                @ There is no more assembly code after this point in the file.
                @ Can be replaced by a blank line.
  @ --------------------------------------------------------------------------------------
  ```

  > Labels such as `main:`, `.end`, `Str` represent addresses! Be aware how these are called in the main section.



## ARM Assembly Program Examples

### Example 1

* **C Code** 

  ```c
  X = P - Q;
  
  if (X >= 0) { X = P + 5; }
  else { X = P + 20; }
  ```

* **ARM Assembly Code (for the Raspberry Pi)**    

  ```assembly
  @ ---------------------------------------------------------------------------
  @ Case 1: P = 12, Q = 9, results in X = 17 (executes 'if' part)
  @ ---------------------------------------------------------------------------
  
  .text
  
  .balign 4             @ Make sure the code is on full word boundaries.
  
  .global main          @ Entry point for the program. (Must be global)
  
  main:
    ldr  r0, =P         @ Load the address of P
    ldr  r0, [r0]       @ Load the value of P
    ldr  r1, =Q         @ Load the address of Q
    ldr  r1, [r1]       @ Load the value of Q
    ldr  r3, =X         @ Load the address of X
    subs r2, r0, r1     @ Do the subtraction and set the CCR flags
    bpl  then           @ If positive, add 5
    add  r2, r0, #20    @ Else (if negative), add 20
    b    exit           @ Done with calculation
  
  then:
    add  r2, r0, #5
  
  exit:
    str  r2, [r3]       @ Write the result back to X in memory
    mov  r0, r2         @ Put x in r0 so it can be echo printed
    bx   lr             @ Stop
  
  .data
  
  .balign 4             @ Force to the next whole word.
  X:  .word 0           @ Define one word for X and initialize to 0
  
  .balign 4             @ Force to the next whole word.
  P:  .word 12          @ Define one word for P and initialize to 12
  
  .balign 4             @ Force to the next whole word.
  Q:  .word 9           @ Define one word for Q and initialize to 9
  
  
  @ ---------------------------------------------------------------------------
  @ Case 2: P = 12, Q = 14, results in X = 32 (executes 'else' part)
  @         (Same logic with changed data value, Q = 14)
  @ ---------------------------------------------------------------------------
  
  .text
  
  .balign 4             @ Make sure the code is on full word boundaries.
  
  .global main          @ Entry point for the program. (Must be global)
  
  main:
  ldr  r0, =P         @ Load the address of P
  ldr  r0, [r0]       @ Load the value of P
  ldr  r1, =Q         @ Load the address of Q
  ldr  r1, [r1]       @ Load the value of Q
  ldr  r3, =X         @ Load the address of X
  subs r2, r0, r1     @ Do the subtraction and set the CCR flags
  bpl  then           @ If positive, add 5
  add  r2, r0, #20    @ Else (if negative), add 20
  b    exit           @ Done with calculation
  
  then:
  add  r2, r0, #5
  
  exit:
  str  r2, [r3]       @ Write the result back to X in memory
  mov  r0, r2         @ Put x in r0 so it can be echo printed
  bx   lr             @ Stop
  
  .data
  
  .balign 4             @ Force to the next whole word.
  X:  .word 0           @ Define one word for X and initialize to 0
  
  .balign 4             @ Force to the next whole word.
  P:  .word 12          @ Define one word for P and initialize to 12
  
  .balign 4             @ Force to the next whole word.
  Q:  .word 14          @ Define one word for Q and initialize to 14
  ```

### Example 2

* **C Code**

  ```c
  /* Calculates 1 + 2 + 3 + ... + 19 + 20 */
  
  int counter = 1;
  int rsum = 0;
  
  while (counter < 21)
  {
      rsum += counter;
  }
  ```

* **ARM Assembly Code (for the Raspberry Pi)**

  ```assembly
  @ ---------------------------------------------------------------------------
  @ Calculates 1 + 2 + 3 + ... + 19 + 20
  @ ---------------------------------------------------------------------------
  
  .text
  
  .balign 4             @ Make sure the code is on full word boundaries.
  
  .global main          @ Entry point for the program. (Must be global)
  
  main:
    mov  r0, #1         @ Counter set to 1
    mov  r1, #0         @ Running sum set to 0
  
  next:
    add  r1, r1, r0     @ Add the counter to the running sum
    add  r1, r0, #1     @ Increment the counter
    cmp  r0, #21        @ If counter < 21
    bne  next           @ Branch to do the next loop
    mov  r0, r1         @ Else, put sum into r0 so it can be displayed
    bx   lr             @ Stop
  
  .data
  ```
