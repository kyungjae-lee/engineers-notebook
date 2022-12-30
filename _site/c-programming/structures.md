<a href="../">Notebook</a> > <a href="./">C Programming</a> > Structures

# Structures



## Introduction to Structures

* Structures in C is a data structure used to create user-defined data types.

* Structures allow us to combine data of different types

* Defining a structure:

  ```c
  struct tag_name
  {
      member_element1;
      member_element2;
      member_element3;
      member_element4;
  };	/* don't forget this semi-colon! */
  ```

  > `struct` is a reserved keyword in C.

  Structure definition example:

  ```c
  struct CarModel
  {
      unsigned int	carNumber;
      uint32_t		carPrice;
      uint16_t		carMaxSpeed;
      float			carWeight;
  }
  ```

​		Defining a structure does not incur memory allocation. Its just a description or a record.

* Creating structure variables:

  ```c
  struct CarModel CarBMW, CarFord, CarHonda; /* memory allocation takes place at this point */
  ```

  > `struct CarModel`: User-defined data type
  >
  > `CarBMW`, `CarFord`, `CarHonda`: Structure variables

* Initializing the structure member variables:

  ```c
  /* C89 method (order is important!) */
  struct CarModel CarBMW = {2021, 15000, 220, 1330}; 
  
  /* C99 method using designated initializers (order is NOT important!)) */
  struct CarModel CarBMW = {.carNumber = 2021, 
                            .carWeight = 1330,
                            .carMaxSpeed = 220,
                            .carPrice = 15000};
  ```

* Accessing the structure member variables:

  ```c
  /* use .(dot operator) to access the member variables */
  printf("%d\n", CarBMW.carPrice);	/* 15000 */
  ```



## Exercise

Write a program to create a `carModel` structure and create 2 variables of type `struct carModel`. Initialize the variables with the below given data and then print them.

1. 2021, 15000, 220, 1330
2. 4031, 35000, 160, 1900.96

```c
#include <stdio.h>
#include <stdint.h>

struct carModel
{
    uint32_t carNumber;
    uint32_t carPrice;
    uint16_t carMaxSpeed;
    float carWeight;
}; 	/* don't forget this semi-colon! */

int main(int argc, char *argv[])
{
    struct carModel carBMW = {2021, 15000, 220, 1330};
    struct carModel carFord = {.carNumber = 4031, .carWeight = 1900.96, 
                               .carMaxSpeed = 160, .carPrice = 35000};
    
    printf("Car BMW information:\n");
    printf("carNumber   = %u\n", carBMW.carNumber);
    printf("carPrice    = %u\n", carBMW.carPrice);
    printf("carMaxSpeed = %u\n", carBMW.carMaxSpeed);
    printf("carWeight   = %f\n", carBMW.carWeight);

    puts("");

    printf("Car Ford information:\n");
    printf("carNumber   = %u\n", carFord.carNumber);
    printf("carPrice    = %u\n", carFord.carPrice);
    printf("carMaxSpeed = %u\n", carFord.carMaxSpeed);
    printf("carWeight   = %f\n", carFord.carWeight);

    return 0;
}
```

```plain
Car BMW information:
carNumber   = 2021
carPrice    = 15000
carMaxSpeed = 220
carWeight   = 1330.000000

Car Ford information:
carNumber   = 4031
carPrice    = 35000
carMaxSpeed = 160
carWeight   = 1900.959961
```



## Size of a Structure

* Do not prematurely assume that the size of the structure is going to be all the member variables' sizes put together. See the following example:

  ```c
  #include <stdio.h>
  #include <stdint.h>
  
  struct carModel
  {
      uint32_t carNumber;		/* 4 byte */
      uint32_t carPrice;		/* 4 byte */
      uint16_t carMaxSpeed;	/* 2 byte */
      float carWeight;		/* 4 byte */
  }; 	/* don't forget this semi-colon! */
  
  int main(int argc, char *argv[])
  {
      struct carModel carBMW = {2021, 15000, 220, 1330};
      
      printf("sizeof(struct carModel) = sizeof(carBMW) = %lu\n", sizeof(struct carModel));
      /* is it going to be 4+4+2+4=14 bytes? */
      
      return 0;
  }
  ```

  ```plain
  izeof(struct carModel) = sizeof(carBMW) = 16
  ```

  > `%lu` in line 16 specifies "long unsigned int".
  >
  > Note that the return type of `sizeof()` is `size_t`, which is platform dependent.

* This can be explained by **aligned/unaligned data access**.



## Aligned/Unaligned Data Access

* For efficiency, the compiler generates instructions to store variables on their **natural size boundary** addresses in the memory. This is also true for structures. Fields in a structure are located on their natural size boundary.

* Natural size boundary:

  ```plain
  			char 	(1-byte aligned)
  Address:	0403010	0403011	0403012	0403013	0403014	0403015 ...
  				  -   	  -		  -		  -		  -		  -
  			short 	(2-byte aligned)
  Address:	0403010	0403012	0403014	0403016	0403016	0403018 ...
  				  -   	  -		  -		  -		  -		  -
  			int 	(4-byte aligned)
  Address:	0403010	0403014	0403018	040301C	0403020	0403024 ...
  				  -   	  -		  -		  -		  -		  -
  ```

* Why does the compiler do this?

  When the data is stored in aligned fashion it becomes a lot easier for the processor to do the read/write transactions on the memory. (Processor performance increases)

  Unaligned data storage will generate more instructions during the compilation process to deal with that unalignment which will actually increase the code size.

* The only negative side effect of aligned data storage is that you will lose some memory space due to padding. If you can't afford to lose any memory space, you could go with unaligned data storage. Using the gcc attribute **packed**, the compiler will generate packed structures which results in unaligned data storage.

  



## References

Nayak, K. (2022). *Microcontroller Embedded C Programming: Absolute Beginners* [Video file]. Retrieved from  https://www.udemy.com/course/microcontroller-embedded-c-programming/
