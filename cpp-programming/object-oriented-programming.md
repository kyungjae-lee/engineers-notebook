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
  };
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
  };
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

  

## Class Member Access Modifiers (`public`, `private`, `protected`)

* `public` - Accessible everywhere
* `private` - Accessible only by members or friends of the class
* `protected` - Used with inheritance

### Examples

* `Player` class

  ```cpp
  class Player
  {
  private:
      std::string name;
      int health;
      int xp;
  public:
      void talk(std::string text_to_say);
      bool is_dead();
  };
  ```

  ```cpp
  Player jack;
  jack.name = "Jack";		// Compiler error
  jack.health = 1000;		// Compiler error
  jack.talk("Hello!");	// OK
  
  Player *enemy = new Player();
  enemy->xp = 100;		// Compiler error
  enemy->talk("Got you!");// OK
  
  delete enemy;
  ```

* `Account` class

  ```cpp
  class Account
  {
  private:
      std::string name;
      double balance;
  public:
      bool withdraw(double amount);
      bool deposit(double amount);
  };
  ```

  ```cpp
  Account jack_account;
  jack_ccount.balance = 1000.00;			// Compiler error
  jack_account.deposit(1000.00);			// OK
  jack_account.name = "Jack's Account";	// Compiler error
  
  Account *sunny_account = new Account();
  
  sunny_account->balance = 10000.0;		// Compiler error
  sunny_account->withdraw(10000.0);		// OK
  
  delete sunny_account;
  ```

  

## Implementing Member Methods

