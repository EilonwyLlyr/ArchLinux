# ArchLinux

Simplified steps to install Arch Linux on any computer. anything in the following `message` are commands you run on your computer

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
If you get a packaged returned you should be fine. If not you will need to either connect an ethernet cable or setup the wifi.
3. `timedatectl set-ntp true` configures you hardware clock

### Wifi setup
1. `iwctl`
2. `wsc list` you will get a list of devices [device].
3. `wsc [device] push-button` will turn on wifi device.
4. `station [device] scan` will populate a list of networks nearby.
5. `station [device] get-networks` lists all of the near by networks [network].
6. `station [device] connect [network]` this will try to connect to the network, if it is password protected you will need to input that password next.
7. `station [device] show` verify if you are connect to the network.
8. `exit`
