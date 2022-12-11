<a href="../">Engineer's Notebook</a> > <a href="../cpp-stl">CPP STL</a> > [CPP STL] Iterators

# [CPP STL] Iterators



* An **iterator** is an object that can iterate over elements.    
  (These elements may be all or a subset of the elements of an STL container.)
* An iterator represents a certain *position* in a container.
* Every STL container defines two iterator types:
  1. `container::iterator` is provided to iterate over elements in **read/write** mode. 
  2. `container::const_iterator` is provided to iterate over elements in **read-only** mode. 
  
  For example:
  ```cpp
  #include <list>
  
  list<char>::iterator pos;         // iterator in read/write mode
  list<char>::const_iterator pos;   // iterator in read-only mode
  ```
  
### Various Operations and Usage

* Fundamental Operations
  ```plain
  Operation     Effect
  ============  ==========================================================================
  *pos          Returns the element of the current position pos (If the elements have
                members, operator -> can be used to access those members directly from the 
                iterator.)
  ++pos, pos++  Moves the iterator pos forward to the next element
  --pos, pos--  Moves the iterator pos backward to the previous element
  pos1 == pos2  Returns whether two iterators pos1 and pos2 represent the same position
  pos1 != pos2  Returns whether two iterators pos1 and pos2 represent the different
                positions
  pos = &elem   Assigns an iterator pos the position of the element elem
  ```
  [!] Note: The **pre-increment** is preferred to the postincrement since it might have
  better performance. (Post-increment internally involves a temporary object because it 
  must return the old position of the iterator.)
* When used to iterate over the elements of (unordered) maps and multimaps, `pos` would
  refer to key/value pairs. In this case:
  
  ```plain
  Operation     Effect
  ============  ==========================================================================
  pos->first    Yields the first part, (constant) key, of the key/value pair 
  pos->second   Yields the second part, value, of the key/value pair 
  ```
* All STL container classes provide the same basic member functions that enable them to
  use iterators to navigate over their elements. For example:

  ```plain
  Operation     Effect
  ============  ==========================================================================
  c.begin()     Returns an iterator that represents the beginning of the elements in the
                container (The beginning is the position of the first element, if any.)
  c.end()       Returns an iterator that represents the end of the elements in the
                container (The end is the position 'behind' the last element.)
                a.k.a. past-the-end iterator
  ```
  ![beg-end-container]({{site.baseurl}}/img/cpp-ds-algo/beg-end-container.png){:.inline-img-w350}

  [!] Note: `begin()` and `end()` define a *half-open range* that includes the first
  element but excludes the last; `[beg, end)`.   
  A half-open range has two advantages:   
  
  1. Loops can simply continue as long as `end()` is not reached.
  2. It avoids special handling for empty ranges. (For empty ranges, `begin()` is equal to
  `end()`.)
    - If the container has no elements, loop will not run.
  ^
* Using Iterators with `auto` Keyword (since C++11)
  ```cpp
  for (auto pos = l.begin(); pos != l.end(); ++pos) { ... }
  ```
  can be used in place of
  ```cpp
  for (list<char>::iterator pos = l.begin(); pos != l.end(); ++pos) { ... }
  ```
  Advantages of using iterators with `auto` keyword:
  1. Code is more condensed.
  2. The loop becomes more robust for such code modifications as changing the type of the
  container.  

  [!] Note: When initialized with `begin()` or `end()`, the iterator loses its 
  *constness*, which might raise the risk of unintended assignments. This is because 
  `begin()` returns an object of type `cont::iterator`.

  To ensure that constant iterators are still used, `cbegin()` and `cend()` are provided
  as container functions since C++11. They return an object of type
  `cont::const_iterator`. For example:
  ```cpp
  for (auto pos = l.cbegin(); pos != l.cend(); ++pos) { ... }   // ensures const iterator
  ```
* Range-Based `for` Loops vs. Iterators   
  For containers, a range-based `for` loop is a convenience interface defined to iterate
  over all elements of the passed range/collection. Within each loop body, the actual
  element is initialized by the value the current iterator refers to. For example:
  ```cpp
  for (int elem : c)    // let 'c' be a container of elements of type 'int'
  {
      ...
  }
  ```
  is interpreted as
  ```cpp
  for (auto pos = c.begin(), end = c.end(); pos! = end; ++pos)
  {
      int elem = *pos;
      ...
  }
  ```
  [!] Note: `elem` should be declared to be a constant reference to avoid unnecessary
  copies. Otherwise, `elem` will be initialized as a copy of `*pos`.

### Iterator Categories

* Iterators are subdivided into *categories* based on their general abilities. The
  iterators of predefined container classes belong to one of the following three
  categories:
    - **Forward iterators**
        - Iterate only forward, using the increment operator.
        - The iterators of the class `forward_list` are forward iterators.
        - e.g., Iterators of the container classes `unordered_set`, 
          `unordered_multiset`, `unordered_map`, and `unordered_multimap` are "at least"
          forward iterators (libraries are allowed to provide bidirectional iterators 
          instead).
    - **Bidirectional iterators**
        - Iterate in two directions: forward by using the increment operator, and
          backward by using the decrement operator.
        - e.g., Iterators of the continaer classes `list`, `set`, `multiset`, `map`, 
          and `multimap`
    - **Random-access iterators**
        - Have all the properties of bidirectional iterators plus *random access*.
        - Provide operators for *iterator arithmetic* (in accordance with *pointer
          arithmetic* of an ordinary pointer).
            - Adding/subtracting offsets, processing differences and comparing iterators
              by using relational operators such as `<` and `>` are possible.
        - e.g., Iterators of the container classes `vector`, `deque`, `array` and 
          iterators of `strings`
          ^
* In addition, two other iterator categories are defined:
    - **Input iterators**
        - Able to read/process some values while iterating forward.
        - e.g., Input stream iterators
    - **Output iterators**
        - Able to write some values while iterating forward.
        - e.g., Inserts and output stream iterators

### Examples of Using Iterators

```cpp
#include <list>
#include <iostream>

using namespace std;

int main(int argc, char *argv[])
{
    list<char> l;       // list container for character elements

    // append elements from 'a' to 'z' to the list 
    for (char c = 'a'; c <= 'z'; ++c)
    {
        l.push_back(c);
    }

    // make all characters in the list uppercase
    //  - iterate over all elements
    list<char>::iterator pos;           // modifying elements using 'pos' is allowed
    for (pos = l.begin(); pos != l.end(); ++pos)
    {
        *pos = toupper(*pos);
    }

    // print all elements:
    //  - iterate over all elements
    list<char>::const_iterator cpos;    // modifying elements using 'cpos' is not allowed
    for (cpos = l.begin(); cpos != l.end(); ++cpos)
    {
        cout << *cpos << ' ';
    }
    cout << endl;
    
    return 0;
}
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```
[!] Note: Instead of using a range-based `for` loops, following loops with the `auto` 
keyowrd can be used to produce the same results.
```cpp
    // make all characters in the list uppercase
    //  - iterate over all elements
    for (auto elem : l)
    {
        elem = toupper(elem);
    }

    // print all elements:
    //  - iterate over all elements
    for (auto elem : l)
    {
        cout << elem << ' ';
    }
```