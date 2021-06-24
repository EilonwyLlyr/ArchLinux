# ArchLinux

Simplified steps to install Arch Linux on any computer with a grub uefi bootloader and plasma for the graphics/window manger.

### Bare Necessities 

- Either an Intel or AMD processor
- Your motherboard or vm must be able to boot into uefi mood.
- iso file for Arch Linux. Can be found on their website
- Flashdrive if installing on hardware

## Arch Base install

1. Verify if uefi boot. 
`ls /sys/firmware/efi/efivars`
A list of files shoudl show. If you get something along the lines of directory does not exist then your boot was not uefi.
2. Check if you have internet access by pining.
`ping google.com -c 1`
If you get a packaged returned you should be fine. If not you will need to either connect an ethernet cable or setup the wifi. [Wifi setup](#wifi-setup)
3. `timedatectl set-ntp true` configures you hardware clock
4. `fdisk -l | less` lists all of the current harddrives on your system. Some examples of how they will look: /dev/sda, /dev/sdb, /dev/nvme0n1. From these list of disks you will select one where you will install arch. `[disk]`
5. `fdisk [disk]` will slect the disk you will format
6. `g` creates an empty GPT partition
7. `n` create a new partition
`[enter]` size of the sectors, just press enter for default
`[enter]` starting sector, just press enter for default
`+100M` the size of the partition, this will the our efi partion so we can boot the system
`t` to change the type of the partition
`1` change the type to EFI type system
8. `n` create a new partition
`[enter]` size of sectors, just press enter for default
`[enter]` starting sector, just press enter for default
`[enter]` or `+##G` the size of the partion where we will install arch, you can either take the rest of the disk space with [enter] or specify an amount if you are going to dual boot with another system.
12. (optional windows dual boot)`n` create a new partion for dual boot
`[enter]` size of sectors, just press enter for default
`[enter]` starting sector, just press enter for default
`[enter]` take up the remaning space of the disk
`t` to change the tpe of the partiton
`11` for a microsoft file system
13. `p` take a note of the partitions that were created. Some examples of how they will look: /dev/sda1, /dev/sda2, /dev/sdb1, /dev/sdv2, /dev/nvme0n1p1 /dev/nvme0n1p2. The partions are numbered from 1 to n where n the number of partitons you created. [partiton#1]  [partiton#2]  (optional)[partiton#3]
14. `mkfs.fat -F32 [partiton#1]` This will make the first parttition, which was EFI type, use a fat32 file allocation table
15. `mkfs.ext4 [partition#2]` This will make the second partition, which was Linux File System, use the ext4 filesystem
16. `mount [partition#2] /mnt` will mount the second partition onto the /mnt directory of the iso running the installation.
17. `pacstrap /mnt base linux linux-firmware` will install the the base files, linux and the firmware into [partition#2] which is mounted on /mnt
18. `genfstab -U /mnt >> /mnt/etc/fstab` generates a fstab
19. `arch-chroot /mnt` login as root user into [partition#2]
20. `ls /usr/share/zoneinfo` lists all the regions [region]
21. `ls /usr/share/zoneinfo/[region]` lists cities [city]
22. `ln -sf /usr/share/zoneinfo/[region]/[city] /etc/localtime` links the localtime to your region and city
23. `hwclock --systohc` sets up the time 
24. `pacman -S vim` install vim for modifying files
25. `vim /etc/locale.gen` look for #en_US.UTF-8 UTF-8 and remove the # to uncomment it. You may also uncomment any other locales you wish.
(When using vim, press `i`, then edit what ever you want. Once done press `esc` then enter `:wq` to save the file and exit)
26. `locale-gen` generates the locales you uncommented
27. `vim /etc/hostname` this will be an empty file, this will be the name of the computer, you can name it anything, perferably all lowercase no spaces [hostname]
28. `vim /etc/hosts` this will setup the local network. At the bottom of the commented text enter:
`127.0.0.1  localhost
::1         localhost
127.0.1.1   [hostname].localdomain  [hostname]`
29. `passwd` this will setup the root password
30. `useradd -m [username]` creates a new user must be all lowercase no spaces [username]
31. `passwd [username]` this will setup the password of the new user
32. `usermod -aG wheel,audio,video,optical,storage [username]` will add the user to the groups
33. `pacman -S sudo` install sudo
34. `visudo` this will edit sudo permissions, look for # %wheel ALL=(ALL) ALL or # %wheel ALL=(ALL) NOPASSWD: ALL. Uncomment one of them to have sudo permisions for the user
35. `pacman -S grub` installs the grub, the manager for uefi for which system to boot
36. `pacman -S efibootmgr dosfstools os-prober mtools ntfs-3g` install tools that would be needed for grub
37. `mkdir /boot/EFI` create the directory where the efi partition will be at
38. `mount [partition#1] /boot/EFI` mounts the EFI type 
39. `grub-install --target=x86_64-efi --bootloader-id=arch --recheck` grub is installed
40. `grub-mkconfig -o /boot/grub/grub.cfg` creates the configuration for the partitions to load.
41. `pacman -S networkmanager` installs a network manger.
42. `systemctl enable NetworkManager` enables netoworkmanger as a deamon process that will startup whenever your computer starts
43. `exit` leaves the arch-chroot commant we entered earlier
44. `umount -l /mnt` unmounts partitions 2 from the iso /mnt directory
45. `shutdown 0` shutsdown the computer. once off you can remove the installation usb/iso

Arch linux has just been installed you can start up the computer and do anything you want within the command line.

(Optional Windows Boot)

1. `sudo su` Once you have logged in as a user do this command to have sudo privlages so you don't have to write sudo before all of the following commands
2. `mkntfs [partition#3]` this will install the ntfs file system that windows uses.
3. Once done you can install windows normally. iso or usb bootable.
4. When selecting a partition to install onto look for [partition#3] within the list of available drives where you can install windows.
5. Once installation is done boot into arch linux
6. `grub-mkconfig -o /boot/grub/grub.cfg` this will make it so that windows will show up on the uefi menu.
7. reboot and go into windows.
8. disable fast startup and disable hibernation

## Graphics/Windows Manger

1. `sudo su` Once you have logged in as a user do this command to have sudo privlages so you don't have to write sudo before all of the following commands
2. `pacman -S intel-ucode` or `pacman -S amd-ucode` installs the pakages needed for your cpu.
3. `pacman -S xorg-server` installs a display server


For more information on the following 3 steps and which dirvers to install go to [Xorg](https://wiki.archlinux.org/title/Xorg)
5. `pacman -S mesa` or `pacman -S nvidia` installs the driver depending on the GPU you are using. mesa for AMD/ATI or Intel nvidia for Nvidia GPU
6. `vim /etc/pacman.conf` if you are going to use/download 32-bit programs such as Steam look for and uncomment the following:
`[multilib]
Include=/etc/pacmad.d/mirrorlist`
7. `pacman -S lib32-mesa` or `pacman -S lib32-nvidia-utils` for the 32-bit packages for the GPU


8. `pacman -S plasma-meta kde-applications` installs the Windows Manager and kde applications (this can take a while)
9. `grub-mkconfig -o /boot/grub/grub.cfg` reapply your configuration
10. `systemctl enable sddm` makes it so that the windows manager graphics will always start on startup
11. `reboot`


### Wifi setup
1. `iwctl`
2. `wsc list` you will get a list of devices `[device]`.
3. `wsc [device] push-button` will turn on wifi device.
4. `station [device] scan` will populate a list of networks nearby.
5. `station [device] get-networks` lists all of the near by networks `[network]`.
6. `station [device] connect [network]` this will try to connect to the network, if it is password protected you will need to input that password next.
7. `station [device] show` verify if you are connect to the network.
8. `exit`
