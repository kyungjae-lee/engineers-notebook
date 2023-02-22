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