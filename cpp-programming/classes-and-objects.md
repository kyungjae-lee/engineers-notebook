<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Classes & Objects

# Classes & Objects



## Classes & Objects

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

