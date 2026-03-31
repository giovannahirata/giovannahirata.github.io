---
title: "[Week 4] Tutorial 4"
date: 2026-03-25 11:58:00 -0300
last_modified_at: 2026-03-31 18:54:00 -0300
categories: [Free Software Development, Tutorials, Tutorial 4]
tags: [kernel, open source, free software development]
---

The [fourth tutorial](https://flusp.ime.usp.br/kernel/char-drivers-intro/) explained concepts related to Linux character devices and applied it to an example of a character device driver.

# About character devices

Character devices provide a way for users to interact with hardware or kernel modules using standard file system calls (open, read, write, close).

Major and Minor numbers:
- Major: identifies the specific driver associated with the device.
- Minor: used by the driver to differentiate between multiple devices or operation modes.

We must use `copy_to user()` and `copy_from_user()` to safely move data across the boudary.

The `struct file_operations` maps standard system calls to our custom C functions in the driver.

# Some important commands summary

| Command                          | Location | Purpose                                                                |
| cat /proc/devices               | VM       | lists registered drivers and their assigned Major Numbers.              |
| mknod <name> c <maj> <min>      | VM       | creates the Device Node (the physical file) in the filesystem.          |
| stat <file>                     | VM       | verifies if a file is a "character special file" and shows its IDs.     |
| insmod <file.ko>                | VM       | loads the compiled module into the Kernel.                              |
| aarch64-linux-gnu-gcc           | Host     | cross-compiler for building test programs (read/write) for the ARM64 VM.|

# Character device workflow

1. Implement the driver: use `alloc_chrdev_region()` to request a Major ID and `cdev_add()` to register the device.

2. Define operations: populate `struct file_operations` with our read and write logic.

3. Load the module: use `insmod` or `modprobe` to insert our .ko file into the running kernel.

4. Identify the Major Number via `cat /proc/devices` and create the interface file using `mknod`.

5. Use user-space programs (compiled on the Host) to write data into the kernel buffer and read it back.

# My experience

While working on tutorial 4, I learned about Character Devices and the essential roles of Major and Minor numbers in the Linux Kernel.

The process wasn't linear, I constantly revisited previous tutorials to check configuration, compilation, and module execution commands. However, this "trial and error" approach was actually the highlight of the experience. It forced me to better understand the "why" of each command, instead of simply copying and pasting.

Although the distinction between Host and VM commands was a bit confusing at first, seeing the data flow between user space and the kernel made everything make more sense. In the end, it worked out well!