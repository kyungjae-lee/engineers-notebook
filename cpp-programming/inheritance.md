<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Inheritance

# Inheritance



## Introduction

* What is inheritance? Why is it useful?
* Terminology and notation
* Inheritance vs. Composition
* Deriving classes from existing classes
  * Types of inheritance

* Protected members and class access
* Constructors and destructors
  * Passing arguments to base class constructors
  * Order of constructor and destructors calls

* Redefining base class methods
* Class hierarchies
* Multiple inheritance



## What is Inheritance and Why is it Used?

* Provides a method for creating new classes from existing classes

* The new class contains the data and behaviors of the existing class

* Allow for reuse of existing classes

* Allows us to focus on the common attributes among a set of classes

* Allows new classes to modify behaviors or existing classes to make it unique without actually modifying the original class

### Examples of Related Classes

* Player, Enemy, Level Boss, Hero, Super Player, etc.
  - These classes might share some attributes such as *health*, *position*, etc.
  - Maybe all other classes are some specializations of the Player class.
* Account, Savings Account, Checking Account, Trust Account, etc.
  - Classes for banking system
  - Withdraw, deposit functionalities are common in these classes.
* Shape, Line, Oval, Circle, Square, etc.
  - Classes for graphic design system
* Person, Employee, Student, Faculty, Staff, Administrator, etc.
  - Classes for the university personnel system

### Case Study: Accounts

* Account
  * **balance**, **deposit**, **withdraw**, ...
* Savings Account
  - **balance**, **deposit**, **withdraw**, interest rate, ...
* Checking Account
  - **balance**, **deposit**, **withdraw**, minimum balance, per check fee...
* Trust Account
  - **balance**, **deposit**, **withdraw**, interest rate...

Without inheritance (Code duplication)

```cpp
class Account {
    // balance, deposit, withdraw, . . .
};

class Savings_Account {
    // balance, deposit, withdraw, interest rate, . . .
};

class Checking_Account {
    // balance, deposit, withdraw, minimum balance, per check fee, . . .
};

class Trust_Account {
    // balance, deposit, withdraw, interest rate, . . .
};
```

With inheritance (Code reuse)

```cpp
class Account {
    // balance, deposit, withdraw, . . .
};

class Savings_Account : public Account {
    // interest rate, specialized withdraw, . . .
    // -------------  --------------------
    // simply "add"   "modify" the behavior of the parent class's attributes
    // to the         to meet its requirement
    // inherited
    // attributes of
    // Account class
};

class Checking_Account : public Account {
    // minimum balance, per check, fee, specialized withdraw, . . .
};

class Trust_Account : public Account {
    // interest rate, specialized withdraw, . . .
};
```



## Terminology

* Inheritance
  * Process of creating new classes from existing classes
  * Reuse mechanism
* Single inheritance
  * A new class is created from another 'single' class
* Multiple inheritance
  * A new class is created from two (or more) other classes

* Base class (parent class, super class)
  * The class being extended or inherited from



<img src="./img/base-derived.png" alt="base-derived" width="170">



* Derived class (child class, sub class)
  * The class being created from the base class
  * Will inherit attributes and operations from base class

* "Is-A" relationship
  * Public inheritance
  * Derived classes are sub-types of their base classes
  * Can use a derived class object wherever we use a base class object
  * e.g., A savigns account "Is-A" account
* Generalization
  * Combining similar classes into a single, more general class based on common attributes
  * The more general class is more abstract, and therefore can be potentially be reused more easily
* Specialization
  * Creating new classes from existing classes proving more specialized attributes or operations
* Inheritance or Class Hierarchies
  * Organization of our inheritance relationships so that our code can be more effectively reused
  * When we design our program, we use both generalization and specialization to make the code more organized and reusable
  * C++ does not have singly-rooted hierarchy. 
    - Java does! The `Object` class is the root class.

### Examples

* Class hierarchy example 1

  Classes:

    - A (root class)
    - B is derived from A
      - B is not a C
    - C is derived from A
    - D is derived from C
    - E is derived from D
      - E is also a C since inheritance is **transitive**
      - E is also an A



<img src="./img/class-hierarchy-1.png" alt="class-hierarchy-1" width="400">



