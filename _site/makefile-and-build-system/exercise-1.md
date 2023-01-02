<a href="../">Notebook</a> > <a href="./">Makefile & Build System</a> > Exercise 1

# Exercise 1



## Requirements

1. Write a makefile to produce the directories d1, d2, d3.
2. Modify your makefile to produce the files f1.c, f2.c, f3.c.
3. Modify your makefile to delete f3.c and d3.
4. Suppress the error.
5. Modify your makefile to do Step 1 and Step 2 in a single run.
6. Write comments in your makefile.

Remember:

```makefile
TARGET: PREREQUISITE / DEPENDENCY										# Rule (optional)
	INSTRUCTIONS ON HOW TO MAKE/PREPARE THE TARGET USING PREREQUISITES	# Recipe (optional)
```



## Makefile

```makefile
# File Name		: makefile
# Description	: My first makefile to demonstrate how it works
# Author		: Kyungjae Lee
# Date Created	: 12/29/2022

all: make_dir create_files	# first (default) target that runs make_dir, create_files at once

make_dir:
	mkdir -p d1 d2 d3	# -p, --parent (no error if existing, make parent directories as needed)
	
create_files:
	touch f1.c f2.c f3.c
	
clean:
	-rm f3.c	# '-' added before rm to ignore the error
	rm -rf d3
```

```shell
$ make	# runs the first (default) target
mkdir -p d1 d2 d3
touch f1.c f2.c f3.c

$ ll
total 24
drwxrwxr-x 5 klee klee 4096 Dec 29 15:12 ./
drwxrwxr-x 3 klee klee 4096 Dec 29 13:38 ../
drwxrwxr-x 2 klee klee 4096 Dec 29 14:58 d1/
drwxrwxr-x 2 klee klee 4096 Dec 29 14:58 d2/
drwxrwxr-x 2 klee klee 4096 Dec 29 15:12 d3/
-rw-rw-r-- 1 klee klee    0 Dec 29 15:12 f1.c
-rw-rw-r-- 1 klee klee    0 Dec 29 15:12 f2.c
-rw-rw-r-- 1 klee klee    0 Dec 29 15:12 f3.c
-rw-rw-r-- 1 klee klee  308 Dec 29 15:12 makefile

$ make clean	# runs the specified target
rm f3.c	
rm -rf d3

$ ll
total 20
drwxrwxr-x 4 klee klee 4096 Dec 29 15:20 ./
drwxrwxr-x 3 klee klee 4096 Dec 29 13:38 ../
drwxrwxr-x 2 klee klee 4096 Dec 29 14:58 d1/
drwxrwxr-x 2 klee klee 4096 Dec 29 14:58 d2/
-rw-rw-r-- 1 klee klee    0 Dec 29 15:12 f1.c
-rw-rw-r-- 1 klee klee    0 Dec 29 15:12 f2.c
-rw-rw-r-- 1 klee klee  308 Dec 29 15:18 makefile
```





## References

Subrata, S. (2022). *GNU Make & Makefile To Build C/C++ Projects - (LINUX,MAC)* [Video file]. Retrieved from  https://www.udemy.com/course/gnu-make-makefile-to-build-cc-projects-linuxmac/

