# ArchLinux

Simplified steps to install Arch Linux on any computer
`planning to make a script later on`

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

### Wifi setup
1. `iwctl`
2. `wsc list` you will get a list of devices [device]
3. `wsc [device] push-button`
