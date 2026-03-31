---
title: "[Week 3] Tutorial 3"
date: 2026-03-16 10:00:00 -0300
last_modified_at: 2026-03-25 11:57:00 -0300
categories: [Free Software Development, Tutorials, Tutorial 3]
tags: [kernel, open source, free software development]
---

This third tutorial moves from running a kernel to actually writing code for it. Here, I learned how to create a module, tell the Linux build system (Kbuild) about it, and manage it while the system is running.

# Add a code to the Kernel

To add a new feature (like `simple_mod.c`), we must touch three files in the source tree:

The source (`.c`): our actual code (includes `module_init` and `module_exit`).

The Kconfig: this allows the module to show up in `menuconfig`.

- Key attribute: `tristate` (allows choosing Yes-builtin, Module, or No).

The Makefile: allows enabling the configuration of our module with kbuild.

- Syntax: `obj-$(CONFIG_SIMPLE_MOD) += simple_mod.o`

Instead of editing the .config file by hand, use the TUI (Text User Interface).

- Command: `make menuconfig` (or `kw build --menu`).
(the / key is very helpful to searching for module)

# Managing modules in the VM

Once the VM is running, we don't always need to reboot to test code:

|Command|Action|When to use it?|
|insmod <file.ko>|Load by path|Testing a local .ko file quickly.|
|modprobe <name>|Load by name|Best for production; it handles dependencies automatically.|
|rmmod <name>|Unload|Safely removing your code from the running kernel.|
|lsmod|List|"Checking 'Is my driver actually running right now?'|
|modinfo|Info|"Checking the author, license, or version of a module."|

# Debugging with Kernel Logs

Since we can't use `printf` in the kernel, we use `pr_info()`. These messages go to a special buffer.

`dmesg | tail`: frequent command we'll run. It shows the last few lines of the kernel log.

# Exporting symbols

If we have two modules where Module B needs to call a function in Module A:

In Module A: use `EXPORT_SYMBOL_GPL(my_function);`.

In Module B: use `extern void my_function(void);`.

In the VM: run `depmod --quick` so the kernel understands that B depends on A.
Running depmod updates the modules.dep file, which is the "map" the kernel uses to link our two modules together.

# Summary of the workflow

Modify code (.c)

Declare it (Kconfig & Makefile)

Toggle it (make menuconfig)

Inject it (insmod / modprobe)

Watch it (dmesg)

# My experience

The [third tutorial](https://flusp.ime.usp.br/kernel/modules-intro/) explained how to add a module to the Linux codebase, create its configuration, and enable it to be compiled, installed, and dynamically loaded or unloaded. It also covered how to use module functionalities within other modules. During this class, I spent the entire time switching my machine's operating system from Ubuntu to Debian. After this change, I was finally able to redo all the previous tutorials—and this one—correctly and without any problems (yay! thank you Professor and Nelson). This fresh start allowed me to better engage with the process and the available material.