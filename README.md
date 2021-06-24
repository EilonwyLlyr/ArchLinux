# ArchLinux

Simplified steps to install Arch Linux on any computer.

### Bare Necessities 

- Your motherboard or vm must be able to boot into uefi mood.
- iso file for Arch Linux. Can be found on their website
- Flashdrive if installing on hardware

## Setup
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
14. `mkfs.fat -F32 [partiton#1]` This will make the first partion create, which was EFI type, use a fat32 file allocation table
15. `mkfs.ext4 [partition#2]` 




### Wifi setup
1. `iwctl`
2. `wsc list` you will get a list of devices `[device]`.
3. `wsc [device] push-button` will turn on wifi device.
4. `station [device] scan` will populate a list of networks nearby.
5. `station [device] get-networks` lists all of the near by networks `[network]`.
6. `station [device] connect [network]` this will try to connect to the network, if it is password protected you will need to input that password next.
7. `station [device] show` verify if you are connect to the network.
8. `exit`
