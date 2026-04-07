---
title: "[Week 5] Tutorials 7 and 8"
date: 2026-04-07 18:00:00 -0300
last_modified_at: 2026-04-07 18:01:00 -0300
categories: [Free Software Development, Tutorials, Tutorials 7 and 8]
tags: [kernel, open source, free software development]
---

# Tutorial 7

In short, tutorial 7 presented a detailed architectural "anatomy" of the iio_simple_dummy driver, which is a sample device driver designed to teach the fundamentals of the Industrial I/O (IIO) subsystem. The tutorial systematically deconstructed how sensors are represented through the iio_chan_spec structure, addressing channel types (such as voltage and acceleration), indexing, and modifiers, while explaining the core logic behind the read_raw and write_raw functions used to intercept and process sensor data. It then explored the role of the probe function in memory allocation and device registration, illustrating how these distinct elements connect to form a functional driver that exposes sensor attributes to user space.

# My experience

This tutorial contains a lot of information and concepts that were unfamiliar to me until now, so even though it was just reading, it was a little difficult to understand and connect with the knowledge I had up to that point, but it gave me an idea of ​​how the IIO (Industrial I/O) subsystem of the Linux Kernel works, which is the layer responsible for managing sensors.

# Tutorial 8



# My experience