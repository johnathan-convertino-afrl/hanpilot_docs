# HOW TO... Create a sdcard.
## For: Terasic Hanpilot Arria 10 ARMHF
### Author: Jay Convertino

This howto will explain the steps needed to put a sdcard together for the a10soc development board.

Requirements:
* Ubuntu 18.04 (recommened, can be done in other Linux based OS).

### ROOTFS BOOTFS
If you haven't, follow the other two howtos and create the files needed.

  <div style="page-break-after: always;"></div>

### Document Version
* v1.2 - 01/18/20 - Updated markdown formatting.

#### Document History
* ~~v1.1~~ - 12/12/18 - Added Rocketboards tweaks.
* ~~v1.0~~ - 12/01/18 - ARM and ARM64 Instructions for Ubuntu
* ~~v0.XX~~ Untested document version.

  <div style="page-break-after: always;"></div>

## Table of Contents
1. [Creating Partitions](#Creating-Partitons)
2. [Copy Data to the SDCARD](#Copy-Data-to-the-SDCARD)

### References
* [Analog Devices, SDCARD alterations with fdisk](https://wiki.analog.com/resources/tools-software/linux-software/altera_soc_images)

  <div style="page-break-after: always;"></div>

### Creating Partitions
[Back to TOC](#Table-of-Contents)

1. Launch gparted.
2. Select the Gparted menu item and move the mouse to Devices, then click on the device that corresponds to your sdcard.
3. If anything exists on the sdcard, umount the partitions (right click each one and select unmount) and then delete them.
4. Right click on the unallocated file system.
    - Move the mouse to new and click new.
    - Set the 'Free Space Preceding' field to 4 MiB.
    - Set the 'New size' field to 512 MiB
    - Set the 'File System' field to fat32.
    - Set the 'Label' field to BOOTFS
    - Click the Add button.
5. Right click on the unallocated file system after the BOOTFS partition.
    - Set the 'Label' field to ROOTFS
    - The other defaults should be fine (ext4, primary partition).
    - Click the Add button.
6. Right click on the unallocated file system before the BOOTFS partition.
    - Set the 'Free Space Preceding' field to 2 MiB.
    - Set the 'New size' field to 1 MiB.
    - Set the 'File System' field to cleared.
    - Click the Add button.
7. Click the 'Apply All Operations' button, this is the check symbol above the file system viewer.
8. Confirm by clicking the Apply button.
9. Once completed, click Close on the 'Applying Pending operations' window.
10. Exit gparted.
11. Open a terminal.
12. In the terminal run fdisk to alter the new sdcard.
    - sudo fdisk /dev/sdx (x is your drive letter, also the name maybe something else mmcx)
13. In fdisk, list the partitions (press enter after each letter command).
    - p
14. Now change the 3rd partition type to unknown (press enter after each letter command).
    - t
    - 3
    - a2
15. In fdisk, list the partitions. The third should now be unknown (press enter after each letter command).
    - p
16. Write the changes to the card and exit (press enter after each letter command).
    - w
17. Leave the SDCARD inserted for copying data.

  <div style="page-break-after: always;"></div>

### Copy Data to the SDCARD
[Back to TOC](#Table-of-Contents)

0. Mount the directories, in Ubuntu this may involve reinserting the card after formatting.
1. Copy the bootfs directory files to the bootfs partition on the sdcard.
    - cp -pr /your/path/to/bootfs/* /path/to/sdcard/bootfs/
2. Copy the rootfs directory files to the rootfs partition on the sdcard.
    - sudo cp -pr /your/path/to/rootfs/* /path/to/sdcard/rootfs/
    - sudo sync
3. Copy the preloader to the unformatted partition.
    - sudo dd if=/your/path/to/bootfs/u-boot-spl-dtb.bin of=/dev/sdx3 (x is your drive letter, also the name maybe something else such as mmcx)
    - sudo sync
4. Insert the sdcard in the hanpilot.
    - All switches and jumpers should be set for sdcard boot.
8. Connect a console to the serial terminal.
    - Console settings: 115200 8n1, hardware flow control: NO, software flow control: NO
    - minicom -D /dev/ttyACM0 115200
    - If you can't enter input, check if hardware flow control is set to yes.
9. Power up the hanpilot, you should see boot messages in the terminal window.
