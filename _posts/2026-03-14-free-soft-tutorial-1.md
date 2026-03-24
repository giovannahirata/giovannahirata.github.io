---
title: "[Week 1] Tutorial 1"
date: 2026-03-14 10:00:00 -0300
categories: [Free Software Development, Tutorials, Tutorial 1]
tags: [kernel, open source, free software development]
---

The [first tutorial](https://flusp.ime.usp.br/kernel/build-linux-for-arm/) explained how to configure and build a custom Linux Kernel and boot-test it into a VM. 

Here, we'll show a brief summary of what was done and important commands to easily remember.

# Setup: Linux Kernel development using QEMU and libvirt

Tutorial 1 explains how to set up a kernel testing environment, showing a guide to getting a safe, isolated VM running for kernel development.

We use libvirt to handle more complex tasks related to QEMU flags management, and guestfs-tools to examine the contents of disk images without needing to initializate them. So, we run (for Debian/Ubuntu):
```bash
sudo apt install qemu-system libvirt-daemon-system virtinst libguestfs-tools
```

Permissions:
```bash
sudo usermod -aG libvirt-qemu $USER
```

# Managing disk images

We used these virt-* commands to prep our environment:

Check partitions: virt-filesystems --long -h --all -a my_disk.qcow2

Extract files: virt-copy-out -a my_disk.qcow2 /boot/vmlinuz-xxx . (Great for grabbing the config or current kernel).

Resize: qemu-img create -f qcow2 new.qcow2 5G

virt-resize --expand /dev/sda1 old.qcow2 new.qcow2

# Daily workflow

Once our VM is registered in libvirt, these are the commands we'll use frequently:

|Action|Command|
|Start and connect|virsh start --console arm64|
|Detach (escape)|Ctrl + ] (exit console without killing VM)|
|Find IP|virsh net-dhcp-leases default|
|Force stop|virsh destroy arm64|
|Push code|scp my_driver.ko root@<VM_IP>:/root/|

# Some lessons learned from this tutorial

- Never test a custom kernel on host machine, but always use the VM, that'll avoid problems.
- Use SSH and virtiofs (shared folders). Compile inside a VM is slow. instead compile on host and 'push' to the guest.
- To minimize build time, run lsmod  to list the currently loaded kernel modules and save that list into a file.

# My experience

I successfully finalized the tutorial, though the process was complicated by broken packages and dependency issues caused by an outdated OS. After these issues persisted for two weeks, and with significant support and guidance from the professor and classroom monitors, I migrated to a different operating system to ensure I could progress with the subject without further technical delays.