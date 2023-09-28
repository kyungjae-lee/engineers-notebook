<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > `this` Pointer

# `this` Pointer



## Introduction

* `this` is a reserved keyword
* Contains the address of the object, so it's a pointer to the object
* Can only be used in class scope
* All member access is done via the `this` pointer
* Can be used by the programmer
  * To access data member and methods
  * To determine if two objects are the same
  * Can be dereferenced (`*this`) to yield the current object
  * When overloading the assignment operator



## Examples

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

