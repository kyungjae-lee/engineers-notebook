<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Deadlock Detection

# Deadlock Detection



* Deadlock prevention strategies are very conservative; they solve the problem of deadlock by limiting access to resources and by imposing restrictions on processes.

* At the opposite extreme, deadlock detection strategies do not limit resource access or restrict process actions. With deadlock detection, requested resources are granted to processes whenever possible. Periodically, the OS performs an algorithm that allows it to detect the circular wait.



## Deadlock Detection Algorithm

* A check for deadlock can be made as frequently as each resource request, or less frequently, depending on how likely it is for a deadlock to occur.

* Consider a system of $n$ processes and $m$ different types of resources. Let us define the following vectors and matrices:

  * **Resource** vector - total amount of each resource in the system

    $R = (R_1, R_2, ... , R_m)$

  * **Available** vector - total amount of each resource not allocated to any process

    $V =(V_1, V_2, ... , V_m)$

  * **Request** matrix - $Q_{ij}$ = amount of resources of type $j$ requested by process $i$
    $$
    Q =
    \begin{pmatrix}
    Q_{11} & Q_{12} & ... & Q_{1m} \\
    Q_{21} & Q_{22} & ... & Q_{2m} \\
    ... & ... & ... & ... \\
    Q_{n1} & Q_{n2} & ... & Q_{nm} \\
    \end{pmatrix}
    $$

  * **Allocation** matrix- $A_{ij}$ = current allocation to process $i$ of resource $j$
    $$
    A =
    \begin{pmatrix}
    A_{11} & A_{12} & ... & A_{1m} \\
    A_{21} & A_{22} & ... & A_{2m} \\
    ... & ... & ... & ... \\
    A_{n1} & A_{n2} & ... & A_{nm} \\
    \end{pmatrix}
    $$

  The following relationships hold:

  1. $R_j = V_j + \sum_{i=1}^{n}A_{ij}$, for all $j$

     All resources are either available for allocated.

  2. $C_{ij} \le R_j$, for all $i$, $j$

     No process can claim more than the total amount of resources in the system.

  3. $A_{ij} \le C_j$, for all $i$, $j$

     No process is allocated more resources of any type than the process originally claimed to need.

  



## Advantages and Disadvantages of Deadlock Detection Algorithm

### Advantages

* It leads to early detection.
* The algorithm is relatively simple because it is based on incremental changes to the state of the system.

### Disadvantages

* Frequent checks consume considerable processor time.






## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.
