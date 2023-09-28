<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Friends of a Class

# Friends of a Class



## Friends of a Class

* Friend
  * A function or class that has access to private class member
  * And, that function of or class is NOT a member of the class it is accessing
* Function
  * Can be regular non-member functions
  * Can be member methods of another class
* Class
  * Another class can have access to private class members

* Friendship must be granted NOT taken
  * Declared explicitly in the class that is granting friendship
  * Declared in the function prototype with the keyword `friend`

* Friendship is not symmetric
  * Must be explicitly granted
  * A being a friend of B does not necessarily make B a friend of A
* Friendship is not transitive
  * Must be explicitly granted
  * A being a friend of B, and B being a friend of C, does not necessarily make A a friend of C



## Examples

* Non-member function as a friend

  ```cpp
  class Player
  {
      friend void display_player(Player &p);
      std::string name;
      int health;
      int xp;
  public:
      . . .
  };
  
  void display_player(Plyaer &p)
  {
      std::cout << p.name << std::endl;
      std::cout << p.health << std::endl;
      std::cout << p.xp << std::endl;
  }
  ```

  > Since `display_player()` is a friend function of the class `Player`, it can modify the `Player`'s private data members.

* Member function of another class as a friend

  ```cpp
  class Player
  {
      friend void Other_class::display_player(Player &p);
      std::string name;
      int health;
      int xp;
  public:
      . . .
  }
  
  class Other_class
  {
      ...
  public:
      void display_player(Player &p)
      {
          std::cout << p.name << std::endl;
          std::cout << p.health << std::endl;
          std::cout << p.xp << std::endl;
      }
  };
  ```

* Another class as a friend

  ```cpp
  class Player
  {
      friend class Other_class;
      std::string name;
      int health;
      int xp;
  public:
      . . .
  };
  ```
