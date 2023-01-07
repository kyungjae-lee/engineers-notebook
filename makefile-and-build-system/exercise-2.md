<a href="../">Notebook</a> > <a href="./">Makefile & Build System</a> > Exercise 2

# Exercise 2



## Requirements

Write a makefile to produce `mod1.o` and `mod1_bin` in `project/obj/` and `project/bin/`, respectively. Include a clean feature to delete `mod1.o` and `mod1_bin`. Write comments in your makefile.

```cpp
/* project/src/mod1/mod1.cpp */
#include <cstdio>
#include <cmath>

int main()
{
    printf("%lf\n", sqrt(14.0));
    
    return 0;
}
```



## Makefile

```makefile
# File Name		: makefile
# Description	: Makefile to compile mod1.cpp, 
#			      produce mod1.o and mod_bin in project/obj/ and project/bin/, repectively,
#				  and delete the produced outputs using 'clean'.
# Author		: Kyungjae Lee
# Date Created	: 01/01/2023

# project/src/mod1/makefile
MODULE_NAME = mod1

TARGET_DIR = ../../bin
TARGET_NAME = mod1_bin
TARGET = $(TARGET_DIR)/$(TARGET_NAME)

OBJ_DIR = ../../obj
OBJ1 = $(OBJ_DIR)/$(MODULE_NAME).o

CFLAGS = -Wall -Werror
LDFLAGS = -lm 

$(TARGET): $(OBJ1)
    g++ $< -o $@ $(LDFLAGS)

$(OBJ1): $(MODULE_NAME).cpp
    g++ $(CFLAGS) -c $< -o $@

clean:
    rm $(TARGET) $(OBJ1)
```

```plain
$ make	
g++ -Wall -Werror -c mod1.cpp -o ../../obj/mod1.o
g++ ../../obj/mod1.o -o ../../bin/mod1_bin -lm

$ make clean
rm ../../bin/mod1_bin ../../obj/mod1.o
```





## References

Subrata, S. (2022). *GNU Make & Makefile To Build C/C++ Projects - (LINUX,MAC)* [Video file]. Retrieved from  https://www.udemy.com/course/gnu-make-makefile-to-build-cc-projects-linuxmac/

