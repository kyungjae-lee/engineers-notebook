<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Constructors & Destructors

# Constructors & Destructors



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

