
# HOW TO... Install and Setup Ubuntu 18.04 on a Virtual Machine for ARM development
## For: Xilinx arm64, Xilinx arm, Intel arm64, Intel arm.

This how-to will explain the steps needed setup Ubuntu 18.04 using VirutualBox.
This setup will be able to build different ARM 32bit or 64bit systems for SoC based designs. 
See ROOTFS, BOOTFS, and SDCARD documents for the exact build steps for your target platform.

Results:

* Ubuntu 18.04 installed in VirtualBox
* Arm tools installed in the virtualized Ubuntu System.

In this guide we will use the GUI whenever possible. It is possible to do everything via the command line.

Requirements:

  * VirtualBox 5.X and above
  * Any host OS will be fine.
  
  <div style="page-break-after: always;"></div>

### Document Version
* v1.2 - 01/18/20 - Updated markdown formatting, added packages for quartus.

#### Document History
* ~~v1.1~~ - 12/14/18 - Added openssl dev library to the packages list for apt-get.
* ~~v1.0~~ - 12/01/18 - ARM and ARM64 Instructions for Ubuntu
* ~~v0.XX~~ Untested document version.

  <div style="page-break-after: always;"></div>

### Table of Contents
1. [Install Ubuntu On VirtualBox](#Install-Ubuntu-On-VirtualBox)
2. [Adjust Ubuntu VirtualBox Settings](#Adjust-Ubuntu-VirtualBox-Settings)
3. [Install Guest Additions](#Install-Guest-Additions)
4. [Install Updates In Ubuntu](#Install-Updates-In-Ubuntu)
5. [Install Compiler Tools In Ubuntu](#Install-Compiler-Tools-In-Ubuntu)
6. [Install Vivado In Ubuntu](#Install-Vivado-In-Ubuntu)
7. [Install Quartus In Ubuntu](#Install-Quartus-In-Ubuntu)

  <div style="page-break-after: always;"></div>

### Install Ubuntu On VirtualBox
[Back to TOC](#Table-of-Contents)

1. Download Ubuntu 18.04 64bit desktop ISO.
    - [Ubuntu 18.04 64bit Desktop](https://www.ubuntu.com/download/desktop)
2. After the download is finished. Install VirtualBox 5.X
    - [How To Install Virutal Box](https://www.virtualbox.org/manual/ch02.html)
3. With Ubuntu ISO downloaded and VirtualBox installed, launch VirtualBox.
4. With the VirtualBox Manager open, click on the top left tab named 'Machine'.
5. Move the mouse cursor to the item labeled 'New', and click on it.
6. A window will pop up, called 'Create Virtual Machine'.
7. In the name field type in "Ubuntu". The type and version should change automatically to Ubuntu 64bit.
    - If not change type to "Linux", and Version to Ubuntu 64bit.
8. Click the Next button.
9. The Memory Size selection window is now displayed. 
    - Select at least 16 Gigs. 
    - Click the Next button when done.
10. The Hard Disk selection window is now displayed. 
    - Select the 'Create a virtual hard disk now' radio button. 
    - Click the Create button when done.
11. The Hard Disk file type selection window is now displayed. 
    - Select the 'VDI (VirtualBox Disk Image)' radio button. 
    - Click the Next button when done.
12. The Storage on physical hard disk selection window is now displayed.
    - Select the 'Dynamically allocated' radio button.
    - Click the Next button when done.
13. The File location and size selection window is now displayed.
    - The folder location field should be left at its default.
    - In the hard drive field size, select at least 120 gigs (Vivado/Quartus require large amounts of space).
    - Click the Create button when done.
14. The previous window will close, and you will be brought back to the 'VirtualBox Manager' main window.
15. In your machine list there will be a new machine named "Ubuntu".
    - Double click on the "Ubuntu" machine icon to launch it.
16. Two windows will pop up. Once that is the Ubuntu Virutal Machine screen, and the other which is the 'Select start-up disk window'.
    - In the 'Select start-up disk' window click the folder icon next to the drop down menu (most likley the name of your CD-ROM drive).
    - A folder browser will pop up, navigate to your Ubuntu 18.04 64bit ISO download.
    - Click on the ISO file. It will automatically select it and close the window.
    - Click the Start button when done.
17. The Ubuntu ISO will now boot in the Ubuntu Virutal Machine window.
18. After the system boots, the 'Install' window will appear.
    - Click the Install Ubuntu button.
    - The Keyboard Layout selection will appear, keep this at its defaults and click the continue button.
    - The Updates and other software will appear, keep this at its defaults and click the continue button.
    - The Installation type will appear, keep this at its defaults and click the install now button.
    - The Write the changes to disks window will appear. Read it over and click the continue button.
    - The Where are you selection will appear, keep it at its default if it is correct, and click the continue button.
    - The Who are you form will appear, enter the needed data into the fields and click the continue button when done.
19. The system will now install. Wait for it to finish.
20. When the installation finishes, a 'Installation Complete' window will appear, click the Restart Now button.
21. When prompted, press the ENTER key to remove the virtual cd medium (ISO).
22. The system will now reboot.Shutdown the system.
23. The login screen should appear with your name, select it, enter your password and click the Sign In button.
24. The desktop and any annoying intro screens will appear.
25. Base system installation is now complete.

  <div style="page-break-after: always;"></div>

### Adjust Ubuntu VirtualBox Settings
[Back to TOC](#Table-of-Contents)

1. Startup VirtualBox.
2. Right click the Ubuntu System in the machine listings.
3. Move the mouse cursor to settings and click it.
4. Click the System Icon in the left side panel.
    - Click the Processor Tab to the right.
    - Change the Processor(s) from 1 to the greatest amount it allows.
7. Click the Display Icon in the left side panel.
    - Change the Video Memory amount to a reasonable amount.
9. Click the USB Icon in the left side panel.
    - Make sure the Enable USB Controller is checked.
    - Select USB 3.0 if it is available. Else select USB 2.0.
12. Click the Shared Folders Icon in the left side panel.
13. Click the small blue folder with a green arrow to the right.
14. The 'Add Share' window will now appear.
    - Click the folder path drop down and select other.
    - A file browser will appear.
    - Find the root of your home directory (/home/yourUserName)
    - Make sure the Directory field is /home/yourUserName
    - Enable the 'Auto-Mount' option.
    - Click the Choose button.
15. Shared folders should now show the added folder.

  <div style="page-break-after: always;"></div>

### Install Guest Additions
[Back to TOC](#Table-of-Contents)

1. Startup your Ubuntu VirtualBox system (double click the Ubuntu System Icon in the machine listing).
2. Once you have logged in to the system and loaded the desktop, click the devices tab.
3. Move the mouse cursor to 'Insert Guest Additions CD image' and click on it.
4. A new window will appear, asking you to download the VirtualBox Guest Additions. Click the Download button.
    - It will now prompt you to make sure you want to do the download. Click the Download button again.
    - Wait for the download to finish
5. When the download finishes, a new window will appear asking you to insert the VirtualBox Guest Additions. Click the Insert button.
6. Ubuntu will prompt you about autorunning "VBox_GAs_5.X.XX". Click the Run button.
    - It will now prompt you for your password for super user access. Enter it and click the Authenticate button.
7. Guest Additions will now install in a newly created terminal window.
    - When finished it will prompt you press return to close, do so when it asks.
8. Open a terminal.
    - Run the following command: sudo usermod -a -G vboxsf $USER
    - Close a terminal.
8. Restart the system.
9. Guest Additions are now installed.

  <div style="page-break-after: always;"></div>
 
### Install Updates In Ubuntu
[Back to TOC](#Table-of-Contents)

1. Startup your Ubuntu VirtualBox system (double click the Ubuntu System Icon in the machine listing).
2. Once you have logged in to the system and loaded the desktop, open a terminal.
    - Run the following command: sudo apt-get update
    - Next Run: sudo apt-get dist-upgrade
    - Press Enter when apt-get asks "Do you want to continue?" (default is yes).
    - Apt-Get will now update the system.
3. Restart the system.
4. Ubuntu is now updated.

  <div style="page-break-after: always;"></div>
 
### Install Compiler Tools In Ubuntu
[Back to TOC](#Table-of-Contents)

1. Startup your Ubuntu VirtualBox system (double click the Ubuntu System Icon in the machine listing).
2. Once you have logged in to the system and loaded the desktop, open a terminal.
    - Run the following command: sudo apt-get install 'package name'
    - Install all of the packages listed below in place of 'package name' above.
    
    ```
      build-essential 
      cmake 
      binfmt-support 
      qemu
      qemu-user-static 
      debootstrap 
      libc6-armel-cross 
      libc6-dev-armel-cross 
      binutils-arm-linux-gnueabi
      libncurses5-dev 
      device-tree-compiler
      crossbuild-essential-armhf
      crossbuild-essential-armel
      crossbuild-essential-arm64
      crossbuild-essential-powerpc
      u-boot-tools 
      git 
      bison 
      flex 
      gparted 
      libc6:i386
      libncurses5:i386
      libxtst6:i386
      libxft2:i386
      libc6-dev-i386
      libxft2
      lib32z1
      lib32ncurses5
      lib32bz2-1.0
      libpng12
      libqt5xml5
      liblzma-dev
      libstdc++6 
      libssl1.0-dev 
      libstdc++6:i386 
      libgtk2.0-0:i386 
      dpkg-dev:i386
      minicom
      net-tools
    ```
    - Press Enter when apt-get asks "Do you want to continue?" (default is yes).
    - apt-get will now install the packages.
3. Ubuntu now has the proper packages installed.
4. Create a link from make to gmake for the Xilinx SDK.
    - sudo ln -s /usr/bin/make /usr/bin/gmake

  <div style="page-break-after: always;"></div>

### Install Vivado In Ubuntu
[Back to TOC](#Table-of-Contents)

1. Download a version of Vivado that matches your project. The version for the Analog Devices HDL 2018 R1 branch is 2017.4.1.
2. Startup your Ubuntu VirtualBox system (double click the Ubuntu System Icon in the machine listing).
3. Once you have logged in to the system and loaded the desktop, open a terminal.
    - Navigate to your download of Vivado.
    - [Install per the instructions from Xilinx (UG973)](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2017_4/ug973-vivado-release-notes-install-license.pdf)
4. Once the install is completed, add a path to your .bashrc.
    - nano ~/.bashrc
    - Page down to the end of the file.
    - Add the following:
        - export PATH=$PATH:/opt/Xilinx/Vivado/2017.4/bin
    - Press ctrl+x to exit.
    - Press Y for yes, to save file.
    - Press enter to confirm file to write to.
5. Restart the system.
6. Vivado is now installed and ready to use from Ubuntu.
7. See your system admin for license configuration.

  <div style="page-break-after: always;"></div>

### Install Quartus In Ubuntu
[Back to TOC](#Table-of-Contents)

1. Download a version of Quartus that matches your project. The version for the Analog Devices HDL 2018 R1 branch is 17.1.1.X Prime Standard.
2. Startup your Ubuntu VirtualBox system (double click the Ubuntu System Icon in the machine listing).
3. For versions lower then 19, install [libpng12.so from here.](https://packages.ubuntu.com/xenial/amd64/libpng12-0/download)
3. Once you have logged in to the system and loaded the desktop, open a terminal.
    - Navigate to your download of Quartus.
    - Also make sure to download the Intel SoC Embedded Development tool that matches your version.
    - DO NOT install in your home directory, change /home/yourUserName to /opt .
    - [Install per the instructions from Intel](https://www.intel.com/content/dam/www/programmable/us/en/pdfs/literature/manual/quartus_install.pdf)
4. Once the install is completed, add various information to your .bashrc.
    - nano ~/.bashrc
    - Page down to the end of the file.
    - Add the following:
    
    ```
      export PATH=$PATH:/opt/intel/std/18.1/quartus/bin
      #LOCALE
      export LANGUAGE=en_US.UTF-8
      export LANG=en_US.UTF-8
      export LC_ALL=en_US.UTF-8
      export LC_CTYPE=en_US.UTF-8
      #quartus info
      export INTELFPGAOCLSDKROOT="/opt/intel/std/18.1/hld"
      export QSYS_ROOTDIR="/opt/intel/std/18.1/quartus/sopc_builder/bin"
      export QUARTUS_64BIT=1
      export QUARTUS_ROOTDIR_OVERRIDE=/opt/intel/std/18.1/quartus
      export QUARTUS_ROOTDIR=/opt/intel/std/18.1/quartus
    ```
    - Press ctrl+x to exit.
    - Press Y for yes, to save file.
    - Press enter to confirm file to write to.
5. Change the permissions of the hidden .altera.quartus folder in your home directory.
      - sudo chown -hR $USER:$USER ~/.alrera.quartus
6. Restart the system.
7. Quartus is now installed and ready to use from Ubuntu.
8. See your system admin for license configuration.

