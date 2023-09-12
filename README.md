# Setup process of an PXE server on Ubuntu_22.04.3-live-server using dnsmasq
Setup of a Ubuntu 22.04.3 Server to use as an PXE server to deploy Images over Network with two networks

#  1. Setup Netplan

   ````
   vim /etc/netplan/00-installer-config.yaml
   ````
   and apply changes
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

   ````
   vim /etc/resolv.conf
   ````

   
   
