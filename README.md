# archsetup
Description of all steps during Archlinux installation 

## Increasing font in virtual console
Since I have fhd screen all font are tiny so I had to make it bigger to read:
`/usr/share/kbd/consolefonts` contain all the available fonts. I choose `setfont iso02-12x22.psfu.gz` since it's the biggest one.

## Wifi
Run `wifi-menu`. 
We are using [netctl](https://wiki.archlinux.org/index.php/Netctl) here. It is a CLI and profile-based network manager and an Arch project. 
Verify by `ping google.com`

## Update the system clock
This is important step because clock setup will be referenced in the future during our progress. 
`timedatectl set-timezone <your_timezone_country/your_timezone_city>`

[`timedatectl`](https://jlk.fjfi.cvut.cz/arch/manpages/man/timedatectl.1) may be used to query and change the system clock and its settings, and enable or disable time synchronization services.

Validate by `timedatectl status` 


## Partitioning and formatting 
The process of dividing a disk into logical areas that can be worked with separately is called partitioning. Disk partitioning is done to subdivide the disk into pieces with broadly different purposes. 

In our case we need two partitions on our disk:

#### 1. EFI System
The EFI system partition (ESP) is a partition on a data storage device (usually a hard disk drive or solid-state drive) that is used by computers adhering to the Unified Extensible Firmware Interface (UEFI).

What that means is that the EFI Partition is an interface for the computer to boot linux off of. its like a step taken    before it runs the `Linux filesystem` partition. its really small but basically without that partition your computer wouldn't know how to boot linux.

#### 2. Linux filesystem
This is where our linux will be installed.

All the steps are perfectly described [here](https://wiki.archlinux.org/index.php/Installation_guide#Partition_the_disks). The only thing you can omit the creation of swap partition. Swap can be created as a file after system is installed check [here](https://www.youtube.com/watch?v=llbL6wOcfoI).

All about formatting is [here](https://wiki.archlinux.org/index.php/Installation_guide#Format_the_partitions)    

## Mount the partitions
Mounting is the attaching of an additional filesystem to the currently accessible filesystem of a computer. A filesystem is a hierarchy of directories (also referred to as a directory tree) that is used to organize files on a computer or storage media (e.g., a CDROM or floppy disk). More on subject [here](http://www.linfo.org/mounting.html) 

### We should do:
1) Mount our `Linux filesystem` by `mount /dev/<linux_partition_name> /mnt`. 
2) Create directory for `EFI System` -> `mkdir /mnt/efi`
3) Mount our `EFI System` -> `mount /dev/<efi_partition_name> /mnt/efi`. And that's how we make `EFI System` partition visible inside mounted linux partition (which will be our operable linux OS quite soon).  

## Installation 
We have to install all the main stuff to make linux work.
`pacstrap /mnt base linux linux-firmware netctl wpa_supplicant dhclient dialog ppp neovim redshift i3-wm scrot xorg-server xorg-xinit wget unzip`
(provide the links to each program description)

## Configuration
Linux is installed but not yet configured. Let's do so.

### Fstab
Generate an `fstab` file `genfstab -U /mnt >> /mnt/etc/fstab`

Here the `fstab` file is used to define how disk partitions, various other block devices, or remote filesystems should be mounted into the filesystem. It's needed because before we did it manually, and now we have to make it during the boot automatically.

### Chroot
`arch-chroot /mnt`

A chroot is an operation that changes the apparent root directory for the current running process and their children. A program that is run in such a modified environment cannot access files and commands outside that environmental directory tree. This modified environment is called a chroot jail. More on this [here](https://wiki.archlinux.org/index.php/Chroot) 

From now on we are inside our of newly created linux OS already to progress with our configuration. 

### Set the time zone

`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`

The `ln` command is a standard Unix command utility used to create a hard link or a symbolic link (symlink) to an existing file. 

A hard link allows multiple filenames to be associated with the same file since a hard link points to the inode of a given file, the data of which is stored on disk. 

A symbolic link refers to a symbolic path indicating the location of the source file. You can see it as a link to a path (itself refering to an inode). A symbolic link is less limited. It can refer to a directory and can cross file system boundaries. That's our case here.

`-f`, `--force` remove existing destination files.

#### Run hwclock to generate /etc/adjtime:

`hwclock --systohc`

`hwclock` is an administration tool for the time clocks. It can: display the Hardware Clock time; set the Hardware Clock to a specified time; set the Hardware Clock from the System Clock; set the System Clock from the Hardware Clock; compensate for Hardware Clock drift; correct the System Clock timescale; set the kernel's timezone, NTP timescale, and epoch (Alpha only); and predict future Hardware Clock values based on its drift rate.

This command assumes the hardware clock is set to UTC. You can check it by `hwclock` and see that the time is different from the actual time. In our case the "right" time is local time. More info is [here](https://wiki.archlinux.org/index.php/System_time#Time_standard) Let's make it "right".

`timedatectl set-local-rtc 1`

### Localization
Read more about localization [here](https://wiki.archlinux.org/index.php/Installation_guide#Localization)

### Network configuration
Read more [here](https://wiki.archlinux.org/index.php/Installation_guide#Network_configuration)

### Password
`passwd`

### Boot loader
Now `exit` and install `pacstrap /mnt grub efibootmgr`

Login again `arch-chroot /mnt`
and `grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB`

[Generate_the_main_configuration_file](https://wiki.archlinux.org/index.php/GRUB#Generate_the_main_configuration_file)

(explain here what it's and why)

`exit`, `reboot`
(explain here what it's and why)



