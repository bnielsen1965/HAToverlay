# Open APS Explorer HAT Raspbian OS

The Explorer HAT is used with the Raspberry Pi compugter and utilizes the Raspbian
operating system. This folder contains documentation and files needed
to setup the host Raspbian operating system on an SD card.


## Quick Start
1. Download Raspbian lite image file.
2. Unzip Raspbian lite image and copy image to SD card.
3. Configure boot partition settings.
4. Configure OS partition settings.



## Raspbian image to SD Card

The SD card needs to be class 10 or better and it is best to research tested
cards as the Raspberry Pi can be a bit picky even with cards that are labeled as
class 10.

### Download image file

Download the latest Raspbian lite image [zip file](https://www.raspberrypi.org/downloads/raspbian/).

The Raspbian lite image will provide the smallest bare bones installation on which
all the software dependencies will be installed.

NOTE: Testing is needed on Raspbian Stretch.


### Copy image to SD Card

Unzip the downloaded Raspbian lite image file and use the appropriate method to
copy the image to your SD card. The method of copying will depend on the operating
system you are using, i.e. Windows, OS/X, or linux.

The Raspberry Pi project has [SD card writing instructions](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)
for various operating systems.

I.E. On a linux system the dd command may be used and may look something like the
following...

> unzip ~/Downloads/2017-04-10-raspbian-jessie-lite.zip
> dd if=~/Downloads/2017-04-10-raspbian-jessie-lite.img of=/dev/mmcblk0 bs=4M


## SD card configuration

Before installing the SD card in the Raspberry Pi there are some configuration
changes that are required. These changes are used to configure the hardware and
the Raspbian operating system.

The SD card will have two partitions after the write, a small VFAT partition with
the boot files and an EXT4 partition with the Raspbian operating system. Changes
are required on both the VFAT boot partition and the EXT4 OS partition.


### Boot partition changes

Boot partition settings will be used to configure the USB serial console and the
i2c hardware. The boot partition is VFAT and can be mounted on most operating systems
but the editor used to change files must support Unix file format.

#### USB serial console & i2c device

Open the file cmdline.txt from the boot partition in a text editor that supports
Unix formatted text files. To enable the USB serial port in the linux kernel, edit
the line of text and append the following settings...

> modules-load=dwc2,g_serial

Next edit the file config.txt and add the dtoverlay and dtparam settings for the
USB serial console and i2c device...

> dtoverlay=dwc2
> dtparam=i2c1=on,i2c1_baudrate=400000


#### Optional Explorer HAT overlay

If the target Explorer HAT hardware does not include a HAT EEPROM that defines
the GPIO hardware settings you can include an overlay file designed for use with
the Explorer HAT when no programmed EEPROM is available.

Copy the openaps.dtbo file from the [raspbian/overlay/](raspbian/overlay/) folder
to the overlay directory in the boot partition.

Edit the config.txt file in the boot partition and add the dtoverlay setting for
the openaps overlay file.

> dtoverlay=openaps


### OS partition changes

Some changes are needed in the Raspbian operating system settings to support the
hardware configuration for the Explorer HAT. The file system on the SD card for
the operating system is an EXT4 partition.

There are two methods that can be used to make these changes. Mount the EXT4
partition on a host system and make the changes before installing the SD card
in the Raspberry Pi, or connect the Raspberry Pi to an HDMI monitor and USB
keyboard and make the changes after the first boot.

Either method presents challenges. Editing the settings prior to installing the
SD card in the Raspberry Pi will require a host operating system that can mount
an EXT4 file system and can create symbolic links in the file system. While using
the Raspberry Pi to make the changes after first boot will require adapters to
connect a monitor and keyboard to the mini ports on the Raspberry Pi Zero W.

#### Option 1, Make changes on mounted EXT4 file system

These instructions will vary depending on the host operating system, in this case
the assumption is that the EXT4 partition is mounted on a linux host system.

##### Systemd settings for serial console

a) Mount the EXT4 partition on the SD card.

b) Open a terminal and cd into the {mount path}/etc/systemd/system/getty.target.wants path of the mounted partition, i.e.
> cd /run/media/bnielsen/f2100b2f-ed84-4647-b5ae-089280112716/etc/systemd/system/getty.target.wants

c) As the super user create the symbolic link.
> sudo ln -s /lib/systemd/system/getty@.service gettyGS0.service


##### Enable i2c device nodes

On the mounted EXT4 file system create the file {mount path}/etc/modules-load.d/i2c.conf, i.e.

> sudo gedit /run/media/bnielsen/f2100b2f-ed84-4647-b5ae-089280112716/etc/modules-load.d/i2c.conf

And add the following line...

> i2c-dev



#### Option 2, Make changes after first boot

These instructions use the Raspberry Pi itself to make the necessary changes.
Connect an HDMI monitor and USB keyboard to the target Raspberry Pi install
the SD card and power up the Raspberry Pi.

##### Systemd settings for serial console

a) After the first boot, when the login prompt is presented, login with the default
Raspbian credentials. Username pi and password raspberry.

b) At the prompt cd into /etc/systemd/system/getty.target.wants path, i.e.
> cd /etc/systemd/system/getty.target.wants

c) As the super user create the symbolic link.
> sudo ln -s /lib/systemd/system/getty@.service gettyGS0.service


##### Enable i2c device nodes

a) Create the file /etc/modules-load.d/i2c.conf, i.e.

> sudo nano /etc/modules-load.d/i2c.conf

b) And add the following line...

> i2c-dev


## First Boot

If you have not completed a first boot then install the SD card in the Raspberry
Pi and connect the power source to start the first boot.

Use a USB cable to connect your development workstation or laptop to the micro
USB port on the Raspberry Pi. And use your favored serial terminal software, i.e.
putty or screen, to connect to the serial console.

Verify that the serial console is working and you can login to the Raspberry Pi.


## Post boot
