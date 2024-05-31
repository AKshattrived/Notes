# Linux Kernel and Boot concepts

<br>

## BIOS and UEFI

### BIOS (Basic Input Output System)
- Master Boot Record (MBR) on the hard drive point the partitions and os, from where to boot.

### UEFI (Unified Extensible Firmware Interface)
- Replacement for BIOS 
- Rather than pointing boot sector, have a different partition on hard drive. 
##
<br>

## Bootloaders

### GRUB (Grand Unified Bootloader)
- Computer Transition from BIOS/UEFI to actual OS. 
- If `/boot/grub` has menu.lst and grub.conf, then GRUB legacy. (Look for menu.lst only to avoid confusion)
- Hard to configure.
- Boot menu usually displays on boot.

### GRUB2
- Next iteration of GRUB2.
- If `/boot/grub` has grub.cfg, then GRUB2. 
- Customizable in `etc/default/grub`. Easy to read.
- Hidden boot meny (Press Shift!)
- `sudo update-grub` to update the changes made in `/etc/default/grub`. This will change the boot code inside the `/boot/grub` folder.
##
<br>

## Boot Methods
- PXE, USB, CD, iPXE, ISO

### PXE (Preboot Execution Environment)
- If hard drive not found on hardware, queries the DHCP server.
- DHCP server responses with IP along with TFTP location of the boot file on a network.
### iPXE 
- Uses HTTP instead of TFTP to download the bootfile.
##
<br>

## Kernel Panic!

- Over clocked CPU, Bad RAM stick, Bad add on card. In short some faulty piece of hardware could be the common cause for the Kernel Panic.
- System upgrade can also cause the Kernel Panic. To solve this go to GRUB and pick the older version of kernel. System will boot up. Re-intall the kernel.
- Or you can boot up from USB or CD, access your hard drive, move your data to different system.
##
<br>

## Loading Kernel Modules on Boot
 - `/etc/modules` file contains list of the kernel modules to be loaded at boot time. 
 - `etc/modprobe.d/blacklist.conf` file contains list of the kernel modules blacklisted. These modules will not boot. To blacklist a module add `blacklist [modulename]` to the file.
 - Linux kernel is usually good at automatically detecting the hardware and loading the modules according to that.
##
<br>

## Manipulating Kernel Modules

- insmod
- modprobe
- depmod
- lsmod
- rmmod

### insmod
 - You need to provide full path of the kernel module you want to install.
 - Doesn't do any dependency checking. Just loads the module.
 - If kernel doesn't work or wrong kernel version or doesn't have the proper dependencies, it's just gonna fail with no explanation.

 ### modprobe
 - More advanced program
 - Just give the module name
 - It will look and determine all the dependency modules that given module needs.
 - To do all this, it requires dependency map. To do this there is `depmod`. Run `depmod` when done intalling new kernel module and it will recreate the dependency map so that `modprobe` knows where to look for all the dependencies.

 ### Then why do we even have `insmod`?
 - Behind the scenes `modprobe` uses `insmod` to install the module and all its dependency modules.
 - Humans use `modprobe` and `modprobe` uses `insmod`.

 ### Notes:  
 - `/lib/modules` contains all the modules.
 - `lsmod` will list all the installed kernel modules.
 - `rmmod` to remove kernel module. But if the module is dependency of some other module, it will give error. It will have the name of the module wich depends on the mentioned module.
## 
<br><br>

# Configure and Verify Network Connections
<br>

## Testing Network Connectivity
- ping
- ip add

### ping
- `ping google.com` or any other site to check the network connection.
- `ping 8.8.8.8` to check if there is not a problem with dns.

### ip add
- `ip add` will give you ip addresses on your local computer.

### ip route
- `ip route` to see the routing table.

### curl ifconfig.me
- `curl ifconfig.me` to see public ip address of your network.

### hostname -I
- `hostname -I` to get the configured ip address for your host. 
##
<br>

## Testing DNS
- `ping`
- `dig @server_ip[optional] host_ip`
- `nslookup host_ip server_ip[optional]`
- `host host_ip server_ip[optional]`
- DNS will look in to `/etc/hosts` file first.
- Restart the local server using `service dnsmasq restart`
## 
<br>

## Locating Common Network config Files

- `/etc/hosts`
- `/etc/resolv.conf`
- `/etc/nsswitch.conf`
##
<br>

## Identifying Ubuntu and Debian network files

- `/etc/network/interfaces` in older version of ubuntu
- `/etc/netplan/*` in newer version of ubuntu. It will have yaml file. If you make changes in this file than `sudo netplan apply` to apply those changes.
- `Network Manager` can be used in GUI. 1:02
