<a href="../">Notebook</a> > <a href="./">Data Structures</a> > Stacks (C)

# Stacks (C)



## Stack (Using Singly-Linked List)

### Interface

```c
/*
 *  File Name   : lstack.h
 *  Description : Interface for stack using singly-linked list (in C)
 *  Author      : Kyungjae Lee
 *  Date Created: 12/23/2022
 */

#ifndef LSTACK_H
#define LSTACK_H

#include <stdbool.h>    /* C99 only */

typedef int Data;

/* structure for stack (using list) elements */
typedef struct LStackElem_
{
    Data data;
    struct LStackElem_ *next;
} LStackElem;

/* structure for stack (using list) */
typedef struct LStack_
{
    int elem_count;
    LStackElem *head;
} LStack;

/* public interface (ADT) */
void init(LStack *lstack);
void push(LStack *lstack, Data data);
void pop(LStack *lstack);
Data top(LStack *lstack);
int size(LStack *lstack);
bool empty(LStack *lstack);
void clear(LStack *lstack);

#endif
```

### Implementation

```c
/*
 *  File Name   : lstack.c
 *  Description : Implementation of stack using singly-linked list list (in C)
 *  Author      : Kyungjae Lee
 *  Date Created: 12/23/2022
 */

#include <stdio.h>
#include <stdlib.h>     /* malloc */
#include <string.h>     /* memset */
#include "lstack.h"

/* initializes stack */
void init(LStack *lstack)
{
    /* initialize members */
    lstack->elem_count = 0;
    lstack->head = NULL;
}

/* inserts an element into the stack */
void push(LStack *lstack, Data data)
{
    /* create a new node to insert */
    LStackElem *new_elem;

    if ((new_elem = (LStackElem*)malloc(sizeof(LStackElem))) == NULL)
    {
        /* no more elements can be created (memory allocation fail) */
        printf("Error: Stack overflow.\n");
        exit(1);
    }

    new_elem->data = data;
    new_elem->next = NULL;	/* optional */
    
    /* insert the new element into the stack */
    new_elem->next = lstack->head;
    lstack->head = new_elem;

    /* update the element counter to account for the inserted element */
    lstack->elem_count++;
}

/* removes an element from the stack (does not return it) */
void pop(LStack *lstack)
{
    LStackElem *rem_elem;

    if (empty(lstack))
    {
        /* do not allow pop operation on an empty stack */
        printf("Error: Stack underflow.\n");
        exit(1);
    }

    /* remove the top element */
    rem_elem = lstack->head;
    lstack->head = lstack->head->next;
    free(rem_elem);
    rem_elem = NULL;

    /* update the element counter to account for the inserted element */
    lstack->elem_count--;
}

/* returns the top element of the stack (does not remove it) */
Data top(LStack *lstack)
{
    /* do not allow top operation on an empty stack */
    if (empty(lstack))
    {
        printf("Error: Stack underflow.\n");
        exit(1);
    }

    return lstack->head->data;
}

/* returns the current number of elements */
int size(LStack *lstack)
{
    return lstack->elem_count;
}

/* returns whether the stack is empty (equivalent to size() == 0) */
bool empty(LStack *lstack)
{
    return lstack->elem_count == 0;	/* lstack->head == NULL, size(lstack) == 0 */
}

/* removes all elements from the stack */
void clear(LStack *lstack)
{
    /* remove all elements */
    while (!empty(lstack))
        pop(lstack);

    /* clear the structure as a precaution */
    memset(lstack, 0, sizeof(LStackElem));
}
```

### Test Driver

```c
/*
 *  File Name   : lstack_main.c
 *  Description : Test driver for stack using singly-linked list (in C)
 *  Author      : Kyungjae Lee
 *  Date Created: 12/23/2022
 */

#include <stdio.h>
#include "lstack.h"

int main(int argc, char *argv[])
{
    LStack s;
    int i;

    init(&s);

    printf("%d\n", size(&s));   // 0
    printf("%d\n", empty(&s));  // 1
    //top(&s);                    // Error: Stack underflow.
    //pop(&s);                    // Error: Stack underflow.

    for (i = 0; i < 5; ++i)
        push(&s, i); 

    printf("%d\n", size(&s));   // 5
    printf("%d\n", top(&s));    // 4

    pop(&s);
    pop(&s);
    pop(&s);
    
    printf("%d\n", size(&s));   // 2
    printf("%d\n", top(&s));    // 1
    printf("%d\n", empty(&s));  // 0
    
    clear(&s);
    printf("%d\n", size(&s));   // 0
    printf("%d\n", empty(&s));  // 1

    return 0;
} 
```

```plain
0
1
5
4
2
1
0
0
1
```

