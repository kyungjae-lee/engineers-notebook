<a href="../">Notebook</a> > <a href="./">Embedded Linux</a> > Root File System (RFS) & Directory Structure

# Root File System (RFS) & Directory Structure



## Root File System (RFS)

* The **root file system (RFS)** is the file system Linux mounts to the `/` (root).

* A file system is a collection of files organized in a standardized directory structure.

* The standard for the Linux file system is called "**File system Hierarchy Standard (FHS)**". See the following links for more details:

  [https://wiki.Linuxfoundation.org/lsb/fhs-30](https://wiki.linuxfoundation.org/lsb/fhs-30)

  [https://en.wikipedia.org/wiki/File system_Hierarchy_Standard#cite_note-2](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard#cite_note-2)



## Directory Structure

* In a typical file system you will find the below folder structure, even though all these folders are not required for Linux to boot and mount the file system successfully.

  See also, [http://refspecs.Linuxfoundation.org/FHS_3.0/fhs/ch03s02.html](http://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s02.html).

### Directory Structure Overview

* `bin/` - Essential command binaries
* `boot/` - Static files of the boot loader
* `dev/` - Device files
* `etc/` - Host-specific system configuration
* `lib/` - Essential shared libraries and kernel modules
* `media/` - Mount point for removable media
* `mnt/` - Mount point for mounting a file system temporarily
* `opt/` - Add-on application software packages
* `run/` - Data relevant to running processes
* `sbin/` - Essential system binaries
* `srv/` - Data for services provided by this system
* `tmp/` - Temporary files
* `usr/` - Secondary hierarchy
* `var/` - Variable data

### Directory Structure in Detail

* `bin/`

  his directory contains **binaries** of Linux commands which are used by both the system admins and users.   

  You don’t need the root privilege from your system admin to execute these  commands. Remember that this folder will  not contain binaries for all the Linux commands. There is a restriction on what types of commands have to be placed in this directory, because  these binaries can be executed by the common user.   

  Below are some the commands you will find it in the `bin/` directory.

  * `cat` - Utility to concatenate files to standard output
  * `chgrp` - Utility to change file group ownership
  * `chmod` - Utility to change file access permissions   

  * `chown` - Utility to change file owner and group   

  * `cp` - Utility to copy files and directories   

  * `date` - Utility to print or set the system data and time   

  * `dd` - Utility to convert and copy a file   

  * `df` - Utility to report file system disk space usage   

  * `dmesg` - Utility to print or control the kernel message buffer   

  * `echo` - Utility to display a line of text   

  * `false` - Utility to do nothing, unsuccessfully   

  * `hostname` - Utility to show or set the system's host name   

  * `kill` - Utility to send signals to processes   

  * `ln` - Utility to make links between files   

  * `login` - Utility to begin a session on the system   

  * `ls` - Utility to list directory contents   

  * `mkdir` - Utility to make directories   

  * `mknod` - Utility to make block or character special files   

  * `more` - Utility to page through text   

  * `mount` - Utility to mount a file system   

  * `mv` - Utility to move/rename files   

  * `ps` - Utility to report process status   

  * `pwd` - Utility to print name of current working directory   





## References

Nayak, K. (2022). *Embedded Linux Step by Step Using Beaglebone Black* [Video file]. Retrieved from https://www.udemy.com/course/embedded-linux-step-by-step-using-beaglebone/