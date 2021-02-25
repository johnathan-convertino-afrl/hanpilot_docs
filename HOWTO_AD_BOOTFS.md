# HOW TO... Create a boot file system for Analog Devices HDL
## For: Terasic Hanpilot Arria 10 ARMHF
### Author: John Convertino

This howto will explain the steps needed to put a boot file system together for the Terasic Hanpilot development board.

Results:

* itb           (Created from the HDL+uboot)
* uBoot         (Second Stage boot loader)
* Kernel        (Linux Kernel)
* DTB           (Device Tree Binary)
* extlinux.conf (uBoot config)

Requirements:

* Ubuntu 20.04 (recommended, can be done in other Linux based OS).
* Quartus Prime Standard 19.4.0
* arm-linux-gnueabihf-
* make
* dtc

  <div style="page-break-after: always;"></div>

### Document Version
* v2.0 - 02/22/21 - Updated Quartus Prime Standard 19.4.0

#### Document History
* ~~v1.2~~ - 04/20/20 - Updated for hanpilot.
* ~~v1.1~~ - 01/18/20 - Updated markdown formatting.
* ~~v1.0~~ - 12/01/18 - Tested working version of the document.
* ~~v0.XX~~ Untested document version.

  <div style="page-break-after: always;"></div>

## Table of Contents
1. [Creating Directories](#Creating-Directories)
2. [Creating Analog Devices HDL](#Creating-Analog-Devices-HDL)
3. [Build u-boot](#Build-uboot)
4. [Build Analog Devices Linux Kernel](#Build-Analog-Devices-Linux-Kernel)
5. [Build Analog Devices Device Tree](#Build-Analog-Devices-Device-Tree)
6. [Create extlinux.conf for SDCARD(NEW)](#Create-extlinux.conf-for-SDCARD)

### Sources
* [Analog Devices HDL Github repo](https://github.com/analogdevicesinc/hdl.git "Analog Devices HDL")
* [Analog Devices Linux Kernel Github repo](https://github.com/analogdevicesinc/linux "Analog Devices Linux Kernel")
* [Altera Uboot Github repo](https://github.com/altera-opensource/u-boot-socfpga/tree/v2018.11 "Altera uboot")

### References
* [Analog Devices, Building HDL](https://wiki.analog.com/resources/fpga/docs/build)
* [Analog Devices, Quick Start for FMCDAQ2](https://wiki.analog.com/resources/eval/user-guides/ad-fmcdaq2-ebz/quickstart/a10soc)
* [Rocket Boards Golden System Reference Design User Manuals](https://rocketboards.org/foswiki/Documentation/GSRD)
* [Intel Reference for generating uboot](https://www.intel.com/content/www/us/en/programmable/documentation/lro1402536290550/lro1436891680025/lro1436891722016/lro1436891724278.html)
* [Intel mkpimage reference](https://www.intel.com/content/www/us/en/programmable/documentation/lro1402536290550/lro1436891680025/lro1436891703860.html)
* [Rocketboards preloader/uboot reference](https://rocketboards.org/foswiki/Documentation/PreloaderUbootCustomization131)
* [Rocketboards preloader/uboot build for a10soc](https://rocketboards.org/foswiki/Documentation/A10GSRDGeneratingUBootAndUBootDeviceTree)
* [Rocketboards preloader/uboot build updated for non-embedded build](https://rocketboards.org/foswiki/Documentation/BuildingBootloader#Arria_10_SoC_45_Boot_from_SD_Card)

  <div style="page-break-after: always;"></div>

### Creating Directories
[Back to TOC](#Table-of-Contents)

* Create three folders.
    - bootfs
    - rootfs
    - git
* Create them in the same directory, remember the paths for later usage.

  <div style="page-break-after: always;"></div>

### Creating Analog Devices HDL
[Back to TOC](#Table-of-Contents)

1. Clone the Analog Devices HDL repo to your git folder.
    - git clone https://github.com/johnathan-convertino-afrl/hdl.git
2. Enter the HDL repo
    - cd hdl
3. Checkout the HDL repo that matches your version of vivado.
    - git checkout master
4. Build the project needed using make.
    - make adrv9371x.hanpilot
5. Wait for make to finish executing the build.
6. Navigate to projects/adrv9371x/hanpilot/output_files
    - cd projects/adrv9371x/hanpilot/output_files
7. Run the quartus convert programming command
    - quartus_cpf -c --hps -o bitstream_compression=on adrv9371x_hanpilot.sof hanpilot.rbf
8. Copy the rbf files to the uboot directory.
    - cp hanpilot.*.rbf /your/path/of/git/u-boot-socfpga
10. Move to uboot folder(see Build uboot for obtaining the source).
  - cd /your/path/of/git/u-boot-socfpga
11. Create itb file for uboot to use for fpga programming.
  - mkimage -E -f board/terasic/arria10-hanpilot/fit_spl_fpga.its fit_spl_fpga.itb
12. Copy to bootfs folder.
  cp fit_spl_fpga.itb /path/to/your/bootfs

  <div style="page-break-after: always;"></div>

### Build uboot
[Back to TOC](#Table-of-Contents)

1. Enter the git folder.
    - cd path/to/your/folder/git
2. Clone uboot-altera
    - git clone https://github.com/johnathan-convertino-afrl/u-boot-socfpga.git
3. Enter the u-boot-socfpga folder.
    - cd u-boot-socfpga
3. Checkout the socfpga_v2020.07 branch
    - git checkout socfpga_v2020.07
4. Set your cross compiler
    - export CROSS_COMPILE=arm-linux-gnueabihf-
5. Set you architecture
    - export ARCH=arm
6. Configure uboot with make
    - make socfpga_arria10_hanpilot_defconfig
7. Convert hps.xml file to include file used by the device tree.
    - ./arch/arm/mach-socfpga/qts-filter-a10.sh /path/to/ad_hdl/project/hanpilot/hps_isw_handoff/hps.xml arch/arm/dts/socfpga_arria10_hanpilot_sdmmc_handoff.h
7. Make the project
    - make -j$(nproc)
8. Copy the needed files to the bootfs folder.
    - cp u-boot.img /path/to/your/bootfs
    - cp spl/u-boot-splx4.sfp /path/to/your/bootfs

  <div style="page-break-after: always;"></div>

### Build Analog Devices Linux Kernel
[Back to TOC](#Table-of-Contents)

1. Clone the Analog Devices Linux Kernel repo to your git folder.
    - git clone https://github.com/johnathan-convertino-afrl/linux
2. Enter the Linux Kernel Repo
    - cd linux
3. Checkout the branch that matches your HDL version.
    - git checkout master
4. Set your cross compiler
    - export CROSS_COMPILE=arm-linux-gnueabihf-
5. Set you architecture
    - export ARCH=arm
6. Setup the configuration.
    - make socfpga_adi_hanpilot_defconfig
7. Build the kernel.
    - make -j$(nproc) zImage
8. Once completed, copy the kernel to your bootfs folder.
    - cp arch/arm/boot/zImage /path/to/your/bootfs/

  <div style="page-break-after: always;"></div>

### Build Analog Devices Device Tree
[Back to TOC](#Table-of-Contents)

1. If you haven't built the kernel yet, follow the directions above in Build Linux Kernel.
2. Generate the device tree binary.
    - make socfpga_arria10_hanpilot_adrv9371.dtb
3. Copy and rename the device tree binary to the bootfs folder.
    - cp arch/arm/boot/dts/socfpga_arria10_hanpilot_adrv9371.dtb /path/to/your/bootfs/devicetree.dtb

  <div style="page-break-after: always;"></div>

### Create extlinux.conf for SDCARD
[Back to TOC](#Table-of-Contents)

1. Navigate to the bootfs folder.
  - cd /path/to/your/bootfs
2. Make a extlinux directory
  - mkdir extlinux
3. Enter the directory
  - cd extlinux
4. Create a extlinux.conf file with the following contents.

```
LABEL Arria10 HANPILOT SDMMC
    KERNEL ../zImage
    FDT ../devicetree.dtb
    APPEND root=/dev/mmcblk0p2 rw rootwait earlyprintk console=ttyS0,115200n8
```
