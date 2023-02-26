# Hanpilot Documents
## AUTHOR: Jay Convertino

### LICENSE
```
License MIT

Copyright 2023 Jay Convertino

Permission is hereby granted, free of charge, to any person obtaining a copy of 
this software and associated documentation files (the “Software”), to deal in the 
Software without restriction, including without limitation the rights to use, 
copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the 
Software, and to permit persons to whom the Software is furnished to do so, 
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all 
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR 
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER 
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN 
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

## INFO
This collection of documents is under the MIT license. Use it how you wish.

### Purpose
All of the documents help use the HDL/LINUX/SOC_FPGA_UBOOT repos to build arria10 based analog devices targets.

### Targets

- adrv9371/5
- adrv9009
- fmcomms2/3

These work the the Hanpilot Arria 10 dev board and the SOCDK Arria 10 dev board.

### Requires

- Quartus Pro 20.4.X

### DOCS

- HOWTO_AD_BOOTFS.md : How to build all of the files needed for the boot file system partition.
- HOWTO_INSTALL_UBUNTU_VIRT.md : How to put together a virtual box image (not recommended, docker would be a better way in this day and age).
- HOWTO_CREATE_SDCARD.md : How to asseble all of the BOOTFS/ROOTFS files on the SDCARD.
- HOWTO_ROOTFS.md : How to build the filesystem needed for the root file system partition.

