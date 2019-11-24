# archsetup
Description of all steps during Archlinux installation 

## Increasing font in virtual console
Since I have fhd screen all font are tiny so I had to make it bigger to read:
`/usr/share/kbd/consolefonts` contain all the available fonts. I choose `setfont iso02-12x22.psfu.gz` since it's the biggest one.

## Wifi
Run `wifi-menu`. 
We are using [netctl](https://wiki.archlinux.org/index.php/Netctl) here. It is a CLI and profile-based network manager and an Arch project. 
Verify by `ping google.com`

## Partitioning
The process of dividing a disk into logical areas that can be worked with separately is called partitioning. Disk partitioning is done to subdivide the disk into pieces with broadly different purposes. 

In our case we need two partitions on our disk:

#### 1. EFI System
The EFI system partition (ESP) is a partition on a data storage device (usually a hard disk drive or solid-state drive) that is used by computers adhering to the Unified Extensible Firmware Interface (UEFI).

What that means is that the EFI Partition is an interface for the computer to boot linux off of. its like a step taken    before it runs the `Linux filesystem` partition. its really small but basically without that partition your computer wouldn't know how to boot linux.

#### 2. Linux filesystem
This is where our linux will be installed.   
