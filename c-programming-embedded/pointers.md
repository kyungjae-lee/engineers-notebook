<a href="../">Notebook</a> > <a href="./">C Programming (Embedded)</a> > Pointers

# Pointers



## Address (`&var`) is not Just a Number!

*  In C, an address of a variable is not just a regular number. Consider the following example:

  ```c
  /* assume 64-bit system, size of a pointer is 8 byte */
  char ch = 'A';
  unsigned long int addr_ch = &ch;
  ```

  > Compiler will give you warning, because `&ch` is not just a number but is recognized as a **pointer** which is of type `char *`.

  If you still want to make this work, **typecast** the pointer into `unsigned long int`.

