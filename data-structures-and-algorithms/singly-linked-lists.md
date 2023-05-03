<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Data Structures & Algorithms</a> > Singly-Linked Lists

# Singly-Linked Lists



## Singly-Linked List (C++)

### Interface

```c
//========================================================================================
// @ File name      : linked_list.h
// @ Description    : Interface for Linked List
// @ Author         : Kyungjae Lee
// @ File created   : 05/02/2023
//========================================================================================

// Class for list nodes
class Node
{
public:
    int value;
    Node *next;

    Node(int value);                    // Constructor
};

// Class for linked lists
class LinkedList
{
public:    
    // Public interface
    LinkedList(int value);              // Constructor
    void append(int value);             // Add a node at the end
    void prepend(int value);            // Add a node at the front
    bool insert(int index, int value);  // Insert a node into the given index position
    void deleteNode(int index);         // Delete a node with the given index position
    void deleteLast();                  // Delete the last node
    void deleteFirst();                 // Delete the first node
    Node* get(int index);               // Get the node value of the given index position
    bool set(int index, int value);     // Set the node value of the given index position
    void reverse();                     // Reverse the list
    void printList();                   // Print the list
    ~LinkedList();                      // Destructor

private:
    Node *head;
    Node *tail;
    int length;
};

```

### Implementation

```c
//========================================================================================
// @ File name      : linked_list.cpp
// @ Description    : Implementation of Linked List
// @ Author         : Kyungjae Lee
// @ File created   : 05/02/2023
//========================================================================================

#include <iostream>
#include "linked_list.h"

using namespace std;

//----------------------------------------------------------------------------------------
// Implementation of Node class interface
//----------------------------------------------------------------------------------------

// Constructor
Node::Node(int value)
{
    this->value = value;
    next = nullptr;
}

//----------------------------------------------------------------------------------------
// Implementation of LinkedList class interface
//----------------------------------------------------------------------------------------

// Constructor
LinkedList::LinkedList(int value)
{
    Node *newNode = new Node(value);
    head = newNode;
    tail = newNode;
    length = 1;
}

// Add a node at the end
void LinkedList::append(int value) 
{
    Node *newNode = new Node(value);

    // Insert a node into an empty list
    if (length == 0)    // (head == nullptr) or (tail == nullptr)
    {
        head = newNode;
        tail = newNode;
    }   
    // Insert a node into a non-empty list
    else
    {
        tail->next = newNode;
        tail = newNode;
    }

    length++;
}

// Add a node at the front
void LinkedList::prepend(int value)
{
    Node *newNode = new Node(value);

    // Insert a node into an empty list
    if (length == 0)
    {
        head = newNode;
        tail = newNode;
    }
    // Insert a node into a non-empty list
    else
    {
        newNode->next = head;
        head = newNode;
    }

    length++;
}

// Insert a node into the given index position
bool LinkedList::insert(int index, int value)
{
    // Validity check for index
    if (index < 0 || index > length)
        return false;

    // Insert a node at front (prepend)
    if (index == 0)
    {
        prepend(value);
        return true;
    }

    // Insert a node at the end (append)
    if (index == length)
    {
        append(value);
        return true;
    }

    // Insert a node somewhere in the middle
    Node *newNode = new Node(value);
    Node *temp = get(index - 1);
    newNode->next = temp->next;
    temp->next = newNode;

    length++;

    return true;
}

// Delete a node with the given index position
void LinkedList::deleteNode(int index)
{
    // Validity check for index
    if (index < 0 || index >= length)
        return;

    // Delete the first node
    if (index == 0)
        return deleteFirst();   // Available only when return types are the same

    // Delete the last node
    if (index == length - 1)
        return deleteLast();

    // Delete the node from somewhere in the middle
    Node *prev = get(index - 1);
    Node *temp = prev->next;
    prev->next = temp->next;
    delete temp;

    length--;
}

// Delete the last node
void LinkedList::deleteLast()
{
    // Do not allow deleting a node from an empty list
    if (length == 0)
        return;

    Node *temp = head;

    // If only 1 node in the list
    if (length == 1)
    {
        head = nullptr;
        tail = nullptr;
    }
    // If 2+ nodes in the list
    else
    {
        Node *prev = head;

        while (temp->next)
        {
            prev = temp;
            temp = temp->next;
        }

        tail = prev;
        tail->next = nullptr;
    }

    delete temp;
    length--;
}

// Delete the first node
void LinkedList::deleteFirst()
{
    // Do not allow deleting a node from an empty list
    if (length == 0)
        return;

    Node *temp = head;

    // If only 1 node in the list
    if (length == 1)
    {
        head = nullptr;
        tail = nullptr;
    }
    // If 2+ nodes in the list
    else
        head = head->next;

    delete temp;
    length--;
}

// Get the node value of the given index position
Node* LinkedList::get(int index)
{
    // Validity check for index
    if (index < 0 || index >= length)
        return nullptr;

    Node *temp = head;

    // Search for the node
    for (int i = 0; i < index; i++)
        temp = temp->next;

    return temp;
}

// Set the node value of the given index position
bool LinkedList::set(int index, int value)
{
    // Get the node
    Node *temp = get(index);

    // Set the value
    if (temp)
    {
        temp->value = value;
        return true;
    }

    return false;
}

// Reverse the list
void LinkedList::reverse()
{
    // Swap head and tail
    Node *temp = head;
    head = tail;
    tail = temp;

    Node *after = temp->next;
    Node *before = nullptr;

    // Iteratively reverse the direction of next pointer
    for (int i = 0; i < length; i++)
    {
        after = temp->next;
        temp->next = before;
        before = temp;
        temp = after;
    }
}

// Print the list
void LinkedList::printList()
{
    Node *temp = head;

    while (temp)
    {
        cout << temp->value << " ";
        temp = temp->next;
    }

    cout << endl;
}

// Destructor
LinkedList::~LinkedList()
{
    // head, tail, length will be destroyed by default, but the nodes will not.
    // So, make sure to delete them manually in the destructor.

    Node *temp = head;

    while (head)
    {
        head = head->next;
        delete temp;
        temp = head;
    }
}
```

### Test Driver

```c
//========================================================================================
// @ File name      : main.cpp
// @ Description    : Test driver for Linked List
// @ Author         : Kyungjae Lee
// @ File created   : 05/02/2023
//========================================================================================

#include <iostream>
#include "linked_list.h"

using namespace std;

void printInfo(LinkedList *list);

int main(int argc, char *argv[])
{
    // Create a list
    LinkedList *list = new LinkedList(2);
    
    // Prepend
    list->prepend(1);

    // Append
    list->append(4);
    list->append(5);
    list->append(6);

    // Insert (insert 3 at index 2)
    list->insert(2, 3); 

    // Print the list
    list->printList();      // 1 2 3 4 5 6

    // Delete first
    list->deleteFirst();

    // Delete last
    list->deleteLast();

    // Delete from the middle (index 1)
    list->deleteNode(1);

    // Print the list
    list->printList();      // 2 4 5

    // Set (value of the node at index 1 to 10)
    list->set(1, 10);

    // Get (value of the node at index 1 to 10)
    cout << list->get(1)->value << endl;    // 10

    // Reverse the list
    list->reverse();

    // Print the list
    list->printList();      // 5 10 2

    return 0;
}
```

```plain
1 2 3 4 5 6 
2 4 5 
10
5 10 2 
```
