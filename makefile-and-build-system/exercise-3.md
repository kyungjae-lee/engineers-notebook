<a href="../">Notebook</a> > <a href="./">Makefile & Build System</a> > Exercise 3

# Exercise 3



## Requirements

Add to the makefile from the previous exercise the functionality to create a shared dynamic library file (.so) that will be used by `mod3` and ` mod5`. 

> A dynamic library is a programming concept in which shared libraries  with special functionalities are launched only during program execution, which minimizes overall program size and facilitates improved  application performance for reduced memory consumption. In most software programs, distributing specific functionalities into distinct modules  allows loading as needed. 
>
>  A dynamic library is never part of an executable file or application.  During runtime, a link is established between a dynamic library and  executable file or application.

Source, header files to be used:

```cpp
// project/src/mod2/mod2.h
#pragma once // prevent from being included multiple times

void f2();
```

```cpp
// project/src/mod2/mod2.cpp 
#include <iostream>

void f2()
{
    std::cout << ".so Implementation of f2()\n";
};
```

To create a shared library from the above files:

```plain
g++ -fPIC -shared mod2.cpp -o libmod2.so
```

> `-fPIC` flag is for "Position Independent Code". It is required for g++ or gcc compiler but not required for clang++ or clang compiler.

Test driver:

```cpp
#include <iostream>
#include "mod2.h"

int main(int argc, char *argv[])
{
    f2();
    return EXIT_SUCCESS;
}
```

Now, to build the project:

```plain
g++ test_mod2.cpp -L. -lmod2
```

> `-L` flag is followed by the location where the library to be linked is located. Here, we put `.` because the library file is located in the current working directory.
>
> `-l` is short for "link". It is followed by the library name.

This may NOT work! It is because by default Ubuntu stores and looks for the libraries in the `/lib`,  `/usr/lib` and `/usr/local/lib` directories and your library file `libmod2` is not in there during the run-time. To work around this issue you can either:

* Copy your library file into one of those directories

* Run the following command in the current shell:

  ```plain
  $ export LD_LIBRARY_PATH=PATH_TO_LIBRARY_LIBMOD2
  ```

  This tells your OS to add `PATH_TO_LIBRARY_LIBMOD2` to its list of shared library search paths. (`export` command applies to your current shell only.)

  In your case,

  ```plain
  $ export LD_LIBRARY_PATH=~/projects/src/mod2
  ```



## Makefile

```makefile
# File Name		: makefile
# Description	: Makefile to 
#				  	- make mod2.h and mod2.cpp into a shared library libmod2.so and store it
#					  in the 'project/lib/' directory.
#					- create a directory 'project/test_build/'.
#					- build the project test_mod2.cpp and store the binary (executable) in the 
#					  'project/test_build/' directory.
#					- copy shared header files into 'project/shared_headers'
#			      The executable should be able to find the shared library libmod2 at run-time.
# Author		: Kyungjae Lee
# Date Created	: 01/01/2023

# project/src/mod2/makefile
MODULE_NAME = mod2
TEST_MODULE = test_mod2.cpp
TEST_BIN_DIR = ../../test_build
SHARED_HEADERS_DIR = ../../shared_headers

TARGET_DIR = ../../lib
TARGET_NAME = lib$(MODULE_NAME).so
TARGET = $(TARGET_DIR)/$(TARGET_NAME)

OBJ_DIR = ../../obj
OBJ1 = $(OBJ_DIR)/$(MODULE_NAME).o

CFLAGS = -fPIC -Wall -Werror
LDFLAGS = #it's okay to leave it empty

$(TARGET): $(OBJ1)
	g++ -shared $< -o $@ $(LDFLAGS)

$(OBJ1): $(MODULE_NAME).cpp
	g++ $(CFLAGS) -c $< -o $@

test:
	g++ $(TEST_MODULE) -o $(TEST_BIN_DIR)/$(MODULE_NAME) -L$(TARGET_DIR) -l$(MODULE_NAME)

clean:
	rm $(TARGET) $(OBJ1)

build_dir:
	mkdir -p $(TEST_BIN_DIR)
    
copy_shared_headers:
	cp *h $(SHARED_HEADERS_DIR)
```

```plain
$ make
g++ -fPIC -Wall -Werror -c mod2.cpp -o ../../obj/mod2.o
g++ -shared ../../obj/mod2.o -o ../../lib/libmod2.so 

$ make test
g++ test_mod2.cpp -o ../../test_build/mod2 -L../../lib -lmod2

$ ../../test_build/mod2 
.so Implementation of f2()

$ make copy_shared_headers
cp *.h ../../shared_headers
```





## References

Subrata, S. (2022). *GNU Make & Makefile To Build C/C++ Projects - (LINUX,MAC)* [Video file]. Retrieved from  https://www.udemy.com/course/gnu-make-makefile-to-build-cc-projects-linuxmac/

