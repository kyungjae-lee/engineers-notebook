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

### Open

* Initialize the device or make device respond to subsequent system calls such as `read()` and `write()`.

* Detect device initialization errors

* Check open permission (`O_RDONLY`, `O_WRONLY`, `O_RDWR`)

* Identify the opened device using the *minor number*

* Prepare device private data structure if required

* Update `f_pos` if required

* `open` method is optional. If not provided, `open` will always succeed and driver is not notified.

* Implementation:

  ```c
  /**
   * pcd_open()
   * Desc.    : Handles the open() system call from the user space
   * Param.   : @inode - pointer to inode object
   *            @filp - pointer to file object
   * Returns  : 0 on success, negative error code otherwise
   * Note     : N/A
   */
  int pcd_open(struct inode *inode, struct file *filp)
  {
      pr_info("open was successful\n");
      return 0;
  }
  ```

### Close (e.g., `close(fd)` system call from the user space)

* Does the reverse operations of open method. Simply put, release method should put the device in its default state, i.e., the state before the open method was called.

  e.g., If open method brings the device out of low power mode, then release method may send the device back to the low power mode.

* Free any data structures allocated by the open method.

* Returns 0 on success, negative error code otherwise (e.g., the device does not respond when you try to de-initialize the device).

* Implementation:

  ```c
  /**
   * pcd_release()
   * Desc.    : Handles the close() system call from the user space
   * Param.   : @inode - pointer to inode struct
   *            @filp - pointer to file struct
   * Returns  : 0 on success, negative error code otherwise
   * Note     : VFS releases the file object. Called when the last reference to an
   *            open file is closed (i.e., when the f_count field of the file object
   *            becomes 0.
   */
  int pcd_release(struct inode *inode, struct file *filp)
  {
      pr_info("close was successful\n");
      return 0;
  }
  ```

### Read (e.g., `read(fd, buff, 20)` system call from the user space)



<img src="img/read-method.png" alt="read-method" width="600">



* Read `count` bytes from a device starting at position `f_pos`.
* Update the `f_pos` by adding the number bytes successfully read.
* A return value less than `count` does not mean that an error has occurred.
* `f_op` and `f_pos`



<img src="img/f-op-and-f-pos.png" alt="f-op-and-f-pos" width="900">



* Data copying (kernel space $\to$ user space)

  ```c
  /**
   * copy_to_user()
   * Desc.	: Copies data from kernel space to user space (during read operation)
   * Param.	: @to - destination address in user space
   *			  @from - source address in kernel space
   *			  @n - number of bytes to copy
   * Returns 	: 0 on success, number of bytes that could not be copied otherwise
   * Notes	: It checks whether the user space pointer @to is valid or not. 
   *			  If the pointer is invalid, copy will not be performed. If an invalid address
   * 			  is encountered during the copy, only part of the data is copied. In either case,
   * 			  the return value is the amount of memory left to be copied.
   *			  If this function returns a non-zero value, you should assume that there was
   * 			  a problem during the data copy.
   */
  unsigned long copy_to_user(void __user *to, const void *from, unsigned long n);
  ```

  > If `copy_to_user()` returns a non-zero value, your read function should return an appropriate error code (-EFAULT).

* Implementation:

  1. Check the user requested `count` value against `DEV_MEM_SIZE` of the device.
     * If `f_pos` + `count` > `DEV_MEM_SIZE`, then adjust the `count` (`count` = `DEV_MEM_SIZE` - `f_pos`).
  2. Copy `count` number of bytes from device memory to user buffer.
  3. Update `f_pos`
  4. Return number of bytes successfully read or error code
  5. If `f_pos` is at EOF, then return 0.

  ```c
  /**
   * pcd_read()
   * Desc.    : Handles the read() system call from the user space
   * Param.   : @filp - pointer to file object
   *            @buff - pointer to user buffer
   *            @count - read count given by the user
   *            @f_pos - pointer to current file position from which the read has to begin
   * Returns  : The number of bytes read on success,
   *            0 if there is no bytes to read (EOF),
   *            appropriate error code (negative value) otherwise
   * Note     : Reads a device file @count byte(s) of data from @f_pos, returns the data back to
   *            @buff (user), and updates @f_pos.
   */
  ssize_t pcd_read(struct file *filp, char __user *buff, size_t count, loff_t *f_pos)
  {
      pr_info("read requested for %zu bytes\n", count);
      return 0;
  }
  ```

  > `__user`
  >
  > * Optional macro that alerts the programmer that this is a user level pointer so cannot be trusted for direct dereferencing.
  > * A macro used with user level pointers which tells the developer not to trust or assume it as a valid pointer to avoid kernel faults.
  > * Never try to dereference user given pointers directly in kernel level programming. Instead, use dedicated kernel functions such as `copy_to_user` and `copy_from_user`.
  > * GCC doesn't care whether you use `__user` macro with user level pointer or not. This is checked by `sparse`, a semantic checker tool of Linux kernel to find possible coding faults.

### Write (e.g., `write(fd, buff, 20)`)

* Write `count` bytes into the device starting at position `f_pos`.

* Update the `f_pos` by adding the number of bytes successfully written

* Data copying (user space $\to$ kernel space)

  ```c
  /**
   * copy_from_user()
   * Desc.	: Copies data from user space to kernel space (during write operation)
   * Param.	: @to - destination address in user space
   *			  @from - source address in kernel space
   *			  @n - number of bytes to copy
   * Returns 	: 0 on success, number of bytes that could not be copied otherwise
   * Notes	: It checks whether the user space pointer @to is valid or not. 
   *			  If the pointer is invalid, copy will not be performed. If an invalid address
   * 			  is encountered during the copy, only part of the data is copied. In either case,
   * 			  the return value is the amount of memory left to be copied.
   *			  If this function returns a non-zero value, you should assume that there was
   * 			  a problem during the data copy.
   */
  unsigned long copy_from_user(void *to, const void __user *from, unsigned long n);
  ```

  > If `copy_from_user()` returns a non-zero value, your write function should return an appropriate error code (-EFAULT).

* Implementation:

  ```c
  /**
   * pcd_write()
   * Desc.    : Handles the write() system call from the user space
   * Param.   : @filp - pointer to file object
   *            @buff - pointer to user buffer
   *            @count - read count given by the user
   *            @f_pos - pointer to current file position from which the read has to begin
   * Returns  : The number of bytes written on success,
   *            appropriate error code (negative value) otherwise
   * Note     : Writes a device file @count byte(s) of data from @f_pos, returns the data back to
   *            @buff (user) and updates @f_pos.
   */
  ssize_t pcd_write(struct file *filp, const char __user *buff, size_t count, loff_t *f_pos)
  {
      pr_info("write requested for %zu bytes\n", count);
      return 0;
  }
  ```

### llseek (e.g., `llseek(fd, buff, 20)`)

* Used to alter the `f_pos`.

* Implementation:

  ```c
  /**
   * pcd_lseek()
   * Desc.    : Handles the llseek() system call
   * Param.   : @filp - pointer to file object
   *            @off - offset value
   *            @whence - origin
   *              - SEEK_SET: The file offset is set to @off bytes
   *              - SEEK_CUR: The file offset is set to its current location plus @off bytes
   *              - SEEK_END: The file offset is set to the size of the file plus @off bytes
   * Returns  : Newly updated file position on sucess, error code other wise
   * Note     : Updates the file pointer by using @off and @whence information.
   */
  loff_t pcd_lseek(struct file *filp, loff_t off, int whence)
  {
      pr_info("lseek requested\n");
      return 0;
  }
  ```

