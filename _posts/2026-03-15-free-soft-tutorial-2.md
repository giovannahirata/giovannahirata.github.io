---
title: "[Week 2] Tutorial 2"
date: 2026-03-15 10:00:00 -0300
last_modified_at: 2026-03-25 10:59:00 -0300
categories: [Free Software Development, Tutorials, Tutorial 2]
tags: [kernel, open source, free software development]
---

On this second tutorial, we moved from setting up the VM to actually compiling and running a code, using kw - a tool developed by the FLOSS community to automate kernel tasks.

# Installing kw

So, to install kw, we run:
```bash
git clone https://github.com/kworkflow/kworkflow.git "$KW_DIR"
cd "$KW_DIR" && git switch unstable && ./setup.sh --full-installation
```
It handles all the cross-compilation dependencies (like gcc-aarch64-linux-gnu) so we don't have to hunt for packages.

# Cloning the tree (IIO subsystem)

```bash
git clone git://git.kernel.org/pub/scm/linux/kernel/git/jic23/iio.git --branch testing --depth 10
```

Here, we're downloading a specific branch where IIO development happens. Using `--depth 10` saves ~5GB of disk space by only downloading the latest history.

# Kernel configuration

Compiling a standard kernel takes hours, so to do it in minutes, we need a lean .config that only includes what our VM actually uses.

Initialize kw: `cd $IIO_TREE && kw init`

Point to VM: `kw remote --add arm64 root@<VM-IP> --set-default`

The optimization: `* kw ssh --get '~/vm_mod_list'` (gets the list of modules from tutorial 1).

- `make ARCH=arm64 LSMOD=vm_mod_list localmodconfig `(disables everything not in that list).

Personalize: `kw build --menu` (use this to change the "Local version" string so we can prove it's our kernel running).

# Build and deploy commands

|Step|Command|What happens?|
|1. Compile|`kw build`|Cross-compiles the source into an image.|
|2. Deploy|`kw deploy --modules`|Pushes the driver files into the VM via SSH.|
|3. Reboot|`sudo virsh destroy arm64`|Force shutdown a VM.|
|4. Verify|`uname -r`|Run this inside the VM to see our custom name|

# My experience

Following the [second tutorial](https://flusp.ime.usp.br/kernel/build-linux-for-arm-kw/) I was able to configure, build, and boot-test a custom Linux kernel from source code. I also integrated kw to streamline daily kernel development tasks. Once again, I encountered friction due to the same broken packages and dependency issues as before. While I managed to complete the tutorial, the environment's instability led the teaching team and me to decide on a clean installation of a new Linux distro to avoid further technical blockers.