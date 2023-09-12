# Explanation of the boot.ipxe

The PXE menu is configured in boot.ipxe file and looks like this

````
#!ipxe

set server_ip  192.168.121.2
set root_path  /srv/pxeboot

menu Select an OS to boot
item ubuntu-22.04.3-live-server-amd64         Install Ubuntu Server 22.04.3 LTS

choose --default exit --timeout 10000 option && goto ${option}

:ubuntu-22.04-desktop-amd64
set os_root os-images/ubuntu-22.04.3-live-server-amd64
kernel tftp://${server_ip}/${os_root}/casper/vmlinuz
initrd tftp://${server_ip}/${os_root}/casper/initrd
imgargs vmlinuz initrd=initrd boot=casper maybe-ubiquity netboot=nfs ip=dhcp nfsroot=${server_ip}:${root_path}/${os_root} quiet splash ---
boot
````

````#!ipxe```` tells the system this is an ipxe file

````set server_ip```` and ````set root_path```` define the adress of the pxe server and where on it the pxe files are located as variables

````menu```` defines the headline in the pxe menu

````item```` defines single entries to choose from the first beeing the direct identifier and the second the depicted entry shown in the menu

````:ubuntu-22.04-desktop-amd64```` tells you that now starts the contet of the chosen entry
after it follows the location of the needed kernel, initrd, the configuration and arguments for the server
````set os_root os-images/ubuntu-22.04.3-live-server-amd64```` sets the location of the image as a variable

# Add new image to install
 for this you need to insert a new ````item imagename         diplayed_name```` with the corresponding ````:imagename````
