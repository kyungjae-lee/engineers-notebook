<a href="../">Notebook</a> > <a href="../problem-solving">Operating Systesms</a> > Quick Notes

# Quick Notes

Following notes will be moved into appropriate sections later.



### fork() and exec()

The `fork()` system call is used to create a separate, duplicate process.

When an `exec()` system call is invoked, the program specified in the parameter to `exec()` will replace the entire process - including all threads.

**fork() example:**

```c
#include <stdio.h>
#include <unistd.h>	// fork(), getpid()
#include <stdlib.h>

int main(int argc, char *argv[])
{
    printf("PID of current process = %d\n", getpid());
    
    if (fork() == 0)	// if child process
    {
        printf("PID of child process = %d\n", getpid());
    }
    else				// if parent process
    {
        printf("PID of parent process = %d\n", getpid());
    }
    
    return 0;
}
```

```plain
PID of current process = 16896
PID of parent process = 16896
PID of child process = 16897
```

**exec() example:**

```c
// ex1.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    printf("PID of ex1.c = %d\n", getpid());
    char *args[] = {"arg1", "arg2", "arg3", NULL};
    execv("./ex2", args);
    printf("Back to ex1.c");
    return 0;
}
```

```c
// ex2.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    printf("Now in ex2.c\n");
    printf("PID of ex2.c = %d\n", getpid());
    return 0;
}
```

When the program ex1.c is run:

```plain
PID of ex1.c = 17147
Now in ex2.c
PID of ex2.c = 17147
```

`execv()` system call completely replaces the current process with the passed process (as an argument). Therefore ex1.c process is replaced by ex2.c process when `execv()` is invoked, and the rest of the ex1.c code will not have chance to get executed. 

Also, note that the printed PIDs are the same. This is because `execv()` system call simply replaces the flow of the process not creating another process.