* Very similar to how we implemented functions
* Member methods have access to member attributes (Don't need to pass them as arguments!)
* Can be implemented inside the class declaration (Implicitly inline)
* Can be implemented outside the class declaration (Need to use `Class_name::method_name`)
* Can be separate specification from implementation
  * `.h` file for the class declaration
  * `cpp` file for the class implementation

### Examples

* Implementing member methods INSIDE the class declaration

  ```cpp
  class Account
  {
  private:
      double balance;
  public:
      void set_balance(double bal) { balance = bal; }
      double get_balance() { return balance; }
  };
  ```

* Implementing member methods OUTSIDE the class declaration

  ```cpp
  class Account
  {
  private:
      double balance;
  public:
      void set_balance(double_bal);
      double get_balance();
  };
  
  void Account::set_balance(double bal) { balance = bal; }
  double Account::get_balance() { return balance; }
  ```

* Separating specification from implementation

  ```cpp
  /* Account.h */
  
  #pragma once
  
  #ifndef ACCOUNT_H
  #define ACCOUNT_H
  
  // Account class declaration
  class Account
  {
  private:
      double balance;
  public:
      void set_balance(double bal);
      double get_balance();
  };
  
  #endif
  ```

  ```cpp
  /* Account.cpp */
  
  #include "Account.h"
  
  void Account::set_balance(double bal) { balance = bal; }
  double Account::get_balance() { return balance; }
  ```

  

## Constructors

* Special member method
* Invoked during object creation
* Useful for initialization (to a stable state)
* Same name as the class
* No return type is specified
* Can be overloaded

### Examples

* `Player` class

  ```cpp
  class Player
  {
  private:
      std::string name;
      int health;
      int xp;
  public:
      // Overloaded constructors
      Player();
      Player(std::string name);
      Player(std::string name, int health, int xp);
  };
  ```

* `Account` class

  ```cpp
  class Account
  {
  private:
      std::string name;
      double balance;
  public:
      // Constructors
      Account();
      Account(std::string name, double balance);
      Account(std::string name);
      Account(double balance);
  };
  ```

  

## Destructors

* Special member method
* Same name as the class proceeded with a tilde (`~`)
* Invoked automatically when an object is destroyed
* No return type and no parameters
* Only 1 destructor is allowed per class - CANNOT be overloaded!
* Useful to release memory and other resources

### Example

* `Player` class

  ```cpp
  class Player
  {
  private:
      std::string name;
      int health;
      int xp;
  public:
      // Overloaded constructors
      Player();
      Player(std::string name);
      Player(std::string name, int health, int xp);
      // Destructor
      ~Player();
  };
  ```

  ```cpp
  {
      Player jack;
      Player sunny {"Sunny", 100, 4};
      Player yena {"Yena"};
  } // 3 destructors called
  
  Player *kj = new Player("KJ", 1000, 0);
  delete enemy;	// destructor called
  ```

* `Account` class

  ```cpp
  class Account
  {
  private:
      std::string name;
      double balance;
  public:
      // Constructors
      Account();
      Account(std::string name, double balance);
      Account(std::string name);
      Account(double balance);
      // Destructor
      ~Account();
  };
  ```

  

## Default Constructor

* Does not expect any arguments (Also, called the **no-args constructor**)
* If your write no constructors at all for a class
  * C++ will generate a default constructor that does nothing
* Called when you instantiate a new object with no arguments

### Examples

* `Player` class

  ```cpp
  Player jack;
  Palyer *sunny = new Player;
  ```

* `Account` class

  With default constructor

  ```cpp
  class Account
  {
  private:
      std::string name;
      double balance;
  public:
      // Default constructor
      Account()
      {
          name = "None";
          balance = 0.0;
      }
  	bool withdraw(double amount);
      bool deposit(double amount);
  };
  ```

  ```cpp
  Account jack_account;
  Account yena_account;
  
  Account *sunny_account = new Account;
  delete sunny_account;
  ```

  No default constructor

  ```cpp
  class Account
  {
  private:
      std::string name;
      double balance;
  public:
      // No default constructor
      Account(std::string name_val, double bal)
      {
          name = name_val;
          balance bal;
      }
  	bool withdraw(double amount);
      bool deposit(double amount);
  };
  ```

  ```cpp
  Account jack_account;					// Error
  Account yena_account;					// Error
  
  Account *sunny_account = new Account;	// Error
  delete sunny_account;
  
  Account kj_account {"KJ", 15000.0};		// OK
  ```



## Overloading Constructor

* Classes can have as many constructors as necessary
* Each must have a unique signature
* Default constructor is no longer compiler-generated once another constructor is declared

### Examples

* `Player` class

  ```cpp
  class player
  {
  private:
  	std::string name;
      int health;
      int xp;
  public:
      // Overloaded constructors
      Player();
      Player(std::string name_val);
      Player(std::string name_val, int health_val, int xp_val);
  };
  
  Player::Player()
  {
      name = "None";
      health = 0;
      xp = 0;
  }
  
  Player::Player(std::string name_val)
  {
      name = name_val;
      health = 0;
      xp = 0;
  }
  
  Player::Player(std::string name_val, int health_val, int xp_val)
  {
      name = name_val;
      health = health_val;
      xp = xp_val;
  }
  ```

  ```cpp
  Player empty;				// None, 0, 0
  Player hero("Hero");		// Hero, 0, 0
  Player villain("Villain");	// Villain, 0, 0
  Player frank {"Frank", 100, 4};					// Frank, 100, 4
  Player *enemy = new Player("Enemy", 1000, 0);	// Enemy, 1000, 0
  delete enemy;
  ```



## Constructor Initialization Lists

* So far, all data member values have been set in the constructor body
* Constructor initialization lists
  * Are more efficient
  * Initialization list immediately follows the parameter list
  * Initializes the data members as the object is created
  * Order of initialization is the order of declaration in the class

### Examples

```cpp
class player
{
private:
	std::string name;
    int health;
    int xp;
public:
    // Overloaded constructors
    Player();
    Player(std::string name_val);
    Player(std::string name_val, int health_val, int xp_val);
};

Player::Player()
    : name{"None"}, health{0}, sp{0}					// Initialization list
{ }

Player::Player::Player(std::string name_val)()
    : name{name_val}, health{0}, sp{0}					// Initialization list
{ }

Player::Player(std::string name_val, int health_val, int xp_val)
    : name{name_val}, health{health_val}, xp{xp_val};	// Initialization list
{ }
```



## Delegating Constructors

* Often the code for constructors is very similar
* Duplicated code can lead to errors
* C++ allows delegating constructors
  * Code for one constructor can call another in the initialization list
  * Avoids duplicating code

```cpp
class player
{
private:
	std::string name;
    int health;
    int xp;
public:
    // Overloaded constructors
    Player();
    Player(std::string name_val);
    Player(std::string name_val, int health_val, int xp_val);
};

Player::Player(std::string name_val, int health_val, int xp_val)
    : name{name_val}, health{health_val}, xp{xp_val};	// Initialization list
{ }

Player::Player()
    : Player{"None", 0, 0}		// Delegating constructor
{ }

Player::Player::Player(std::string name_val)()
    : Player{name_val, 0, 0}	// Delegating constructor
{ }
```

