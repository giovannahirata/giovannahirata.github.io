---
title: "[Week 5] Tutorials 7 and 8"
date: 2026-04-07 18:00:00 -0300
last_modified_at: 2026-04-08 12:21:00 -0300
categories: [Free Software Development, Tutorials, Tutorials 7 and 8]
tags: [kernel, open source, free software development]
---

# Tutorial 7

In short, [seventh tutorial](https://flusp.ime.usp.br/iio/iio-dummy-anatomy/) presented a detailed architectural "anatomy" of the iio_simple_dummy driver, which is a sample device driver designed to teach the fundamentals of the Industrial I/O (IIO) subsystem. The tutorial systematically deconstructed how sensors are represented through the iio_chan_spec structure, addressing channel types (such as voltage and acceleration), indexing, and modifiers, while explaining the core logic behind the read_raw and write_raw functions used to intercept and process sensor data. It then explored the role of the probe function in memory allocation and device registration, illustrating how these distinct elements connect to form a functional driver that exposes sensor attributes to user space.

# My experience

This tutorial contains a lot of information and concepts that were unfamiliar to me until now, so even though it was just reading, it was a little difficult to understand and connect with the knowledge I had up to that point, but it gave me an idea of ​​how the IIO (Industrial I/O) subsystem of the Linux Kernel works, which is the layer responsible for managing sensors.

# Tutorial 8

The [eighth tutorial](https://flusp.ime.usp.br/iio/experiment-one-iio-dummy/) provided a practical guide on how to interact with and modify the Linux Industrial I/O (IIO) subsystem using the iio_dummy driver, with the goal of understanding the communication between the kernel (sensors) and the userspace.

In summary, the experiment enabled the activation, compilation, and loading of the iio_dummy module. This was done using configfs to create a virtual device and inspect its attributes in the /sys directory.

Furthermore, a guide was presented for modifying the source code to add new channels (a 3-axis magnetometer/compass). This involved updating the header (.h), defining the channel specifications, and implementing the reading logic in the main file (.c).

- Some useful commands and workflow

For module management

- Compile: `make M=drivers/iio/dummy` (builds the module).
- Install: `sudo make modules_install` (installs it to the system directory).
- Load: `sudo modprobe iio_dummy` (inserts the module into the kernel).
- Info: `modinfo iio_dummy` (checks metadata and descriptions).
- Unload: `sudo modprobe -r iio_dummy` (removes the module).

For device instantiation (configfs)

The iio_dummy requires a virtual instance to be created manually:

- Mounting: `sudo mount -t configfs none /mnt/iio_experiments/`
- Creating device: `sudo mkdir /mnt/iio_experiments/iio/devices/dummy/my_device`
- Removal: `sudo rmdir /mnt/iio_experiments/iio/devices/dummy/my_device` (required before unloading the module).

Once created, the device appears in the IIO bus:
- `/sys/bus/iio/devices/iio:deviceX/` (location)
- `cat in_accel_x_raw` (an example for accelerometer raw data for reading)

- A simple worflow that I follow to this tutorial:

Modify: update iio_simple_dummy.h (structs/enums) and iio_simple_dummy.c (channels/read logic).

Compile: rebuild the module on the HOST.

Transfer: send the .ko file to the VM (via scp).

Reload: unload the old module, replace the file, and load the new one.

Instantiate: create the directory via configfs to generate the new sysfs files.

Verify: check for the new files (e.g., in_magn_x_raw) and read their values.

# My experience

While following this tutorial, I encountered some problems executing certain commands, so I had to adapt them to my machine and kernel version, constantly checking and debugging for errors. For example, I had to run `make ARCH=arm64 CROSS_COMPILE=... M=drivers/iio/dummy` to compile and then run `scp drivers/.../iio_dummy.ko root@IP:/root/`. In the end, everything worked out; the magnetometer channels appeared in sysfs and returned the correct outputs. Although this was a new area for me, it certainly increased my experience in developing drivers for the Linux kernel and allowed me to practically understand the structure of the IIO subsystem.