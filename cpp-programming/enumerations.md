<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Enumerations

# Enumerations



## Overview

* What is an enumeration?
* Motivation
* Structure of an enumeration
* Types of enumerations
  * Unscoped enumeration
  * Scoped enumeration

* Enumerations in use



## What is an Enumeration?

* A user-defined type that models a set of constant integral values.
  * The days of the week (Mon, Tue, Wed, ...)
  * The months of the year (Jan, Feb, Mar, ...)
  * The suits in a deck of cards (Clubs, Hearts, Spades, Diamonds)
  * The values in a deck of cards (Ace, Two, Three, ...)
  * States of a system (Idle, Defense_Mode, Attack_Mode, ...)
  * The directions on a compass (North, South, East, West)



## Motivation

* Prior to enumerated types

  * Unnamed numerical constants
  * "Magic numbers"
  * These constants would be used as conditionals in control statements.
  * Often, one would have no idea what an algorithm was doing.
  * As a result, many algorithms suffered from low readability and high numbers of logic errors.

* Readability of the following code

  ```cpp
  int state;
  std::cin >> state;
  
  if (state == 0)
      initiate(3);
  else if (state == 1)
      initiate(4);
  else if (state == 2)
      initiate(5);
  ```

   Can be improved by using enumeration

  ```cpp
  enum State {Engin_Failure = 0, Inclement_Weather = 1, Nominal = 2};
  enum Sequence {Abort = 3, Hold = 4, Launch = 5};
  
  int user_input;
  std::cin >> user_input;
  State state = State(user_input);
  
  if (state == Engin_Failure)				// state = 0
      initiate(Abort);					// sequence = 3
  else if (state == Inclement_Weather)	// state = 1
      initiate(Hold);						// sequence = 4
  else if (state == Nominal)				// state = 2
      initiate(Launch);					// sequence 5
  ```

* Algorithm correctness

  ```cpp
  int get_state()
  {
      return state_of_fridge;
  }
  
  int getState()
  {
      return state_of_rocket;
  }
  ```

  ```cpp
  int state = get_state();
      
   if (state == 0)
      initiate(3);
  else if (state == 1)
      initiate(4);
  else if (state == 2)
      initiate(5);   
  ```

  > Will compile!

  ```cpp
  State state = get_state();
      
  if (state == Engin_Failure)
      initiate(Abort);
  else if (state == Inclement_Weather)
      initiate(Hold);
  else if (state == Nominal)
      initiate(Launch);
  ```

  > Will not compile!



## Structure of an Enumeration

* Syntax

  ```plain
  enum-key enum-name : enumerator-type { };
  ```

  > * `enum-key` - Defines the scope of the enumeration
  > * `enum-name` - Optional name of the enumeration
  > * `enumerator-type` - Enumerator type; can be omitted and the compiler will try to deduce it
  > * `{ }` - Enumerator list; list of enumerator definitions

* Enumerator initialization

  ```cpp
  enum {Red, Green, Blue};	// Implicit initialization
  //	   0     1     2
  ```

  ```cpp
  enum {Red = 1, Green = 2, Blue = 3};	// Explicit initialization
  ```

  ```cpp
  enum {Red = 1, Green, Blue};	// Explicit/Implicit initialization
  //               2     3
  ```

* Enumerator type

  | Integral Type      | Width in Bits |
  | ------------------ | ------------- |
  | int                | at least 16   |
  | unsigned int       | at least 16   |
  | long               | at least 32   |
  | unsigned long      | at least 32   |
  | long long          | at least 64   |
  | unsigned long long | at least 64   |

  ```cpp
  enum {Red, Green, Blue};				// Underlying type: int
  //	   0     1     2
  //    000   001   010
  
  enum {Red, Green, Blue = -32769};		// Underlying type: long
  //     0     1    -32769
  //    000   001   11111111111111110111111111111111
  
  enum : uint8_t {Red, Green, Blue};		// Underlying type: unsigned 8 bit int
  enum : long long {Red, Green, Blue};	// Underlying type: long long
  ```

* Enumeration name

  ```cpp
  enum {Red, Green, Blue};	// Anonymous; No type safety
  
  int my_color;
  
  my_color = Green;	// Valid
  my_color = 4;		// Also valid
  ```

  ```cpp
  enum Color {Red, Green, Blue};	// Named; Type safe
  
  Color my_color;
  
  my_color = Green;	// Valid
  my_color = 4;		// Also valid
  ```

  

## Unscoped Enumerations

* Syntax

  ```plain
  enum enum-name : enumerator-type { };
  ```

  > * `enum` - Enum; Defines an unscoped enumeration

