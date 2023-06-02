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
      pr_info("Read requested for %zu bytes\n", count);
      pr_info("Current file position = %lld\n", *f_pos);
  
      /* Adjust the count */
      if ((*(f_pos) + count) > DEV_MEM_SIZE)
          count = DEV_MEM_SIZE - *f_pos;
  
      /* Copy to user buffer 
         Note: Global data access should be synchronized by using mutual exclusion to avoid
         race condition. */
      if (copy_to_user(buff, &device_buffer[*f_pos], count))
      {
          return -EFAULT;
      }
  
      /* Update the current file position f_pos */
      *f_pos += count;
  
      pr_info("%zu bytes successfully read\n", count);
      pr_info("Updated file position = %lld\n", *f_pos);
  
      /* Return the number of bytes successfully read */
      return count;
  }
  ```
  
  > `__user`
  >
  > * Optional macro that alerts the programmer that this is a user level pointer so cannot be trusted for direct dereferencing.
  > * A macro used with user level pointers which tells the developer not to trust or assume it as a valid pointer to avoid kernel faults.
  > * Never try to dereference user given pointers directly in kernel level programming. Instead, use dedicated kernel functions such as `copy_to_user` and `copy_from_user`.
  > * GCC doesn't care whether you use `__user` macro with user level pointer or not. This is checked by `sparse`, a semantic checker tool of Linux kernel to find possible coding faults.

### Write (e.g., `write(fd, buff, 20)`)



<img src="img/write-method.png" alt="write-method" width="600">



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

  1. Check the user requested `count` value against `DEV_MEM_SIZE` of the device
     * If `f_pos` + `count` > `DEV_MEM_SIZE`, then adjust the `count` (`count` = `DEV_MEM_SIZE` - `f_pos`).
  2. Copy `count` number of bytes from user buffer to device memory
  3. Update `f_pos`
  4. Return the number of bytes successfully written or error code
  
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
      pr_info("Write requested for %zu bytes\n", count);
      pr_info("Current file position = %lld\n", *f_pos);
  
      /* Adjust the count */
      if ((*(f_pos) + count) > DEV_MEM_SIZE)
          count = DEV_MEM_SIZE - *f_pos;
  
      if (!count)
      {
          pr_err("No more space left on the device\n");
          return -ENOMEM;
      }
  
      /* Copy from user buffer 
         Note: Global data access should be synchronized by using mutual exclusion to avoid
         race condition. */
      if (copy_from_user(&device_buffer[*f_pos], buff, count))
      {
          return -EFAULT;
      }
  
      /* Update the current file position f_pos */
      *f_pos += count;
  
      pr_info("%zu bytes successfully written\n", count);
      pr_info("Updated file position = %lld\n", *f_pos);
  
      /* Return the number of bytes successfully written */
      return count;
  }
  ```

### llseek (e.g., `llseek(fd, buff, 20)`)

* Used to alter the `f_pos`.

  * If `whence` = `SEEK_SET`, `filp->f_pos` = `off`

  * If `whence` = `SEEK_CUR`, `filp->f_pos` = `filp->f_pos + off`

  * If `whence` = `SEEK_END`, `filp->f_pos` = `DEV_MEM_SIZE + off`

* Implementation:

  ```c
  /**
   * pcd_lseek()
   * Desc.    : Handles the llseek() system call
   * Param.   : @filp - pointer to file object
   *            @offset - offset value
   *            @whence - origin
   *              - SEEK_SET: The file offset is set to @offset bytes
   *              - SEEK_CUR: The file offset is set to its current location plus @offset bytes
   *              - SEEK_END: The file offset is set to the size of the file plus @offset bytes
   * Returns  : Newly updated file position on sucess, error code other wise
   * Note     : Updates the file pointer by using @offset and @whence information.
   *            Allows the file offset to be set beyond the end of the file (but this does not
   *            change the size of the file).
   */
  loff_t pcd_lseek(struct file *filp, loff_t offset, int whence)
  {
      loff_t temp;
  
      pr_info("lseek requested\n");
      pr_info("Current file position = %lld\n", filp->f_pos);
  
      switch (whence)
      {   
          case SEEK_SET:
              if ((offset > DEV_MEM_SIZE) || (offset < 0)) 
                  return -EINVAL;
              filp->f_pos = offset;
              break;
          case SEEK_CUR:
              temp = filp->f_pos + offset;
              if ((temp > DEV_MEM_SIZE) || (temp < 0)) 
                  return -EINVAL;
              filp->f_pos = temp;
              break;
          case SEEK_END:
              temp = DEV_MEM_SIZE + offset;
              if ((temp > DEV_MEM_SIZE) || (temp < 0)) 
                  return -EINVAL;
              filp->f_pos = temp;
              break;
          default:
              return -EINVAL;
      }   
  
      pr_info("Updated file position = %lld\n", filp->f_pos);
      return filp->f_pos;
  }
  ```



