# RaspberryPi-USBgadget-mode-on-MacOS
This repo is not quite a full guide, just the way it worked for me and I felt it would help someone out there.
Using a Raspberry Pi Zero 2 w with the RPIOS (in my case i used the lite version) and a M3 Macbook air, i had problems with the USB Gadget mode and here my fix.

This project was implemented using: Raspberry PI Zero 2 w, M3 Macbook Air with Sequoia 15.5 MacOS, Ubuntu 20.04.6 LTS 

note: I did this without using the RPI, so this is just modifying the SDCard with a Ubuntu 20.04.6 LTS

note2: After installing the OS on the SDCard (i used RaspberryPI imager), there are two partitions: bootfs and rootfs. You **cannot** access the rootfs with MacOS or Widnows. The rootfs is ext4 format which MacOS and Windows cannot natively read.

## Steps
 1- In the "bootfs" partition, edit the config.txt file and just the line bellow '[all]' add this line:
 ```dtoverlay=dwc2```
 
 2- Again in the "bootfs" partition, edit the cmdline.txt file and just after the 'rootwait' command add this line (there should be one space character between the previous and next command from line bellow):
 ```modules-load=dwc2,g_ether```

 3- Now moving to the "rootfs" partition, navigate to '/etc/udev/rules.d' and add a file named '90-usb-gadget.rules'. Edit the fiel you just made and add this line:
```SUBSYSTEM=="net",ACTION=="add",KERNEL=="usb0",RUN+="/sbin/ifconfig usb0 192.168.1.2 netmask 255.255.255.0",RUN+="/usr/bin/python -c 'import time; time.sleep(20)'",RUN+="/sbin/ip route add 192.168.1.1 dev usb0"```

4- Insert the SD Card on your RPI and Connect your RPI through a Data cable to your Mac device and on your Mac Open System Preferences → Network or System Settings → Network and you should see a device like this: ```RNDIS/Ethernet Gadget```. That is your RPI and if it is Connected you are all set

5- Opne terminal on your Mac and run the ssh command:
```ssh username@raspberrypi.local```
and after that if everything works fine, it should ask for your RPI's password and after entering your password you should have your RPI's terminal.
note: If you haven't changed the setting the default are username: pi and password: raspberry.

