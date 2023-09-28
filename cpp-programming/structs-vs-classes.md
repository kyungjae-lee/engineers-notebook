<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Structs vs. Classes

# Structs vs. Classes



## Introduction

* In addition to define a `class` we can declare a `struct`.
* `struct` comes from the C programming language.
* Essentially the same as a class except that members are `public` by default.



## General Guidelines

* `struct`
  * Use `struct` for passive objects with public access
  * Don't declare methods in `struct`
* `class`
  * Use class for active objects with private access
  * Implement getters/setters as needed
  * Implement member methods as needed



## Examples

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
