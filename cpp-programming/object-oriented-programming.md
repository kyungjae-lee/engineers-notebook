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



## Default Constructor Parameters

* Can often simplify our code and reduce the number of overloaded constructors
* Same rules apply as we learned with non-member functions

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
      // Constructor with default parameter values
      Player(std::string name_val = "None",
             int health_val = 0,
             int xp_val = 0);
  };
  
  Player::Player(std::string name_val, int health_val, int xp_val)
      : name{name_val}, health{health_val}, xp{xp_val}
  { }
  
  Player empty;					// None, 0, 0
  Player jack{"Jack"};			// Jack, 0, 0
  Player yena{"Yena", 100, 55};	// Yena, 100, 55
  Player hero{"Hero", 100};		// Hero, 100, 0
  ```

  > Note what happens if you declare a no-args constructor



## Copy Constructor

* When objects are copied C++ must create a new object from an existing object
* When is a copy of an object made?
  * Passing object by value as a parameter
  * Returning an object from a function by value
  * Constructing one object based on another of the same class
* C++ must have a way of accomplishing this so it provides a compiler-defined copy constructor if you don't. (If you don't provide your own way of copying objects by value then the compiler provides a default way of copying objects.)
* Copies the values of each data member to the new object
  * Default member-wise copy
* Perfectly fine in many cases, but beware if you have a pointer data member
  * Pointer will be copied
  * Not what it is pointing to
  * Shallow copy vs. Deep copy
* Best practices
  * Provide a copy constructor when your class has raw pointer members
  * Provide the copy constructor with a **const reference** parameter
  * Use STL classes as they already provide copy constructors
  * Avoid using raw pointer data members if possible

### Examples

* Pass object by value

  ```cpp
  Player hero{"Hero", 100, 20};
  
  void display_player(Player p)
  {
      // p is a COPY of hero in this example
      // Use p
      // Destructor for p will be called
  }
  
  display_player(hero);
  ```

* Construct one object based on another

  ```cpp
  Player hero{"Hero", 100, 100};
  Player another_hero{hero};			// A COPY of hero is made
  ```

* Declaring and implementing a copy constructor

  ```cpp
  // Declaration
  Type::Type(const Type &source);
  
  // Implementation
  Type::Type(const Type &source)
  {
      // Code or initialization list to copy the object
  }
  ```

  ```cpp
  // Declaration
  Player::Player(const Player &source);
  Account::Account(const Account &source);
  
  // Implementation
  Player::Player(const Player &source)
      : name{source.name}, health{source.health}, xp{source.xp}
  { }
  
  Account::Account(const Account &source)
      : name{source.name}, balance{source.balance}
  { }
  ```

  

## Shallow Copy vs. Deep Copy

* Consider a class that contains a pointer as a data member. Constructor allocates storage dynamically and initializes the pointer. Destructor releases the memory allocated by the constructor. What happens in the default copy constructor?

### Default Copy Constructor (Shallow Copy)

* Member-wise copy
* Each data member is copied from the source object.
* The pointer is copied, NOT what it points to (shallow copy)
* **Problem**
  * The source and the newly created object BOTH point to the SAME data area!
  * When we release the storage in the destructor, the other object still refers to the released storage!

### User-Defined Copy Constructor (Deep Copy)

* Create a copy of the pointed-to-data, NOT just the pointer itself.
* Each copy will have a pointer to UNIQUE storage in the heap.
* Deep copy when you have a raw pointer as a class data member.

### Example

* Shallow copy

  ```cpp
  class Shallow
  {
  private:
      int *data;
  public:
      Shallow(int d);					// Constructor
      Shallow(const Shallow &source);	// Copy constructor (shallow copy)
      ~Shallow();						// Destructor
  };
  
  // Constructor
  Shallow::Shallow(int d)
  {
      data = new int;		// Dynamically allocate storage
      *data = d;
  }
  
  // Destructor
  Shallow::~Shallow()
  {
      delete data;		// Free storage
      cout << "Destructor freeing data" << endl;
  }
  
  // Copy constructor (shallow copy)
  Shallow::Shallow(const Shallow &source)
      : data(source.data)
  {
  	cout << "Copy constructor (shallow)" << endl;        
  }
  ```

  The following sample main will likely to crash.

  ```cpp
  int main()
  {
      Shallow obj1{100};
      display_shallow(obj1);
      // obj1's data has been released!
      
      obj1.set_data_value(1000);
      Shallow obj2{obj1};
      cout << "Hello world" << endl;
      
      return 0;
  }
  ```

* Deep copy

  ```cpp
  class Deep
  {
  private:
      int *data;
  public:
      Deep(int d);				// Constructor
      Deep(const Deep &source);	// Copy constructor (deep copy);
      ~Deep();					// Destructor
  };
  
  // Constructor
  Deep::Deep(int d)
  {
      data = new int;		// Dynamically allocate storage
      *data = d;
  }
  
  // Destructor
  Deep::~Deep()
  {
      delete data;		// Free storage
      cout << "Destructor freeing data" << endl;
  }
  
  // Copy constructor (deep copy)
  Deep::Deep(const Deep &source)
  {
      data = new int;		// Dynamically allocate storage
      *data = *source.data;
  	cout << "Copy constructor (deep)" << endl;        
  }
  ```

  ```cpp
  // Copy constructor (deep copy) - Delegating constructor
  Deep::Deep(const Deep &source)
      : Deep{*source.data}	// Delegating to Deep(int)
  {
  	cout << "Copy constructor (deep)" << endl;        
  }
  ```

  The following method causes no issue.

  ```cpp
  void display_deep(Deep s)
  {
      cout << s.get_data_value() << endl;
  }
  ```

  > When `s` goes out of scope, the destructor is called and releases `data`.
  > No problem since the storage being released is unique to `s`

  Also, the following sample main will not crash.

  ```cpp
  int main()
  {
      Deep obj1{100};
      display_deep(obj1);
      
      obj1.set_data_value(1000);
      Deep obj2{obj1};
      
      return 0;
  }
  ```

  

## Move Constructor

* Sometimes when we execute code the compiler creates unnamed temporary values:

  ```cpp
  int total{0};
  total = 100 + 200;
  ```

  > 1. `100 + 200` is evaluated and `300` stored in an unnamed `temp` value
  >
  > 2. The `300` is then stored in the variable `total`
  >
  > 3. Then the `temp` value is discarded
  >
  > The same happens with objects as well.

* Sometimes copy constructors are called many times automatically due to the copy semantics of C++.
* Copy constructors doing deep copying can have a significant performance bottleneck.
* C++11 introduced move semantics and the move constructor.
* Move constructor moves an object rather than copy it.
* Optional but recommended when you have a raw pointer.
* Copy elision - C++ may optimize copying away completely (RVO - Return Value Optimization)

### What Does Move Constructor Do?

* Instead of making a deep copy of the resource
  * It **moves** the resource
  * Simply copies the address of the resource from source to the current object
  * And, nulls out the pointer in the source pointer.
* Very efficient

### R-Value References

* Used in moving semantics and perfect forwarding
* Move semantics is a ll about r-value references
* Used by move constructor and move assignment operator to efficiently move an object rather than copy it.
* R-value reference operator (`&&`)

### Examples

* R-value references

  ```cpp
  // Syntax
  Type::Type(Type &&source);
  
  Player::Player(Player &&source);
  Move::Move(Move &&source);
  ```

  ```cpp
  int x{100};
  
  int &l_ref = x;		// L-value reference
  l_ref = 10;			// Change x to 10
  
  int &&r_ref = 200;	// R-value reference
  r_ref = 300;		// Change r_ref to 300
  
  int && x_ref = x;	// Compiler error
  ```

* R-value reference parameters

  ```cpp
  int x{100};				// x is an l-value
  
  void func(int &&num);	// B
  
  func(200);				// Calls B (200 is an r-value)
  func(x);				// Error (x is an l-value)
  						// - Cannot bind r-value reference of type 'int&&' to 
  						//   an l-value of type 'int'
  ```

* L-value reference parameters

  ```cpp
  int x{100};				// x is an l-value
  
  void func(int &num);	// A
  
  func(x);				// Calls A (x is an l-value)
  func(200);				// Error (200 is an r-value)
  						// - Cannot bind non-const l-value reference of type 'int&' to 
  						//   an r-value of type 'int'
  ```

* L-value and r-value reference parameters

  ```cpp
  int x{100};				// x is an l-value
  
  void func(int &num);	// A
  void func(int &&num);	// B
  
  func(x);				// Calls A (x is an l-value)
  func(200);				// Calls B (200 is an r-value)
  ```

* `Move` class

  ```cpp
  class Move
  {
  private:
      int *data;
  public:
      void set_data_value(int d)	{ *data = d; }
      int get_data_value()		{ return *data }
      Move(int d);				// Constructor
      Move(const Move &source);	// Copy constructor
      ~Move();					// Destructor
  };
  
  // Copy constructor
  Move::Move(const Move &source)
  {
      data = new int;
      *data = *source.data;
  }
  
  // Move constructor
  Move::Move(Move &&source)
      : data{source.data}
  {
      source.data = nullptr;
  }
  ```

  > The move constructor 'steals' the data and nulls out the source pointer.

  Inefficient copying (Copy constructors will be called to copy the temps)

  ```cpp
  vector<Move> vec;
  
  vec.push_back(Move{10});
  vec.push_back(Move{20});
  ```

  ```plain
  Constructor for: 10
  Constructor for: 10
  Copy constructor (deep copy) for: 10
  Destructor freeing data for: 10
  Constructor for: 20
  Constructor for: 20
  Copy constructor (deep copy) for: 20
  Constructor for: 10
  Copy constructor (deep copy) for: 10
  Destructor freeing data for: 10
  Destructor freeing data for: 20
  ```

  Efficient copying (Move constructors will be called for the temp r-values)

  ```cpp
  vector<Move> vec;
  
  vec.push_back(Move{10});
  vec.push_back(Move{20});
  ```

  ```plain
  Constructor for: 10
  Move constructor (moving resource): 10
  Destructor freeing data for nullptr
  Constructor for: 20
  Move constructor (moving resource): 20
  Move constructor (moving resource): 10
  Destructor freeing data for nullptr
  Destructor freeing data for nullptr
  Destructor freeing data for: 10
  Destructor freeing data for: 20
  ```

  

## `this` Pointer

* `this` is a reserved keyword
* Contains the address of the object, so it's a pointer to the object
* Can only be used in class scope
* All member access is done via the `this` pointer
* Can be used by the programmer
  * To access data member and methods
  * To determine if two objects are the same
  * Can be dereferenced (`*this`) to yield the current object
  * When overloading the assignment operator

### Examples

* `Account` class

  ```cpp
  void Account::set_balance(double bal)
  {
      balance = bal;	// 'this->balance' is implied
  }
  ```

  Can be used to disambiguate identifier

  ```cpp
  void Account::set_balance(double balance)
  {
      // balance = balance		// Ambiguous
      this->balance = balance;	// Unambiguous
  }
  ```

* To determine object identity

  Sometimes it is useful to know if two objects are the same object

  ```cpp
  int Account::compare_balance(const Account &other)
  {
      if (this == &other)
          std::cout << "The same objects" << std::endl;
      ...
  }
  
  jack_account.compare_balance(jack_account);
  ```

  

## Using `const` with Classes

* Pass arguments to class member methods as `const`
* We can also create `const` objects - It's attributes cannot change.
* What happens if we call member functions on `const` objects?
* `const` - Correctness

### Examples

* `const` object

  ```cpp
  const Player villain {"Villain", 100, 55};
  
  void display_player_name(const Player &p)
  {
      cout << p.get_name() << endl;
  }
  
  // What happens when you call member methods on a const object
  villain.set_name("Nice guy");					// ERROR
  std::cout << villain.get_name() << std::endl;	// ERROR
  display_player_name(villain);					// ERROR
  ```

* `const` methods

  ```cpp
  class Player
  {
  private:
      . . .
  public:
      std::string get_name() const;
      // ERROR if code in get_name modifies this object
      . . .
  };
  ```

  

## `static` Class Members

* Class data members can be declared as `static`
  * A single data member that belongs to the class, not the objects
  * Useful to store class-wide information
* Class functions can be declared as `static`
  * Independent of any objects
  * Can be called using the class name

### Examples

* `Player` class -  `static` members

  ```cpp
  class Player
  {
  private:
      static int num_players;
  
  public:
      static int get_num_players();
      . . .
  }
  ```

  Initializing the `static` data

  ```cpp
  #include "Player.h"
  
  int Palyer::num_players = 0;
  ```

  Implementing `static` method

  ```cpp
  int Player::get_num_players()
  {
      return num_players;
  }
  ```

* `Player` class - Update the constructor and destructor

  ```cpp
  Player::Player(std::string name_val, int health_val, int xp_val)
      : name{name_val}, health{health_val}, xp{xp_val}
  {
      ++num_players;
  }
  
  Player::~Player()
  {
      --num_players;
  }
  ```



## Structs vs Classes

* In addition to define a `class` we can declare a `struct`.
* `struct` comes from the C programming language.
* Essentially the same as a class except that members are `public` by default.

### General Guidelines

* `struct`
  * Use `struct` for passive objects with public access
  * Don't declare methods in `struct`
* `class`
  * Use class for active objects with private access
  * Implement getters/setters as needed
  * Implement member methods as needed

### Examples

* Class

  ```cpp
  class Person
  {
      std::string name;
      std::string get_name;
  };
  
  Person p;
  p.name = "Frank";			// Compiler ERROR - private
  std::cout << p.get_name();	// Compiler ERROR - private
  ```

* Struct

  ```cpp
  struct Person
  {
      std::string name;
      std::string get_name();	
  };
  
  Person p;
  p.name = "Frank";			// OK - public
  std::cout << p.get_name();	// OK - public
  ```
