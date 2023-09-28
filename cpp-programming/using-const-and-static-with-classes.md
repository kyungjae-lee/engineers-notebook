<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Using `const` & `static` with  Classes

# Using `const` & `static` with  Classes



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
