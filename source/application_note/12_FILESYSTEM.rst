File System
===========

.. contents::
  :local:
  :depth: 2

FATFS File System
-----------------

Storage is a key feature of embedded system. AmebaPro2 provides flexible
method of storage management. In this chapter, three kinds of
application scenarios (SD/RAM/flash) will be mentioned.

FATFS Module
~~~~~~~~~~~~

.. image:: ../_static/12_FILESYSTEM/image1.png
   :align: center

AmebaPro2 utilizes FAT File system Module to provide access to low level
storage devices. Applications can manage and operate the file system
through FATFS API.

FATFS API
~~~~~~~~~

AmebaPro2 SDK uses open source FATFS module. The application interface
provides various functions for applications to manipulate the file
system.

(1) File Access

-  f_open - Open/Create a file

-  f_close - Close an open file

-  f_read - Read data from the file

-  f_write - Write data to the file

-  f_lseek - Move read/write pointer, Expand size

-  f_truncate - Truncate file size

-  f_sync - Flush cached data

-  f_forward - Forward data to the stream

-  f_expand - Allocate a contiguous block to the file

-  f_gets - Read a string

-  f_putc - Write a character

-  f_puts - Write a string

-  f_printf - Write a formatted string

-  f_tell - Get current read/write pointer

-  f_eof - Test for end-of-file

-  f_size - Get size

-  f_error - Test for an error

(2) Directory Access

-  f_opendir - Open a directory

-  f_closedir - Close an open directory

-  f_readdir - Read an directory item

-  f_findfirst - Open a directory and read the first item matched

-  f_findnext - Read a next item matched

(3) File and Directory Management

-  f_stat - Check existance of a file or sub-directory

-  f_unlink - Remove a file or sub-directory

-  f_rename - Rename/Move a file or sub-directory

-  f_chmod - Change attribute of a file or sub-directory

-  f_utime - Change timestamp of a file or sub-directory

-  f_mkdir - Create a sub-directory

-  f_chdir - Change current directory

-  f_chdrive - Change current drive

-  f_getcwd - Retrieve the current directory and drive

(4) Volume Management and System Configuration

-  f_mount - Register/Unregister the work area of the volume

-  f_mkfs - Create an FAT volume on the logical drive

-  f_fdisk - Create logical drives on the physical drive

-  f_getfree - Get total size and free size on the volume

-  f_getlabel - Get volume label

-  f_setlabel - Set volume label

-  f_setcp - Set active code page

   More details about the usage of FATFS API, please visit http://elm-chan.org/fsw/ff/00index_e.html


Setup the timestamp
~~~~~~~~~~~~~~~~~~~

FATFS setting time is set through the following API, users can set it
according to the current real time, such as rtc or sntp.

sdk\\component\\file_system\\fatfs\\r0.14\\diskio.c

.. code-block:: c

    DWORD get_fattime(void)
    {
        DWORD time_abs;

        time_abs = ((DWORD)(2016 - 1980) << 25) /* Fixed to Feb. 2, 2016 */
                   | ((DWORD)2 << 21)
                   | ((DWORD)2 << 16)
                   | ((DWORD)0 << 11)
                   | ((DWORD)0 << 5)
                   | ((DWORD)0 >> 1);

        return time_abs;
    }


LITTLEFS File System
--------------------

The little file system (LittleFS) is a fail-safe file system designed
for embedded systems, specifically for microcontrollers that use
external flash storage.

There are three challenges for embedded storage on microcontrollers and
flash storage: power loss, wear and limited RAM and ROM. This file
system provides the solution to these challenges.

-  Bounded RAM/ROM - This file system works with a limited amount of
   memory. It avoids recursion and limits dynamic memory to configurable
   buffers that can be provided statically.

-  Power-loss resilient - We have designed this for operating systems
   that may have random power failures. It has strong copy-on-write
   guarantees and keeps storage on disk in a valid state.

-  Wear leveling - Because the most common form of embedded storage is
   erodible flash memories, the file system provides a form of dynamic
   wear leveling for systems that cannot fit a full flash translation
   layer.

LITTLEFS Module
~~~~~~~~~~~~~~~

.. image:: ../_static/12_FILESYSTEM/image2.png
   :align: center

AmebaPro2 utilizes Littlefs File system Module to provide access to low
level storage devices. Applications can manage and operate the file
system through Littlefs API.

LITTLEFS API
~~~~~~~~~~~~

(1) File Access

-  lfs_file_open - Open a file.

-  lfs_file_opencfg - Open a file with extra configuration.

-  lfs_file_close - Close a file.

-  lfs_file_sync - Synchronize a file on storage.

-  lfs_file_read - Read data from file.

-  lfs_file_write - Write data to file

-  lfs_file_seek - Change the position of the file.

-  lfs_file_truncate - Truncates the size of the file to the specified
   size

-  lfs_file_tell - Return the position of the file.

-  lfs_file_rewind - Change the position of the file to the beginning of
   the file.

-  lfs_file_size - Return the size of the file.

(2) Directory Access

-  lfs_mkdir - Create a directory.

-  lfs_dir_open - Open a directory.

-  lfs_dir_close - Close a directory

-  lfs_dir_read - Read an entry in the directory.

-  lfs_dir_seek - Change the position of the directory.

-  lfs_dir_tell - Return the position of the directory.

-  lfs_dir_rewind - Change the position of the directory to the
   beginning of the directory.

(3) File and Directory Management

-  lfs_remove - Removes a file or directory.

-  lfs_rename - Rename or move a file or directory.

-  lfs_stat - Find info about a file or directory.

-  lfs_getattr - Get a custom attribute.

-  lfs_setattr - Set custom attributes.

-  lfs_removeattr - Removes a custom attribute

-  lfs_fs_size - Finds the current size of the file system.

-  lfs_fs_traverse - Traverse through all blocks in use by the file
   system.

(4) Volume Management and System Configuration

-  lfs_format - Format a block device with the littlefs.

-  lfs_mount - Mounts a littlefs.

-  lfs_unmount - Unmounts a littlefs.

-  lfs_migrate - Attempts to migrate a previous version of littlefs.

Modify the space
~~~~~~~~~~~~~~~~

Both of Nor and Nand need to assign the start address and block size. Please
reference the platform_opts.h to do the setup. This side of the file system must be placed behind the FW, the size according to their own use of context setting, if it is NAND FLASH is recommended that half of the space to the FW, the back half of the FILESYSTEM and USER DATA, if it is a NOR FLASH according to the current PARTITION TABLE setup placed!

.. code-block:: c

   #define NAND_APP_BASE           0x4000000 //NAND FLASH FILESYSTEM begin address It need to alignment block size, the default is 512 BLOCK. = 512(block index) * 64(page number per block) * 2048(page size)

   #define FLASH_APP_BASE          USER_DATA_END  //Nor flash file system base address

   #define FLASH_FILESYS_SIZE      (NOR_FLASH_END - FLASH_APP_BASE)  //flash file system size(Nor and Nand)

For the ram disk setup, please modify the fatfs_ramdisk_api.c

.. code-block:: c

   #define RAM_DISK_SZIE         1024*1024*10
   
   #define SECTOR_SIZE_RAM       512
   
   #define SECTOR_COUNT_RAM      (RAM_DISK_SZIE/512)

VFS 
~~~~~~~~~~~~~~~~

Through the virtual file system, it can support the operation of different operating systems LITTLEFS and FATFS, file system operations, and support for different interfcae, currently supports NAND, NOR, RAM, and SD CARD interface, you can refer to the example_std_file.c for details.