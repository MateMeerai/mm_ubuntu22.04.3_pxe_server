# mm_ubuntu22.04.3_pxe_server
Setup of a Ubuntu 22.04.3 Server to use as an PXE server to deploy Images over Network with two networks

1. Setup Netplan

   ```
   vim /etc/netplan/00-installer-config.yaml
   ```
   and apply changes
   ```
   netplan apply
   ```

3. 
