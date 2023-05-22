<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Linux Device Drivers</a> > Exercise: Pseudo Character Driver

# Exercise: Pseudo Character Driver



## Problem Statement

* Write a character driver to deal with a pseudo character device
* The pseudo-device is a memory buffer of some size
* The driver you write must support reading, writing and seeking to this driver
* Test the driver functionality by running user-level command such as echo, dd, cat and by writing user level programs



<img src="img/pseudo-char-driver-1684777614311-33.png" alt="pseudo-char-driver" width="700">



### Connection Establishment between Device File Access and the Driver

1. Create device number
   * Request the kernel to dynamically allocate the device numbers(s)
2. Make a char device registration with the Virtual File System (VFS). (`CDEV_ADD`)
3. Create device files
4. Implement the driver's file operation methods for `open`, `close`, `read`, `write`, `llseek`, etc.

### Expected Results

* Do `make host` and see if you are getting `pcd.ko` from `pcd.c`.

* Insert the LKM (`sudo insmod pcd.ko`), run `dmesg` and see if the messages are getting printed.

* Check `/sys/class/` if you see `pcd_class/` which should be created by the **`class_create()`** kernel function.

  * `pcd_class/` directory should contain `pcd` (the same name as your LKM) directory
  * `pcd_class/pcd/` directory should contain `dev` file whose contents is the device number `<major:minor>`
  * `pcd_class/pcd/` directory should also contain `uevent` whose contents is major number, minor number and devname.

   `udev` creates the device file under `/dev` directory according to these details which are created and populated by the **`device_create()`** kernel function.

* Check `/dev/` if you see the device file `pcd`.

* Remove the LKM (`sudo rmmod pcd.ko`), run `dmesg` and see if the messages are getting printed.



## Pseduo Character Driver (PCD) File Operation Methods

* **Open**

  ```c
  /**
   * pcd_open() - Pseudo char driver open method
   * @inode: Pointer to an inode associated with the filename
   * @file: Pointer to a file object
   *
   * Opens a file, 
   * returns 0 on success, negative error code otherwise
   */
  int pcd_open(struct inode *inode, struct file *filp)
  {
      return 0;
  }
  ```

  * Initialize the device or make device respond to subsequent system calls such as `read()` and `write()`.
  * Detect device initialization errors
  * Check open permission (`O_RDONLY`, `O_WRONLY`, `O_RDWR`)
  * Identify the opened device using the *minor number*
  * Prepare device private data structure if required
  * Update `f_pos` if required
  * `open` method is optional. If not provided, `open` will always succeed and driver is not notified.

* **Close** (e.g., `close(fd)` system call from the user space)

  ```c
  /**
   * pcd_release() - Pseudo char driver method to handle close() system call
   * @inode: Pointer to an inode associated with the filename
   * @filp: Pointer to a file object
   *
   * VFS Releases the file object. Called when the last reference to an open file is closed, i.e.,
   * when the f_count field of the file object becomes 0.
   * Returns 0 on success, negative error code otherwise
   */
  int pcd_release(struct inode *inode, struct file *filp)
  {
      return 0;
  }
  ```

  * Does the reverse operations of open method. Simply put, release method should put the device in its default state, i.e., the state before the open method was called.

    e.g., If open method brings the device out of low power mode, then release method may send the device back to the low power mode.

  * Free any data structures allocated by the open method.

  * Returns 0 on success, negative error code otherwise (e.g., the device does not respond when you try to de-initialize the device).

* **Read** (e.g., `read(fd, buff, 20)` system call from the user space)

  ```c
  /**
   * pcd_read() - Pseudo char driver method to handle read() system call
   * @filp: Pointer to a file object
   * @buff: Pointer of user buffer
   * @count: Read count given by user
   * @f_pos: Pointer of current file position from which the read has to begin
   *
   * Reads a device file @count byte of data from @f_pos, returns the data back to @buff (user),
   * and updates @f_pos. Returns the number of bytes successfully read, 0 if there is no bytes to 
   * read (EOF), appropriate error code (negative value) otherwise.
   */
  ssize_t pcd_read(struct file *filp, char __user *buff, size_t count, loff_t *f_pos)
  {
      return 0;
  }
  ```

  > `__user`
  >
  > * Optional macro that alerts the programmer that this is a user level pointer so cannot be trusted for direct dereferencing.
  > * A macro used with user level pointers which tells the developer not to trust or assume it as a valid pointer to avoid kernel faults.
  > * Never try to dereference user given pointers directly in kernel level programming. Instead, use dedicated kernel functions such as `copy_to_user` and `copy_from_user`.
  > * GCC doesn't care whether you use `__user` macro with user level pointer or not. This is checked by `sparse`, a semantic checker tool of Linux kernel to find possible coding faults.

  * Read `count` bytes from a device starting at position `f_pos`.
  * Update the `f_pos` by adding the number bytes successfully read.
  * A return value less than `count` does not mean that an error has occurred.

* **Write** (e.g., `write(fd, buff, 20)`)

  ```c
  /**
   * pcd_write() - Pseudo char driver method to handle write() system call
   * @filp: Pointer to a file object
   * @buff: Pointer of user buffer
   * @count: Write count given by user
   * @f_pos: Pointer of current file position from which the write has to begin
   *
   * Writes a device file @count byte of data from @f_pos, returns the data back to 
   * @buff (user) and updates @f_pos. Returns the number of bytes successfully written,
   * appropriate error code (negative value) otherwise.
   */
  ssize_t pcd_read(struct file *filp, char __user *buff, size_t count, loff_t *f_pos);
  ```

  * Write `count` bytes into the device starting at position `f_pos`.
  * Update the `f_pos` by adding the number of bytes successfully written

* **llseek** (e.g., `llseek(fd, buff, 20)`)

  Used to alter the `f_pos`.

  ```c
  /**
   * pcd_lseek() - Pseudo char driver method to handle llseek() system call
   * @filp: Pointer to a file object
   * @off: Offset value
   * @whence: Origin 
   * 		- SEEK_SET: The file offset is set to @off bytes
   *		- SEEK_CUR: The file offset is set to its current location plus @off bytes
   * 		- SEEK_END: The file offset is set to the size of the file plus @off bytes
   *
   * Updates the file poitner by using @off and @whence information. Returns the newly updated 
   * file position on success, error otherwise.
   */
  loff_t pcd_lseek(struct file *filp, loff_t off, int whence);
  ```
