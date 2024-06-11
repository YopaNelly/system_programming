# Building and Loading a Linux Kernel Module

This guide will walk you through the steps to build and load a simple "Hello World" kernel module in Ubuntu.

## Prerequisites

- Ubuntu Linux
- Basic knowledge of Linux commands
- GCC compiler installed
- Kernel headers installed

## Steps

### 1. Create the Source Code for the Kernel Module

Create a directory for your kernel module and navigate into it:

```bash
mkdir ~/kernel_module
cd ~/kernel_module
```
Now, create the source file helloworld.c:

```bash

nano helloworld.c
```
Inside helloworld.c, paste the following code:

```c

// helloworld.c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Yopa Nelly");
MODULE_DESCRIPTION("A simple Hello World LKM");
MODULE_VERSION("0.1");

static int __init hello_init(void) {
    printk(KERN_INFO "Hello, World!\n");
    return 0;
}

static void __exit hello_exit(void) {
    printk(KERN_INFO "Goodbye, World!\n");
}

module_init(hello_init);
module_exit(hello_exit);
```
Save and close the file (Ctrl+X, then Y, and Enter).

2. Create the Makefile

Create a Makefile in the same directory:

```bash

nano Makefile
```
Inside Makefile, paste the following code:

```Makefile

# Makefile
obj-m := helloworld.o
KERNEL_SRC ?= /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)

all:
    $(MAKE) -C $(KERNEL_SRC) M=$(PWD) modules

clean:
    $(MAKE) -C $(KERNEL_SRC) M=$(PWD) clean

modules_install:
    $(MAKE) -C $(KERNEL_SRC) M=$(PWD) modules_install
```
Specifically, ensure that the lines:

```makefile

    $(MAKE) -C $(KERNEL_SRC) M=$(shell pwd) $@
```
are indented with a tab character (not spaces).

3. Build the Kernel Module

Run the following command to compile the kernel module:

```bash

make
```
4. Install the Kernel Module

Install the kernel module to the appropriate directory:

```bash

sudo make modules_install
```
5. Update Module Dependencies

Ensure the kernel is aware of the newly installed module:

```bash

sudo depmod
```
6. Load the Kernel Module

Load the kernel module using modprobe:

```bash

sudo modprobe helloworld
```
7. Verify the Module is Loaded

Check if the module is loaded successfully:

```bash

lsmod | grep helloworld
```
8. Check Kernel Messages

View the kernel log messages to confirm that the module has been loaded without any issues:

```bash

sudo dmesg | tail
```
9. Unload the Kernel Module

If you want to remove the kernel module, use the following command:

```bash

sudo rmmod helloworld
```
Again, you can verify it by checking the kernel messages:

```bash

sudo dmesg | tail
```
Following these steps, you should be able to create, build, install, and manage a simple "Hello World" kernel module in Ubuntu.
