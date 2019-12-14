# Venom Linux Installation Guide
A comprehensive installation guide for Venom Linux

### Minimum System Requirements
- x86_64 CPU
- 4 GB RAM (System Memory)
- 100 GB of Disk Space
- CD/DVD drive or a USB port for the installer media

### Getting Started
Venom Linux provides a few live installer images that contain a installer.
These live images come with a full desktop enviornement and basic applications configured for that environment.
MATE, Xfce4, LXDE and LXQT are available in addition to these flavours Venom Linux provides a Xorg-only iso and a base image.
The base image provides a minimal set of packages to install and setup a Venom Linux system.

All the installation images can be downloaded from the [Venom Linux Website](http://venomlinux.org/download.html)
It is advised to verify the mdsum of the downloaded iso file.
If the mdsum matches you can move forward and either burn the iso image on a CD/DVD or create a bootable USB drive.

### Creating a Bootable USB drive on Linux
Identify you USB drive with *fdisk* by opening your favorite terminal emulator and typing:

`sudo fdisk -l`

The output of this will show the USB device as /dev/sdb or /dev/sdc, we will refer to /dev/sdx in this guide where x is the appropriote letter from the output of the fdisk command.
Next you need to make sure that your USB drive is not mounted by unmounting it with

`sudo umount /dev/sdx`

Now you are ready to write the downloaded iso to your USB drive, we will use the *dd* command for this.

**Warning: use the dd command with caution as it will overwrite any data on the USB drive.**

`sudo dd if=/home/username/Downloads/venom-xfce4-20191002.iso of=/dev/sdx bs=4M && sync`
Change the command to let dd find the iso in the correct directory and change the image name and device name of this example to the correct image name and device name. Note sdx, do **not** use partitions i.e. sdb1, sdc1, etc.
The *sync* command at the end will flush all data.
dd will not print any output until the process is completed (or if it failed). 
Depending on your device, this process can take a few minutes.

### Burning on a CD or DVD

Any modern disk burning application should be able to write the iso file to a CD or DVD.
A few suggestions are:
- Brasero
- K3B
- Xfburn
- Infrarecorder (Windows)

In general live sessions will be less responsive on a CD or DVD than with a USB or hard drive.

### Booting up

Boot your machine using the previously-created installation medium. This can be done by starting the computer and press ESC, F1, F2, F8 or F10 during the initial startup screen depending on the BIOS manufacturer. A menu may appear giving you the option to give a CD/DVD or USB drive boot sequence priority over the hard drive, move it to the first position in the list. You can choose to run the live image from the media, or, if you have the resources available, you can load the contents of the image into RAM. This option takes some time at the beginning but provides a quicker installation procedure.

Once the live image has booted you need to check if you have a working internet connection.
Venom Linux does not have a graphical installar to start the installer you open the a terminal, the default terminal that comes with the Destop Enviornement is fine. To start the installer run the command `sudo venom-installer` when prompted for a password type in *root* and hit enter to start the installer.

### Installing Venom Linux

When the installer starts you are greeted by a curses menu 
