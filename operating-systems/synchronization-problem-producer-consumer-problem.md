<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Synchronization Problem - Producer/Consumer Problem

# Synchronization Problem - Producer/Consumer Problem



## Introduction

* In dealing with the design synchronization and concurrency mechanisms, it is useful to be able to relate the problem at hand to known problems, and to be able to test any solution in terms of its ability to solve these  known problems.
* One of the several problems that appear frequently is the **producer/consumer problem**. (Another is, readers/writers problem)



## Producer/Consumer Problem

* **Definition**

  One or more producers generating some type of data (records, characters) and placing them into the buffer. A consumer is removing items from the buffer, one at a time. Only one producer or consumer may access the buffer at at time.

  Need to ensure that

  * A producer does not try to add to a full buffer
  * A consumer does not try to take from an empty buffer.




## References

Allen, B. (2023, January 10). Introduction to Operating Systems [Lecture]. University of Alabama in Huntsville, Huntsville, AL, United States.
