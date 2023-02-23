<a href="../">Notebook</a> > <a href="./">Computer/Software Security</a> > Hashing

# Hashing



## Hash Functions

* Good hash function has the following properties:

  * Can be applied to a block of data of any size

  * Produces a fixed-length output

  * $H(x)$ is relatively easy to compute for any given $x$.

  * One-way or pre-image resistant (impossible to restore original value from the hash value)

    Computationally infeasible to find $x$ such that $H(x) = h$.

  * No two different block of data result in the same hash value. 

    Computationally infeasible to find $y \neq x$ such that $H(y) = H(x)$

  * Collision resistant or strong collision resistance

    Computationally infeasible to find any pair $(x, y)$ such that $H(y) = H(x)$
  
* A hash function creates a fixed length compressed value from an arbitrary length digital message using a mathematical function.

* The hash value is much smaller than the original digital message, so a hash is often called a "digest".

* Message Digest algorithms are used to create Digital Signatures and Message Authentication Codes.

* Well known hash functions include:

  * Message Digest (MD)
  * Secure Hash Algorithm (SHA)

### Message Digest (MD)

* MD5 has been widely used - 128 bit (16 byte) hash value (others include: MD6, MD4, MD2)
* In 2004 successful attacks on MD5 were reported, so MD5 is no longer recommended for use.

### Secure Hash Algorithm (SHA)

* SHA1 has been widely used - 160 bit (20 byte) hash value
  * Successful attacks reported in 2005
  * SHA1 will be replaced in major browsers by 2017
  * SHA2 family SHA256, SHA512, etc. based on number of bits in the hash value
  * SHA3 released by National Institute of Standards in 2015



## Review Questions

* Which hash function is preferred, MD5 or SHA256, and Why?

  The SHA-256 algorithm returns hash value of 256-bits, or 64 hexadecimal digits. While not quite perfect, **current research indicates it is considerably more secure than either MD5 or SHA-1**. Performance-wise, a SHA-256 hash is about 20-30% slower to calculate than either MD5 or SHA-1 hashes.

* What is the (primary) problem nowadays with the MD5 hash function? WHY is this a problem?

  1. Because it is a broken algorithm. (Since 2004)
  2. Because it has collision issues. In other words, different plaintext can produce the same hash value which gives way to the hacking issues. (MD5 is susceptible to collision attacks and so far no collisions have been found for SHA256)

  

