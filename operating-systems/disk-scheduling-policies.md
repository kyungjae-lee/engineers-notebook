<a href="../">Notebook</a> > <a href="./">Operating Systems</a> > Disk Scheduling Policies

# Disk Scheduling Policies



## Disk Scheduling Policies

* **FIFO (First In First Out)**
  * Simple
  * Fair
  * Suffers from random seek times
* **Priority**
  * Control of scheduling is now outside of the disk manager
  * Does not (not intended to) optimize disk utilization
  * Poor for general purpose
* **LIFO (Last In First Out)**
  * Simple
  * Based on the idea that new requests happen near recent ones (locality)
  * However, early processes tend to move out of range and starve.
* **SSTF (Shortest Seek Time First)**
  * Select the disk I/O request that requires the least arm movement distance.
  * Drawback - Seeks will tend to cluster around an area, starving the outliers that are farther away.
* **SCAN**
  * Alleviates the problems with random seek times (FIFO) and starvation (LIFO, SSTF).
  * Requires the arm to move in one direction at a time.
  * Satisfy all requests along the path of the arm. When it reaches the edge, reverse course and process all requests along the way back.
  * LOOK stops moving in a direction when there are no more to process there.
* **C-SCAN (Circular SCAN)**
  * Improves the fairness of SCAN.
  * Disk arm always works on requests in one direction only.
  * When it reaches the end/edge, the arm picks up, moves back to the beginning and starts over there for another pass in.
  * Linux uses a variation of SCAN called "Elevator".



## Examples







## References

Stallings, W. (2018). *Operating Systems: Internals and Design Principles* (9th ed.). Pearson Education, Inc.

