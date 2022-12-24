<a href="../">Notebook</a> > <a href="./">Data Structures</a> > Queues (C)

# Queues (C)



## Queue (Using Singly-Linked List)

### Interface

```c
/*
 *  File Name   : lqueue.h
 *  Description : Interface for queue using singly-linked list (in C)
 *  Author      : Kyungjae Lee
 *  Date Created: 12/24/2022
 */

#ifndef LQUEUE_H
#define LQUEUE_H

#include <stdbool.h>    /* C99 only */

typedef int Data;

/* structure for queue (using list) elements */
typedef struct LQueueElem_
{
    Data data;
    struct LQueueElem_ *next;
} LQueueElem;

/* structure for queue (using list) */
typedef struct LQueue_
{
    int elem_count;
    LQueueElem *head;
    LQueueElem *tail;
} LQueue;

/* public interface (ADT) */
void init(LQueue *lqueue);    
void enqueue(LQueue *lqueue, Data data);  
void dequeue(LQueue *lqueue);
Data front(LQueue *lqueue);
Data back(LQueue *lqueue);
int size(LQueue *lqueue);
bool empty(LQueue *lqueue);
void clear(LQueue *lqueue);

#endif
```

### Implementation

```c
/*
 *  File Name   : lqueue.c
 *  Description : Implementation of queue using singly-linked list (in C)
 *  Author      : Kyungjae Lee
 *  Date Created: 12/24/2022
 */

#include <stdio.h>    
#include <stdlib.h>     /* malloc */
#include <string.h>     /* memset */
#include "lqueue.h"

/* initializes the queue */
void init(LQueue *lqueue)
{
    lqueue->elem_count = 0;
    lqueue->head = NULL;
    lqueue->tail = NULL;
}

/* inserts an element into the queue */
void enqueue(LQueue *lqueue, Data data)
{
    /* create a new node to insert */
    LQueueElem *new_elem;

    if ((new_elem = (LQueueElem*)malloc(sizeof(LQueueElem))) == NULL)
    {
        /* no more elements can be created (memory allocation fail) */
        printf("Error: Queue overflow.\n");
        exit(1);
    }

    new_elem->data = data;
    new_elem->next = NULL;  /* mandatory */

    /* insert the new element into the queue (enqueue to the tail) */
    if (lqueue->head == NULL)   /* lqueue->tail == NULL, size(lqueue) == 0 */
    {
        lqueue->head = new_elem;
        lqueue->tail = new_elem;
    }
    else
    {
        lqueue->tail->next = new_elem;
        lqueue->tail = new_elem;
    }
    
    /* update the element counter to account for the inserted element */
    lqueue->elem_count++;
}

/* removes an element from the queue (does not return it) */
void dequeue(LQueue *lqueue)
{
    LQueueElem *rem_elem;

    if (lqueue->head == NULL)   /* lqueue->tail == NULL, size(lqueue) == 0 */
    {
        /* do not allow dequeue operation on an empty queue */
        printf("Error: Queue underflow.\n");
        exit(1);
    }

    /* remove the new element from the queue (dequeue from the head) */
    rem_elem = lqueue->head;

    if ((lqueue->head = lqueue->head->next) == NULL)
        lqueue->tail = NULL;    /* handle removing the last element from the queue */

    free(rem_elem);
    rem_elem = NULL;

    /* update the element counter to account for the inserted element */
    lqueue->elem_count--;
}

/* returns the next element (inserted first) in the queue without removing it */
Data front(LQueue *lqueue)
{
    if (lqueue->head == NULL)   /* lqueue->tail == NULL, size(lqueue) == 0 */
    {
        /* do not allow front operation on an empty queue */
        printf("Error: Queue underflow.\n");
        exit(1);
    }

    return lqueue->head->data;
}

/* returns the last element (inserted last) in the queue without removing it */
Data back(LQueue *lqueue)
{
    if (lqueue->head == NULL)   /* lqueue->tail == NULL, size(lqueue) == 0 */
    {
        /* do not allow back operation on an empty queue */
        printf("Error: Queue underflow.\n");
        exit(1);
    }

    return lqueue->tail->data;
}

/* returns the current number of elements */
int size(LQueue *lqueue)
{
    return lqueue->elem_count;
}

/* returns whether the queue is empty */
bool empty(LQueue *lqueue)
{
    return lqueue->elem_count == 0;
}

/* removes all elements from the queue */
void clear(LQueue *lqueue)
{
    /* remove all elements from the queue */
    //
    while (size(lqueue))    /* lqueue->head != NULL, lqueue->tail != NULL */
        dequeue(lqueue);

    /* clear the structure as a precaution */
    memset(lqueue, 0, sizeof(LQueue));    
}
```

### Test Driver

```c
/*
 *  File Name   : lqueue_main.c
 *  Description : Test driver for queue using singly-linked list (in C)
 *  Author      : Kyungjae Lee
 *  Date Created: 12/24/2022
 */

#include <stdio.h>
#include "lqueue.h"

int main(int argc, char *argv[])
{
    LQueue q;
    int i;

    init(&q);

    printf("%d\n", size(&q));   // 0
    printf("%d\n", empty(&q));  // 1
    //front(&q);                  // Error: Stack underflow
    //back(&q);                   // Error: Stack underflow
    //dequeue(&q);                // Error: Stack underflow

    for (i = 0; i < 5; ++i)
        enqueue(&q, i); 

    printf("%d\n", size(&q));   // 5
    printf("%d\n", front(&q));  // 0
    printf("%d\n", back(&q));   // 4

    dequeue(&q);
    dequeue(&q);
    dequeue(&q);
    
    printf("%d\n", size(&q));   // 2
    printf("%d\n", front(&q));  // 3
    printf("%d\n", back(&q));   // 4
    printf("%d\n", empty(&q));  // 0
    
    clear(&q);
    printf("%d\n", size(&q));   // 0
    printf("%d\n", empty(&q));  // 1

    return 0;
}
```

```plain
0
1
5
0
4
2
3
4
0
0
1
```