* Using `if` and `switch` statements with unscoped enumerations

  ```cpp
  State state = get_state();
      
  if (state == Nominal)					// Accessed first
      initiate(Launch);
  else if (state == Inclement_Weather)	// Accessed second
      initiate(Hold);
  else if (state == Engine_Failure)		// Accessed last
      initiate(Abort);
  ```

  ```cpp
  State state = get_state();
      
  switch (state)
  {
      case Engine_Failure:	// Equal access time
          initiate(Abort);
          break;
      case Inclement_Weather:	// Equal access time
          initiate(Hold);
          break;
      case Nominal:			// Equal access time
          initiate(Launch);
          break;
  }
  ```

* Using `cin` and `cout` with unscoped enumerations

  ```cpp
  enum State {Engine_Failure, Inclement_Weather, Nominal};
  
  State state;
  std::cin >> state;	// Not allowed using standard extraction operator
  ```

  ```cpp
  enum State {Engine_Failure, Inclement_Weather, Nominal};	// Underlying type: int
                   0                   1           2
  std::underlying_type_t<State> user_input;					// Type: int
  std::cin >> user_input;										// User input = 3
  State state = State(user_input);							// state = 3
  ```

  ```cpp
  enum State {Engine_Failure, Inclement_Weather, Nominal};	// Underlying type: int
                   0                   1           2
  std::underlying_type_t<State> user_input;					// Type: int
  std::cin >> user_input;										// User input = 3
  State state;
  switch (user_input)
  {
      case Engine_Failure:		state = State(user_input); break;
      case Inclement_Weather:		state = State(user_input); break;
      case Nominal:				state = State(user_input); break;
      default:					std::cout << "User input is not a valid state.";
  }
  ```

  ```cpp
  State state;
  std::cin >> state;		// Valid with overloaded extraction operator
  ```

  ```cpp
  enum State {Engine_Failure, Inclement_Weather, Nominal};
  State state = Engine_Failure;
  std::cout << state;		// Displays 0
  ```

  ```cpp
  enum State {Engine_Failure, Inclement_Weather, Nominal};
  State state = Engine_Failure;
  switch (state)
  {
      case Engine_Failure:		std::cout << "Engine Failure";
      case Inclement_Weather:		std::cout << "Inclement Weather";
      case Nominal:				std::cout << "Nominal";
      default:					std::cout << "Unknown";
  }	// Displays "Engine Failure"
  ```

  ```cpp
  enum State {Engine_Failure, Inclement_Weather, Nominal};
  
  std::stream& operator<<(std::ostream& os, State& state)
  {
      switch (state)
      {
          case Engine_Failure:		os << "Engine Failure";
          case Inclement_Weather:		os << "Inclement Weather";
          case Nominal:				os << "Nominal";
          default:					os << "Unknown";
      }
      
      return os;
  }
  ```

  ```cpp
  State state = Engine_Failure;
  std::cout << state;		// Displays "Engine Failure"
  ```

  

## Scoped Enumerations

* Syntax

  ```plain
  enum class enum-name : enumerator-type { };
  ```

  > * `enum class` (or `enum struct`) - Defines a scoped enumeration

* Motivation

  ```cpp
  enum Whale {Blue, Beluga, Gray};
  enum Shark {Greatwhite, Gammerhead, Bull, Blue};	// Error: Blue already defined
  
  if (Beluga == Hammerhead)
      std::cout << "A beluga whale is equivalent to a hammerhead shark.";
  ```

* Using `if` and `switch` statements with unscoped enumerations

  ```cpp
  enum class Whale {Blue, Beluga, Gray};
  Whale whale = Whale::Beluga;
  
  if (whale == Whale::Blue)
      std::cout << "Blue whale";
  else if (whale == Whale::Beluga)
      std::cout << "Beluga whale";
  else if (whale == Whale::Gray)
      std::cout << "Gray whale";
  ```

  ```cpp
  enum class Whale {Blue, Beluga, Gray};
  Whale whale = Whale::Beluga;
  
  switch (whale)
  {
      case Whale::Blue:
      	std::cout << "Blue whale";
          break;
      case Whale::Beluga:
      	std::cout << "Beluga whale";
          break;
      case Whale::Gray:
      	std::cout << "Gray whale";
          break;
  }
  ```

* Using scoped enumerator values

  ```cpp
  enum class Item {Milk = 350, Bread = 250, Apple = 132};		// Underlying type: int
  
  int milk_code = Item::Milk;
  int total = Item::Milk + Item::Bread;
  std::cout << Item::Milk;
  ```

  ```cpp
  enum class Item {Milk = 350, Bread = 250, Apple = 132};		// Underlying type: int
  
  int milk_code = int(Item::Milk);					// milk_code = 350
  // Or
  int milk_code = static_cast<int>(Item::Milk);
  
  int total = int(Item::Milk) + int(Item::Bread);		// total = 600
  std::cout << underlying_type_t<Item>(Item::Milk);	// Displays 350
  ```
