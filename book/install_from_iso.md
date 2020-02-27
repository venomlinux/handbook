# Venom Linux Installation Guide
A comprehensive installation guide for Venom Linux.

### Minimum System Requirements
- x86_64 CPU
- 4 GB RAM (System Memory)
- 100 GB of Disk Space
- CD/DVD drive or a USB port for the installation media

### Getting Started
Venom Linux provides a few installer images.
These installer images come with a full desktop environment and basic applications configured for that environment.
MATE, XFCE4, LXDE and LXqt flavours are available. We also provide installers and base images with only Xorg.
The base image provides a minimal set of packages to install and configure a Venom Linux system.

All the installation images can be downloaded from the [Venom Linux Website](http://venomlinux.org/download.html)
It is advised to verify the md5sum of the downloaded iso file.
If the md5sum matches you can move forward and either burn the iso image to a CD/DVD, or create a bootable USB drive.

### Creating a Bootable USB drive on Linux
Identify your USB device with *fdisk*:

`sudo fdisk -l`

The output of this will show the USB device as /dev/sdb or /dev/sdc, we will refer to /dev/sdX in this guide where `X` is the appropriate letter from the output of `fdisk`.
Next, you need to make sure that your USB drive is not mounted. Use this command to unmount the device:

`sudo umount /dev/sdx`

You can now proceed to write the iso to your USB drive. We reccomend using the dd command.

**Warning: Ensure that the device you are writing to is the USD device. `dd` wipes all data on a drive.**

`sudo dd if=/home/username/Downloads/venom-xfce4-20191002.iso of=/dev/sdX bs=4M && sync`
The `if=` should be replaced with the path to your iso, and `of=` with the appropriate device. Make sure to not write to a partition(/dev/sdX1).
the `sync` at the end will ensure that the data is written to the USB drive.
dd will not print any output until the process is completed (or if it failed). 
Depending on your device, this process can take a few minutes.

### Burning on a CD or DVD

Any modern disk burning application should be able to write the iso file to a CD or DVD.
A few suggestions are:
- Brasero
- K3B
- Xfburn
- Infrarecorder (Windows)

In general, live sessions will be less responsive on a CD or DVD than with a USB or hard drive.

### Booting up

Boot your machine using the device you just created. This can be done by starting the computer and pressing `ESC`, `F1`, `F2`, `F8` or `F10` during the boot sequence. A menu may appear giving you the option to give a CD/DVD or USB drive boot sequence priority over the hard drive, move it to the first position in the list. You can choose to run the live image from the media, or, if you have the resources available, you can load the contents of the image into RAM. This option takes some time at the beginning but provides a quicker installation procedure.

Once the live image has booted you need to check if you have a working internet connection.
Venom Linux does not have a graphical installer. To use the installer, the terminal that comes with your DE is fine. To use the installer, run `sudo venom-installer` when prompted for a password type in *root* and hit enter to start the installer.

### Installing Venom Linux

When the installer starts you are greeted by a curses menu.
