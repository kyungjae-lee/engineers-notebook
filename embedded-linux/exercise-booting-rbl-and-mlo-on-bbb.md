<a href="../">Notebook</a> > <a href="./">Embedded Linux</a> > Exercise - Booting RBL & MLO on BBB

# Exercise - Booting RBL & MLO on BBB



## Micro-SD Card Setup

* Set up your micro-SD card's partitions as follows:



<img src="./img/micro-sd-card-partitions.png" alt="micro-sd-card-partitions" width="700">



* I've used the partitioning application called "GParted".

  [!] Note: Select the correct device name that corresponds to your SD card. Selecting a wrong device may destroy your system.



<img src="./img/gparted-select-sd-card.png" alt="gparted-select-sd-card" width="700">



* Delete the previously allocated file system and create two partitions as follows.



<img src="./img/gparted-create-new-partition.png" alt="gparted-create-new-partition" width="700">

<img src="./img/gparted-create-new-partition-2.png" alt="gparted-create-new-partition-2" width="700">

<img src="./img/gparted-partitions-configured.png" alt="gparted-partitions-configured" width="700">





## References

Nayak, K. (2022). *Embedded Linux Step by Step Using Beaglebone Black* [Video file]. Retrieved from https://www.udemy.com/course/embedded-linux-step-by-step-using-beaglebone/