* Class hierarchy example 2

  Classes:

    - Person (root class)
    - Employee is derived from Person
    - Student is derived from Person
      - Student is a person (Student class inherits the attributes and operations of the Person class.
      - Student is NOT an Employee
    - Faculty is derived from Employee
    - Staff is derived from Employee
      - Staff is an Employee, and in fact, is also a Person
    - Administrator is derived from Employee

    [!] Note: Notice that the relationships are NOT bi-directional! A Person is not necessarily an Employee because it could be a student.



<img src="./img/class-hierarchy-2.png" alt="class-hierarchy-2" width="500">





## Public Inheritance vs. Composition

* Both allow reuse of existing classes

### Public Inheritance

- "is-a" relationship between derived and base classes
  - Employee "is-a" Person
  - Checking Account "is-a" Account
  - Circle "is-a" Shape
- Derived classes automatically inherit all of the base classes' attributes and operations.
- Not all relationship can be described in terms of inheritance.

### Composition

- "has-a" relationship
  - Person "has a" Account
  - Player "has-a" Special Attack
  - Circle "has-a" Location

- Using a combination of inhertiance and composition, we can express complex relationships between classes and leverage code reuse.
    - When we model class data members, we're using composition.
      - Many times the instance variables are primitive types, so we don't include them in class diagrams, but the concept is the same.

### Example



<img src="./img/public-inheritance-vs-composition.png" alt="public-inheritance-vs-composition" width="750">



  - We will use the term **composition** to simply mean it has a relationship, and we won't be concerned about whether the Account object can logically exist without being associated with a Person object.
  - Does a Student object also have an account? $$\to$$ Yes! Because a Student is a Person.
  - What a bout a Faculty member? $$\to$$ Yes! Because a Faculty member is a Person.




## When to Choose Inheritance over Composition?

* If the "is-a" relationship dosn't make sense, then don't use public inheritance.
* A rule of thumb when using inheritance is to step back, look at your design and be sure that the inheritance is appropriate.
    - If you can model a relationship with composition, then you should consider doing that first since inheritance adds more complexity to your design.




## Composition Shown in a Class

* Person Class

  ```cpp
  class Person
  {
  private:
      std::string name;     // "has-a" name
      Account account;      // "has-a" account
  };  
  ```

  > Composition is a common design pattern for reuse and you'll see it used much more frequently than inheritance. But we can use both inheritance and composition together to create powerful frameworks that allow us to reuse existing code.




## Types of Inheritance in C++

* `public`
  - Most common
  - Establishes "**is-a**" relationship between Derived and Base classes
* `private` and `protected` (beyond our scope)
  - Establishes "derived class **has-a** base class" relationship
  - "Is implemented in terms of" relationship
  - Different from composition




## Deriving Classes from Existing Classes

* C++ Derivation Syntax

  ```cpp
  class Base
  {
      // Base class members . . .
  };
  
  class Derived: access-specifier Base
  {
      // Derived class members . . .
  };
  ```

  > Note: Access-specifier can be: `public`, `private`, or `protected` (if not provided, `private` inheritance by default)

  ```cpp
  class Account
  {
      // Account class members . . .
  };
  
  class Savings_Account: public Account
  {
      // Savings_Account class members . . .
  };
  ```

  > Savings_Account "is-a" Account.
  >
  > Now, a Savings_Account inherits everything in the account class, and it's free to implement its own specialized behaviors based on the behavior it inherited from account.

* C++ Creating Objects

  ```cpp
  Account account{};
  Account *p_account = new Account();
  
  account.deposit(1000.0);
  p_account->withdraw(200.0);
  
  delete p_account;
  ```

  ```cpp
  Savings_Account sav_account{};
  Savings_Account *p_sav_account = new Savings_Account();
  
  sav_account.deposit(1000.0);
  p_sav_account->withdraw(200.0);
  
  delete p_sav_account;
  ```



## `Protected` Members & Class Access

* The `protected` class member modifier

  ```cpp
  class Base
  {
  protected:
      // Protected Base class members . . .
  };
  ```

  > * Accessible from the `Base` class itself
  > * Accessible from classes derived from `Base`
  > * Not accessible directly from objects of either the `Base` or its derived class. 





<img src="./img/public-protected-private-inheritances.png" alt="public-protected-private-inheritances" width="550">





## Constructors & Destructors

### Constructors

* A derived class inherits from its base class.

* The base part of the derived class MUST be initialized BEFORE the derived class initialized.

* When a derived object is created

  1. Base class constructor executes

  2. Derived class constructor executes

### Destructors

* Class destructors are invoked in the reverse order as constructors
* The derived part of the derived class MUST be destroyed BEFORE the base class destructor is invoked
* When a derived object is destroyed
  1. Derived class destructor executes
  2. Base class destructor executes
  3. Each destructor should free resources allocated in it's own constructors

### Note

* A derived class does NOT inherit
  * Base class constructors
  * Base class destructor
  * Base class overloaded assignment operators
  * Base class friend functions
* However, the derived class constructors, destructors, and overloaded assignment operators can invoke the base-class versions.
* C++11 allows explicit inheritance of base 'non-special' constructors with
  * `using Base::Base;` anywhere in the derived class declaration
  * Lots of rules involved, often better to define constructors yourself
* Passing arguments to base class constructors
  * The base part of a derived class must be initialized first
  * How can we control exactly which base class constructor is used during initialization?
  * We can invoke whichever base class constructor we wish in the initialization list of the derived class.

### Example

* Constructors, destructors and class initialization

  ```cpp
  class Base
  {
  public:
      Base() { cout << "Base constructor" << endl; }
      ~Base() { cout << "Base destructor" << endl; }
  };
  
  class Derived : public Base
  {
  public:
      Derived() { cout << "Derived constructor" << endl; }
      ~Derived() { cout << "Derived destructor" << endl; }
  };
  ```

  ```cpp
  Base base;			// Base constructor
  					// Base destructor
  
  Derived derived;	// Base constructor
  					// Derived constructor
  					// Derived destructor
  					// Base destructor
  ```

* Passing arguments to base class constructors

  ```cpp
  class Base
  {
  public:
      Base();
      Base(int);
      . . . 
  };
  
  Derived::Derived(int x)
      : Base(x), {/* optional initializers for Derived */}
  { 
  	// code
  };
  ```

  ```cpp
  class Base
  {
      int value;
  public:
      Base() : value{0}
      { cout << "Base no-args constructor" << endl; }
      Base(int x) : value{x}
      { cout << "int Base constructor" << endl; }
  };
  
  class Derived : public Base
  {
      int doubled_value;
  public:
      Derived() : Base{}, doubled_value{0}
      { cout << "Derived no-args constructor" << endl; }
      Derived(int x) : Base{x}, doubled_value{x * 2}
      { cout << "int Derived constructor" << endl; }
  };
  ```

  ```cpp
  Base base;				// Base no-args constructor
  
  Base base{100};			// int Base constructor
  Derived derived;		// Base no-args constructor
  						// Derived no-args constructor
  
  Derived derived{100};	// int Base constructor
  						// int Derived constructor
  ```

  

## Copy/Move Constructors and Operator `=` with Derived Classes

* Copy/move constructors and overloaded operator `=`
  * Not inherited from the base class
  * You may not need to provide your own
    * Compiler-provided versions may be just fine
  * We can explicitly invoke the base class versions from the derived class
* Often you do not need to provide you own
* If you **DO NOT** define them in the derived class, then the compiler will create them automatically and call the base class' version.
* If you **DO** provide the derived versions, then YOU must invoke the base versions explicitly yourself.
* Be careful with raw pointers
  * Especially if base and derived each have raw pointers
  * Provide them with deep copy semantics

### Example

* Copy constructor

  Can invoke base copy constructor explicitly (Derived object `other` will be sliced)

  ```cpp
  Derived::Derived(const Derived &other)
      : Base(other), { /* Derived initialization list */ }
  {
      // code
  }
  ```

  ```cpp
  class Base
  {
      int value;
  public:
      // Same constructors as previous example
      
      Base(const Base &other) : value{other.value}
      { cout << "Base copy constructor" << endl; }
  };
  ```

  ```cpp
  class Derived : public Base
  {
      int doubled_value;
  public:
  	// Same constructors as previous example
  
      Derived(const Derived &other)
          : Base(other), doubled_value{other.doubled_value}
      { cout << "Derived copy constructor" << endl; }
  };
  ```

* Operator `=`

  ```cpp
  class Base
  {
      int value;
  public:
      // Same constructors as previous example
      
      Base &operator=(const Base &rhs)
      {
          if (this != &rhs)
              value = rhs.value;	// assign
          
          return *this;
      }
  };
  ```

  ```cpp
  class Derived : public Base
  {
      int doubled_value;
  public:
  	// Same constructors as previous example
  
      Derived &operator=(const Base &rhs)
      {
          if (this != &rhs)
          {
              Base::operator=(rhs);				// Assign Base part
              doubled_value = rhs.doubled_value;	// Assign Derived part
          }
          
          return *this;
      }    
  };
  ```



## Using and Redefining Base Class Methods

* Derived class can directly invoke base class methods
* Derived class can **override** or **redefine** base class methods
* Very powerful in the context of polymorphism

### Static Binding of Method Calls

* Binding of which method to use is done at compiler time
  * Default binding for C++ is **static**.
  * Derived class objects will use `Derived::deposit`.
  * But, we can explicitly invoke `Base::deposit` from `Derived::deposit`.
  * OK, but limited - much more powerful approach is **dynamic binding**.

### Example

* Using and redefining base class methods

  ```cpp
  class Account
  {
  public:
      void deposit(double amount) { balance += amount; }
  };
   
  class Savings_Account : public Account
  {
  public:
      // Redefine base class method
      void deposit(double amount) 
      {
          amount += some_interest;
          Account::deposit(amount);	// Invoke base class method
      }
  }
  ```

* Static binding of method calls

  ```cpp
  Base b;
  b.deposit(1000.0);			// Base::deposit
  
  Derived d;
  d.deposit(1000.0);			// Derived::deposit
  
  Base *ptr = new Derived();
  ptr->deposit(1000.0);		// Base::deposit ???
  ```

  > Due to the type of `ptr`, `ptr->deposit()` is statically bound to `Base::deposit` at compile time.



## Multiple Inheritance

* A derived class inherits from two or more base classes at the same time.
* The base classes may belong to unrelated class hierarchies.
* Note:
  * Beyond the scope of this course
  * Some compelling use-cases
  * Easily misused
  * Can be very complex

### Example



<img src="./img/multiple-inheritance.png" alt="multiple-inheritance" width="550">



* C++ syntax

  ```cpp
  class Department_Chair : public Faculty, public Administrator
  {
      . . .
  };
  ```