## Testing

### On the Host

1. Build the kernel module `pcd.c` against the host Linux kernel version

2. Insert the mode: `sudo insmod pcd.ko`

3. Test the pseudo character driver: `echo "Hello, welcome!" > /dev/pcd` $\to$ `dmesg`

   Output:

   ```plain
   [21343.359074] pcd_driver_init :Device number <major>:<minor> = 509:0
   [21343.359135] pcd_driver_init :Module init was successful
   [21401.491786] pcd_open :open was successful
   [21401.491856] pcd_write :Write requested for 16 bytes
   [21401.491863] pcd_write :Current file position = 0
   [21401.491868] pcd_write :16 bytes successfully written
   [21401.491871] pcd_write :Updated file position = 16
   [21401.491901] pcd_release :close was successful
   ```

   > `echo` command opens the pseudo character device by invoking the `open()` system call. (L5: `pcd_open()` gets called from the driver)
   >
   > L7: When a file is opened, the current file position is always 0.
   >
   > L10: `echo` invokes `close()` system call, and subsequently `pcd_release()` of the driver gets called.

4. Check the contents of the device (buffer): `cat /dev/pcd`

   Output:

   ```plain
   Hello, welcome!
   ```

5. Check the read activity: `dmesg | tail`

   ```plain
   [22741.144956] pcd_open :open was successful
   [22741.144967] pcd_read :Read requested for 131072 bytes
   [22741.144969] pcd_read :Current file position = 0
   [22741.144971] pcd_read :512 bytes successfully read
   [22741.144972] pcd_read :Updated file position = 512
   [22741.144980] pcd_read :Read requested for 131072 bytes
   [22741.144981] pcd_read :Current file position = 512
   [22741.144982] pcd_read :0 bytes successfully read
   [22741.144983] pcd_read :Updated file position = 512
   [22741.144991] pcd_release :close was successful
   ```

   > L2: Read request for 131072 bytes because that's how the `cat` program is implemented!
   >
   > L5: Updated file position is 512 bytes (i.e., `DEV_MEM_SIZE`) because that's the size of our buffer.
   >
   > L6: `cat` requests to read again
   >
   > L8: 0 bytes since no more to read

6. Attempt copying a large text file (> 512 Bytes) into `/dev/pcd`: `cp file.txt /dev/pcd`

   Output:

   ```plain
   cp: error writing '/dev/pcd': Cannot allocate memory
   ```

   And run `dmesg` to see what happened:

   ```plain
   [24039.217897] pcd_driver_init :Module init was successful
   [24045.901773] pcd_open :open was successful
   [24045.901788] pcd_write :Write requested for 1717 bytes
   [24045.901790] pcd_write :Current file position = 0
   [24045.901792] pcd_write :512 bytes successfully written
   [24045.901793] pcd_write :Updated file position = 512
   [24045.901795] pcd_write :Write requested for 1205 bytes
   [24045.901796] pcd_write :Current file position = 512
   [24045.901797] pcd_write :No more space left on the device
   [24045.901973] pcd_release :release was successful
   ```

   > L6: 512 bytes successfully written
   >
   > L7: The file position reached the end
   >
   > L8: `cp` command attempts to copy the rest of the contents (1717 - 512 = 1205 bytes)
   >
   > L9: Since the file position is already at the end, no more write can be done. (`pcd_write()` returns the error code `-ENOMEM` which triggers the error message `cp: error writing '/dev/pcd': Cannot allocate memory`.)
