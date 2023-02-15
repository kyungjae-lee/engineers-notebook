<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Integrated Deadlock Strategy

# Integrated Deadlock Strategy



## An Integrated Deadlock Strategy

* There are strengths and weaknesses to all of the strategies for dealing with deadlock. Rather than attempting to design an OS facility that employs only one of these strategies, it might be more efficient to use different strategies in different situations.

* One suggested approach:

  * Group resources into a number of different resource classes.

  * Use the linear ordering strategy defined previously for the prevention of circular wait to prevent deadlocks between resource classes.

  * Within a resource class, use the algorithm that is most appropriate for that class.

    e.g., Consider the following classes of resources and the strategies could be used:

    * **Swappable space** (Blocks of memory on secondary storage for use in swapping processes)

      Prevention of deadlocks by requiring that all of the required resources that may be used be allocated at one time, as in the hold-and-wait prevention strategy. This strategy is reasonable if the maximum storage requirements are known, which is often reasonable if the maximum storage requirements are known, which is often the case. 

      Deadlock avoidance is also a possibility.

    * **Process resources** (Assignable devices, such as tape drives, and files)

      Avoidance will often be effective in this category, because it is reasonable to expect processes to declare ahead of time the resources that they will require in this class.

      Prevention by means of resource ordering within this class is also possible.

    * **Main memory** (Assignable to processes in pages or segments)

      Prevention by preemption appears to be the most appropriate strategy for main memory. When a process is preempted, it is simply swapped to secondary memory, freeing space to resolve the deadlock.

    * **Internal resources** (Such as I/O channels)

      Prevention by means of resource ordering can be used.






## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.
