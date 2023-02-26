# HOW TO... Create a root file system
## For: ARM systems

This how-to will explain the steps needed to put a root file system together for the zedboard development board.
Results:

* Ubuntu 18.04 base system.

In this guide we will use the GUI whenever possible. It is possible to do everything via the command line.

Requirements:

* Ubuntu 20.04 (recommended, can be done in other Linux based OS).
* qemu-user-static
* debootstrap

  <div style="page-break-after: always;"></div>

### Document Version
* v1.2 - 01/25/20 - Updated convert root. Added extra root section.

#### Document History
* ~~v1.1~~ - 01/18/20 - Updated markdown formatting.
* ~~v1.0~~ - 12/1/18 - ARM and ARM64 Instructions for Ubuntu
* ~~v0.XX~~ Untested document version.

  <div style="page-break-after: always;"></div>

### Table of Contents
1. [Creating Directories](#Creating-Directories)
2. [Create a root file system in Ubuntu](#Create-a-root-file-system-in-Ubuntu)
3. [Convert root file system to ramfs file](#Convert-root-file-system-to-ramfs-file)
4. [Extract root file system from ramfs file](#Extract-root-file-system-from-ramfs-files)
5. [Old Method](#Old-build-method)
6. [The Dump](#The-Dump)

  <div style="page-break-after: always;"></div>

### Creating Directories
[Back to TOC](#Table-of-Contents)

* Create three folders.
    - bootfs
    - rootfs
    - git
* Create them in same directory, remember the paths for later usage.

  <div style="page-break-after: always;"></div>

### Create a root file system in Ubuntu
[Back to TOC](#Table-of-Contents)

1. Enter the directory containing your rootfs folder.
2. Run the qemu-debootstrap command to build the arm base system.
    - For ARM64:
        - sudo qemu-debootstrap --arch arm64 bionic rootfs
    - For ARM
        - sudo qemu-debootstrap --arch armhf bionic rootfs
3. Process will build the bionic base system.
4. When completed, chroot into the rootfs directory.
    - sudo chroot rootfs
5. You should now be in the arm filesystem. Check this by checking the Linux architecture.
    - uname -a
    - look for arm in the string that it returns.
6. Add a user to the base system.
    - adduser name_of_user
7. Add user to sudo group
    - usermod -aG sudo name_of_user
8. Set the locale.
    - locale-gen en_US.UTF-8
    - dpkg-reconfigure locales
        - follow the on screen instructions.
9. Exit the chroot.
    - exit

  <div style="page-break-after: always;"></div>

### Convert root file system to ramfs file
[Back to TOC](#Table-of-Contents)

1. Enter the directory that contains the rootfs folder.
2. Convert that directory to a ramfs.
    - find . -print0 | cpio --null -ov --format=newc | gzip -9 > ../ramfs.cpio.gz
3. Wrap the ramfs in a uboot header.
    - mkimage -n SYS_NAME -A arm -T ramdisk -C gzip -d ramfs.cpio.gz ramfs.cpio.gz.ub

  <div style="page-break-after: always;"></div>

### Extract root file system from ramfs file
[Back to TOC](#Table-of-Contents)

1. Enter the directory that contains the ramfs file.
2. Create a folder to extract the ramfs too.
    - mkdir extraction
3. Extract the ramfs.
    - dumpimage -i ramfs.cpio.gz.ub ramfs.cpio.gz
    - zcat ramfs.cpio.gz | cpio -idmv -D extraction

  <div style="page-break-after: always;"></div>

### Old build method
#### Works with Arch Linux and Fedora builds
[Back to TOC](#Table-of-Contents)

* This build method uses a precompiled root file system. This can be compiled from scratch or downloaded from the internet.

* Source: [mikkeloscar github page](https://gist.github.com/mikkeloscar/a85b08881c437795c1b9)

* All of the below require root.

* Replace arm-chroot with rootfs.

```
First, download a root file system for ARCH linux ARM, Ubuntu ARM, or Fedora ARM 
from their website.

You can use qemu-user-static to interpret the ARM instructions in the chroot [1]. 
Get it from AUR if you are on Archlinux.
Copy the qemu-arm-static binary to the chroot.

# cp /usr/bin/qemu-arm-static arm-chroot/usr/bin

Register the qemu-arm-static as an ARM interpreter in the kernel (using binfmt_misc kernel module) [1].

# echo ':arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:
\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:
/usr/bin/qemu-arm-static:' > /proc/sys/fs/binfmt_misc/register

Finally bind {/proc,/dev,/dev/pts,/sys} and chroot into the chroot.

# mount -t proc /proc arm-chroot/proc
# mount -o bind /dev arm-chroot/dev
# mount -o bind /dev/pts arm-chroot/dev/pts
# mount -o bind /sys arm-chroot/sys
# chroot arm-chroot /bin/bash

When in the chroot check that it is working

[chroot]# uname -a
Linux saturn 3.14.6-1-ARCH #1 SMP PREEMPT Sun Jun 8 10:08:38 CEST 2014 armv7l GNU/Linux

Depending on the rootfs you used you might want to set the nameserver to something 
like 8.8.8.8 in /ect/resolv.conf in order to lookup services by domainname 
(or copy the /etc/resolv.conf from your host system). You can also just copy the 
resolv.conf of your host into the chroot file system by exiting, copying, 
and reentering the chroot.
When you are done, exit the chroot and unmount everything

[chroot]# exit
# umount arm-chroot/{sys,proc,dev/pts,dev}
# umount arm-chroot

```

  <div style="page-break-after: always;"></div>

### The Dump
[Back to TOC](#Table-of-Contents)

```
RANDOM BOOT ARG THAT MIGHT BE USEFULL?

devtmpfs.mount=0

DIRS:

mkdir proc sys dev etc/init.d usr/lib

rcS:

/etc/init.d/rcS
adding:

#!/bin/sh
mount -t proc none /proc
mount -t sysfs none /sys
echo /sbin/mdev > /proc/sys/kernel/hotplug
/sbin/mdev -s

Create this link as well? Need to check.
/bin/busybox -> /init

make menuconfig... config stuff for busybox.
```
