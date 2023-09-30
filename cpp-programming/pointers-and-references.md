<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Pointers & References

# Pointers & References



## Overview

### Pointers

* What is a pointer?
* Declaring pointers
* Storing addresses in pointers
* Dereferencing pointers
* Dynamic memory allocation
* Pointer arithmetic
* Pointers and arrays
* Pass-by-reference with pointers
* `const` and pointers
* Using pointers to functions
* Potential pointer pitfalls

### References

* What is a reference?
* Review passing references to functions
* `const` and references
* Reference variables in range-based for loops
* Potential reference pitfalls
* Raw vs. Smart pointers



## Pointers

### What is a Pointer?

* A variable whose value is an address

* What can be at that address?
  * Another variable
  
  * A function
  
* Pointers point to variables or functions?

* If x is an integer variable and its value is 10, then I can declare a pointer that points to it.

* To use the data that the pointer is pointing to you must know its type


### Why Use Pointers?

* Can't I just use the variable or function itself? $\to$ Yes, but not always!
* Inside functions, pointers can be used to access data that are defined outside the function. Those variables may not be in scope so you can't access them by their name.
* Pointers can be used to operate on arrays very efficiently.
* We can allocate memory dynamically on the heap or free store.
  * This memory doesn't even have a variable name.
  * The only way to get to it is via a pointer.
* With OO, pointers are how polymorphism works!
* Can access specific addresses in memory.
  * Useful in embedded and systems applications

### Declaring Pointers

* Declaration

  ```cpp
  variable_type *pointer_name;
  ```

  Example:

  ```cpp
  int *int_ptr;
  double *double_ptr;
  char *char_ptr;
  string *string_ptr;
  ```

* Initializing pointer variables to 'point nowhere'

  ```cpp
  variable_type *pointer_name {nullptr};
  ```

  Example;

  ```cpp
  int *int_ptr{};
  double *double_ptr{nullptr};
  char *char_ptr{nullptr};
  string *string_ptr{nullptr};
  ```

* Always initialize pointers

* Uninitialized pointers contain garbage data and can 'point anywhere'.

* Initializing to zero or `nullptr` (C++11) represents address zero

  * Implies that the pointer is 'pointing nowhere'

* If you don't initialize a pointer to point to a variable or function then you should initialize it to `nullptr` to 'make it null'

