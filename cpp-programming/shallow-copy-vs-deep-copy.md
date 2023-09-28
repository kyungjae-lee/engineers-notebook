<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">C++ Programming</a> > Shallow Copy vs. Deep Copy

# Shallow Copy vs. Deep Copy



## Introduction

* Consider a class that contains a pointer as a data member. Constructor allocates storage dynamically and initializes the pointer. Destructor releases the memory allocated by the constructor. What happens in the default copy constructor?



## Default Copy Constructor (Shallow Copy)

* Member-wise copy
* Each data member is copied from the source object.
* The pointer is copied, NOT what it points to (shallow copy)
* **Problem**
  * The source and the newly created object BOTH point to the SAME data area!
  * When we release the storage in the destructor, the other object still refers to the released storage!



## User-Defined Copy Constructor (Deep Copy)

* Create a copy of the pointed-to-data, NOT just the pointer itself.
* Each copy will have a pointer to UNIQUE storage in the heap.
* Deep copy when you have a raw pointer as a class data member.



## Examples

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

