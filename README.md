# Setup process of an PXE server on Ubuntu_22.04.3-live-server using dnsmasq
Setup of a Ubuntu 22.04.3 Server to use as an PXE server to deploy Images over Network with two networks

#  1. Setup Netplan

   Change network settings
   ````
   vim /etc/netplan/00-installer-config.yaml
   ````
   and apply changes:
   ````
   netplan apply
   ````

#  2. Preparation for DNS Service

   Stopping and disabling the systemd-resolved service.
   ````
   systemctl stop systemd-resolved
   systemctl disable systemd-resolved
   unlink /etc/resolv.conf
   ````

   Adjust nameservers
   ````
   vim /etc/resolv.conf
   ````


#  3. Downloading iPXE Source Code and Compiling iPXE

   Installing needed tools and libs
   ````
   apt install build-essentials liblzma isolinux git -y
   ````

   Clone the ipxe repo

   ````
   cd /git
   git clone https://github.com/ipxe/ipxe.git
   ````

   create a boot configuration so iPXE automaticly uses your computer

   ````
   cd /git/ipxe/src
   vim bootconfig.ipxe
   ````

   In the same directory execute following command to compile  boot firmware files for BOOT and UEFI systems

   ````
   make bin/ipxe.pxe bin/undionly.kpxe bin/undionly.kkpxe bin/undionly.kkkpxe bin-x86_64-efi/ipxe.efi EMBED=bootconfig.ipxe
   ````
   then the files can be copied to the firmware folder inside pxeboot
   ````
   cp -v bin/{ipxe.pxe,undionly.kpxe,undionly.kkpxe,undionly.kkkpxe} bin-x86_64-efi/ipxe.efi /srv/pxeboot/firmware/
   ````

#  4. Installing and Configuring DHCP and TFTP servers

   Install dnsmasq
   ````
   apt install dnsmasq -y
   ````

   make a backup of the conf
   ````
   mv -v /etc/dnsmasq.conf /etc/dnsmasq.conf.backup
   ````

   setup a new configuration file for dnsmasq
   ````
   vim /etc/dnsmasq.conf
   ````

   After setting up the new configuration we need to restart the dnsmasq service
   ````
   systemctl restart dnsmasq
   ````

#  5. Installing and configuring NFS server

   Installing the needed package
   ````
   apt install nfs-kernel-server -y
   ````

   To share the pxeboot/ directory we need to adjust following file
   ````
   vim /etc/exports
   ````
   and execute following command
   ````
   exportfs -av
   ````

# 6. Configuring iPXE to PXE Boot Ubuntu 22.04.3 live server installer

   Download the server image with following command, the directory doesn't matter besause the files will be extracted and copied to a different folder
   ````
   wget https://releases.ubuntu.com/jammy/ubuntu-22.04.3-live-server-amd64.iso
   ````

   mount the iso file to following directory
   ````
   mount -o loop /directory/to/ubuntu-22.04.3-live-server-amd64.iso /mnt
   ````

   create a dedicated folder for the image files and copy the iso files into the directory
   ````
   mkdir -pv /srv/pxeboot/os-images/ubuntu-22.04.3-live-server-amd64
   rsync -avz /mnt/ /srv/pxeboot/os-images/ubuntu-22.04.3-live-server-amd64/
   ````
   when the process is finished you can unmount the image and delete it
   ````
   unmount /mnt
   rm -v /directory/to/ubuntu-22.04.3-live-server-amd64.iso
   ````

   seting up the iPXE boot config
   ````
   vim /srv/pxeboot/config/boot.ipxe
   ````

#  7. PXE boot

   Now, boot any computer on the network via PXE and you should see that the iPXE firmware is being used for the process.


# This guide was copied from https://linuxhint.com/pxe_boot_ubuntu_server/, I adjusted the configurations to suit my needs.

