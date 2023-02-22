<a href="../">Notebook</a> > <a href="./">Computer/Software Security</a> > Password Handling

# Password Handling



## Authentication

* NIST SP 800-63-3 (Digital Authentication Guideline, October 2016) defines digital user **authentication** as:

  "The process of establishing confidence in user identities that are presented electronically to an information system."

* Authentication is essentially proving they are who they say they are.



## Password

* 1st level of defense
* In the early days, passwords used to be stored in the OS as a plaintext.
* Better way is to store the hash of the passwords.
  * This still has some problems. What is there are multiple users with different passwords that result in the same hash value? If one hash value is hacked, all the other users with the same hash value will be open to hacking.
  * As one way to improve it
    * PW + Salt value $\to$ Hash value
    * This way, two different users with the same PW will still end up with the different hash values. (Salt values are different per user.)



## Password Cracking

* Dictionary attack
  * Develop a large dictionary of possible passwords and try each against the password file.
  * Each password must be hashed using each salt value and then compared to stored hash values.
* Rainbow table attack
  * Pre-compute tables of hash values for all salt.
  * A mammoth table of hash values.
  * Can be countered by using a sufficiently large salt value and a sufficiently large hash length.
* Smart password crackers
  * Shorter password lengths are also easier to crack.
  * Try some common passwords that people tend to use; e.g., 0000, 12345, etc. It's a human thing!
* Join the ripper
  * Open-source password cracker first developed in 1996.
  * Uses a combination of brute-force and dictionary techniques.