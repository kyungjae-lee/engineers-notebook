<a href="../">Notebook</a> > <a href="./">C Programming (Embedded)</a> > Pointers

# Pointers



## Address (`&var`) is not a Regular Number!

*  In C, an address of a variable is not a regular number. Consider the following example:

  ```c
  /* assume 64-bit system, size of a pointer is 8 byte */
  char ch = 'A';
  unsigned long long int addr_ch = &ch;	// size of 'long long in' is always 8 bytes 
  ```

  > Compiler will give you warning, because `&ch` is not just a number but is recognized as a **pointer** which is of type `char *`.

  If you still want to make this work, **typecast** the pointer into `unsigned long long int`.

  ```c
  unsigned long long int addr_ch = (unsigned long long int)&ch;
  ```

  

