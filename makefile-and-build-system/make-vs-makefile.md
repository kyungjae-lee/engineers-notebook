<a href="../">Notebook</a> > <a href="./">Makefile & Build System</a> > Make vs. Makefile

# Make vs. Makefile



## What are Make & Makefile?

* **Make** is a tool. 

  ```shell
  $ which make
  /usr/bin/make
  ```

  ```shell
  $ make		# correct; make is an executable
  $ makefile	# incorrect; makefile is NOT an executable
  ```

* **Makefile** is a file written in such a way that Make tool can understand.

* Make tool needs Makefile as the input and process as per the instructions written in it.

* "Makefile" or "makefile" both are valid notations.



## Why Makefile?

* Makefile is used for automating the build process.
* Make tool is very powerful in that it can detect the **modified source file(s)** and process/build file them **only** by running make. Compiling only the modified source files and linking them into the project can save huge amount of time when only a small number of source files has been modified in a big project.
  * **Time stamp** is used to detect the modified source files.
* In general, compilation is much slower process than linking. Make tool takes the advantage of this fact.



## Structure of a Makefile

Just like general C programs have the following structure:

```c
/* comments */
/* MACROS */
/* variable declarations / definitions */
/* function declarations / definitions */
/* instructions / statements */

int main() { ... }
```

Makefiles also have a general structure as follows:

```makefile
# comments
# MACROS / VARIABLES
# functions
# instructions / statements

TARGET	 # TARGET is nothing but a meaningful name we can give
```

For example:

```makefile
VARIABLE1
VARIABLE2

define my_func
	...
endef

TARGET: PREREQUISITE / DEPENDENCY										# Rule (optional)
	INSTRUCTIONS ON HOW TO MAKE/PREPARE THE TARGET USING PREREQUISITES	# Recipe (optional)
```

Note that the syntax for Makefiles requires that indented lines start with a **tab**, and not a space.



## Targets

* A makefile may have multiple targets.

  ```makefile
  target1: ...
  	...
  
  target2: ...
  	...
  ```

* By default, the first target gets executed.

  ```shell
  $ make			# runs the first (default) target
  $ make target2	# runs the specified target
  ```

* Use `.DEFAULT_GOAL` to override it.

  ```makefile
  .DEFAULT_GOAL = target2	# can be placed anywhere, but what is the convention?
  
  target1: ...
  	...
  
  target2: ...
  	...
  ```

  ```shell
  $ make			# runs target2 by default
  ```

  

## Phony Targets

A phony target is one that is not really the name of a file; rather it is just a name for a recipe to be executed when you make an explicit request. There are two reasons to use a phony target: 

* To avoid a conflict with a file of the same name
* To improve performance

If you write a rule whose recipe will not create the target file, the recipe will be executed every time the target comes up for remaking. For example:

```makefile
clean:
	rm *.o temp
```

Because the `rm` command does not create  a file named "clean", probably no such file will ever exist. Therefore, the `rm` command will be executed every time you run `make clean`. To avoid this problem you can explicitly declare the target to be **phony** by making it a prerequisite of the special target `.PHONY` as follows:

```makefile
.PHONY: clean
clean:
        rm *.o temp
```

Once this is done, `make clean` will run the recipe regardless of whether there is a file named `clean`.





## References

Subrata, S. (2022). *GNU Make & Makefile To Build C/C++ Projects - (LINUX,MAC)* [Video file]. Retrieved from  https://www.udemy.com/course/gnu-make-makefile-to-build-cc-projects-linuxmac/

