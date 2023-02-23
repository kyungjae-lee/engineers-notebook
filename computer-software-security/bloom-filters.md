<a href="../">Notebook</a> > <a href="./">Computer/Software Security</a> > Bloom Filters

# Bloom Filters



## Bloom Filters

* A Bloom filter is a data structure designed to tell you, rapidly and memory-efficiently, whether an element is present in a set.
* The price paid for this efficiency is that a Bloom filter is a **probabilistic data structure**: it tells us that the element either *definitely is not* in the set or *may be* in the set.
* Typically used to add elements to a set and test if an element is in a set.
* Bloom filters are useful in cases where:
  * the data to be searched is large
  * the memory available on the system is limited/low
* Bloom filter is memory efficient than a Hash Map with the same performance. The only thing to note is that this is a probabilistic data structure so for a small number of cases, it may give wrong results (which can be limited).
* The applications of Bloom Filter are:
  * Weak password detection
    * The idea is that the system can maintain a list of weak password in a form of Bloom Filter. This can be updated whenever a new user enters a password or existing user updates the password.
    * Whenever a new user comes, the new password is checked in the Bloom Filter and if a potential match is detected then the user is warned.
  * Internet Cache Protocol
  * Safe browsing in Google Chrome
  * Wallet synchronization in Bitcoin
  * Hash based IP Traceback
  * Cyber security like virus scanning