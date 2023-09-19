<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Object-Oriented Programming

# Object-Oriented Programming



## Overview

* What is Object-Oriented Programming?
* What are classes and objects?
* Declaring classes and creating objects
* Dot and pointer operations
* `public` and `private` access modifiers
* Methods, constructors and desctructors
  * `class` methods
  * Default and overloaded constructors
  * Copy and move constructors
  * Shallow vs. deep copy
  * `this` pointer
* `static` class members
* `struct` vs. `class`
* `friend` of a class



## Procedural Programming

* Focus on processes or actions that a program takes
* Programs are typically collection of functions
* Data is declared separately
* Data is passed as arguments into functions
* Fairly easy to learn

### Limitations

* Functions need to know the structure of the data.
  * If the structure of the data changes, many functions must be changed.

* As programs get larger they become more:
  * Difficult to understand
  * Difficult to maintain
  * Difficult to extend
  * Difficult to debug
  * Difficult to reuse code
  * Fragile and easier to break



## Object-Oriented Programming

* Classes and objects
  * Focus is on classes that model real-world domain entities
  * Allows developers to think at a higher level of abstraction
  * Used successfully in very large programs
* Encapsulation
  * Objects contain **data** and **operations** that work on that data
  * Abstract Data Type (ADT)
* Information-hiding
  * Implementation-specific logic can be hidden
  * Users of the class code to the interface since they don't need to know the implementation
  * More abstraction
  * Easier to test, debug, maintain and extend
* Reusability
  * Easier to reuse classes in other applications
  * Faster development
  * Higher quality
* Inheritance
  * Can create new classes in terms of existing classes
  * Reusability
  * Polymorphic classes
* Polymorphism and more...

### Limitations

* Not a panacea
  * OO Programming won't make bad code better
  * Not suitable for all types of problems
  * Not everything decomposes to a class
* Learning curve
  * Usually steeper leaning curve, especially for C++
  * Many OO languages, many variations of OO concepts
* Design
  * Usually more up-front design is necessary to create good models and hierarchies
* Programs can be:
  * Larger in size
  * Slower
  * More complex



## Classes and Objects

### Classes

* Blueprint from which objects are created
* A user-defined data-type
* Has attributes (data; attributes)
* Has methods (functions; behaviors)
* Can hide data and methods
* Provides a public interface

* Examples:
  * `Account`
  * `Employee`
  * `Image`
  * `std::vector`
  * `std::string`

### Objects

* Created from a class
* Represents a specific instance of a class
* Can create many, many objects
* Each has its own identity
* Each can use the defined class methods

* Examples:

  * Jack's account is an instance of an `Account`.
  * Sunny's account is an instance of an `Account`.

  Each has its own balance, can make deposits, withdrawals, etc.

### Examples

* `Player` class and objects

  ```cpp
  // Class declaration
  
  class Player
  {
      // Attributes
      std::string name;
      int health;
      int xp;
      
      // Methods
      void talk(std::string text_to_say);
      bool is_dead();
  }
  ```

  ```cpp
  // Object instantiataion
  
  Player jack;
  Player hero;
  
  Player *enemy = new Player();
  delete enemy;
  ```

* `Account` class and objects

  ```cpp
  // Class declaration
  
  class Account
  {
      // Attributes
      std::string name;
      double balance;
      
      // Method
      bool withdraw(double amount);
      bool deposit(double amount);
  }
  ```

  ```cpp
  Account jack_account;
  Account sunny_account;
  
  Account *yena_account = new Account();
  delete yena_account;
  
  Account accounts[] {jack_account, sunny_account};
  
  std::vector<Account> accounts1 {jack_account};
  accounts1.push_back(sunny_account);
  ```

  

## Accessing Class Members

* You can access **class attributes** and **class methods**.

* Some class members will not be accessible

* Need an object to access instance variables

* If you have an **object**, you can access the class members by using the **dot operator**.

  ```cpp
  Account jack_account;
  
  jack_account.balance;
  jack_account.deposit(1000.00);
  ```

* If you have a **pointer to an object**, you can access the class members by using the **arrow operator (member of pointer operator)**.

  ```cpp
  Account jack_account = new Account();
  
  // 1. Dereference the poiner, then use the dot operator.
  (*jack_account).balance;
  (*jack_account).deposit(1000.00);
  
  // 2. Use the arrow operator (member of pointer operator)
  jack_account->balance;
  jack_account->deposit(1000.00);
  ```

  
