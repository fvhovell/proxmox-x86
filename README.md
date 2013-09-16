proxmox-x86
===========

Patches to the Proxmox VE software for compilation on x86 / i386.
=======
# Install a standard Debian Wheezy (x86)

Created based on: http://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Wheezy

## Preparation

1. Install a standard Debian Wheezy (x86), for details see Debian. Go for a LVM based partitioning and a fixed IP and take care that you have enough free space for snapshots (needed for online backup with vzdump)

2. Please make sure that your hostname is resolvable via /etc/hosts, i.e you need an entry in /etc/hosts which assigns an IP address to that hostname.

## Install Proxmox VE
Adapt your sources.list

1. Adapt your sources.list and add the Proxmox VE repository:

    `nano /etc/apt/sources.list`

        deb http://ftp.debian.org/debian squeeze main contrib
        deb http://security.debian.org/ squeeze/updates main contrib
		
	# PVE packages provided by fvhovell
        deb http://www.fvhovell.nl/proxmox pve3/

2. Update your repository and system by running:

        aptitude update
        aptitude full-upgrade

3. Install Proxmox VE Kernel

        aptitude install pve-firmware
        aptitude install pve-kernel-2.6.32-24-pve

4. Reboot and make sure to select Proxmox VE Kernel on the boot loader (grub2).

5. Optional - install Kernel headers:

        aptitude install pve-headers-2.6.32-24-pve

6. Now restart the system using the Proxmox VE kernel.

## Install Proxmox VE packages

1. Make sure you are running the Proxmox VE Kernel, otherwise the installation will fail.
Check the currently active Kernel:

        uname -a
        Linux 2.6.32-24-pve ... 

2. Install the Proxmox VE packages:

        aptitude install proxmox-ve-2.6.32

## Install the rest of needed packages:

    aptitude install ntp ssh lvm2 postfix ksm-control-daemon vzprocps

Accept the suggestion to remove Exim and configure postfix according to your network.

## Connect to the Proxmox VE web interface
Connect to the admin web interface (https://youripaddress:8006) and configure the vmbr0 and review all other settings, finally reboot to check if everything is running as expected.
