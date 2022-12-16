<a href="../">Notebook</a> > <a href="./">C Programming</a> > `const` Keyword and Pointers

## `const` Keyword and Pointers



## Constant Pointers

A constant pointer is a pointer that cannot change the address it is holding. In other words, once a constant pointer points to a variable then it cannot point to any other variable.



## Pointer to Constant

A pointer through which one cannot change the value of variable it points to. The pointer itself can point to a different memory location, but modifying the value stored at that location through the pointer is not allowed.



## Examples

```c
int *ptr;				// pointer to int

const int *ptr;			// pointer to 'const int'
						// ptr has no ability to modify int value it points to
						// ptr itself can point to a different memory location

int * const ptr;		// 'const pointer' to int
						// ptr has the ability to modify the int value it points to
						// ptr itself cannot point to a different memory location

const int * const ptr;	// 'const pointer' to 'const int'
						// ptr has no ability to modify int value it points to
						// ptr itself cannot point to a different memory location
```

The first `const` keyword can be on either side of the type:

```c
const int *ptr;			// int const *ptr;
const int * const ptr;	// int const * const ptr;
```



## More Examples

```c
int **ptr;					// pointer to a pointer to an int
int ** const ptr;			// const pointer to a pointer to an int
int * const *ptr;			// pointer to a const pointer to an int
const int **ptr;			// pointer to a pointer to a const int
int * const * const ptr;	// const pointer to a const pointer to an int
```

