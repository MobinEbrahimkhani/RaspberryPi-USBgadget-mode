# RaspberryPi-USBgadget-mode-on-MacOS
This project was implemented using: Raspberry PI Zero 2 w with RPIOS lite 64-bit, M3 Macbook Air with Sequoia 15.5 MacOS, a PC with Ubuntu 20.04.6 LTS

This is not quite a full guide, but the way it worked for me and I felt it would help someone out there. 

I did this without using the RPI, so this is just modifying the SDCard with a Linux PC (which in my case was a Ubuntu). 

After installing the OS on the SDCard (i used RaspberryPI imager), there are two partitions: bootfs and rootfs. You **cannot** access the rootfs with MacOS or Widnows. This is because the rootfs is ext4 format which MacOS and Windows cannot natively read. So i recomend using a Linux PC  for the setup if you can't use the RPI itself directly (like me).


Here is what i did
## Steps
 1- In the "bootfs" partition, edit the config.txt file and just the line bellow '[all]' add this line:
 ```dtoverlay=dwc2```
 
 2- Again in the "bootfs" partition, edit the cmdline.txt file and just after the 'rootwait' command add this line (there should be one space character between the previous and next command from line bellow):
 ```modules-load=dwc2,g_ether```

 3- Now moving to the "rootfs" partition, navigate to '/etc/udev/rules.d' and add a file named '90-usb-gadget.rules'. Edit the fiel you just made and add this line:
```SUBSYSTEM=="net",ACTION=="add",KERNEL=="usb0",RUN+="/sbin/ifconfig usb0 192.168.1.2 netmask 255.255.255.0",RUN+="/usr/bin/python -c 'import time; time.sleep(20)'",RUN+="/sbin/ip route add 192.168.1.1 dev usb0"```

4- Insert the SD Card on your RPI and Connect your RPI through a Data cable to your Mac device and on your Mac Open System Preferences → Network or System Settings → Network and you should see a device like ```RNDIS/Ethernet Gadget```. That is your RPI and if it is Connected you are all set

5- Open terminal on your Mac and run the ssh command:
```ssh username@raspberrypi.local```. If everything works fine, it should ask for your RPI's password and after entering your password you should be in your RPI's terminal.

*note: If you haven't changed the defaults they should be username: ```pi``` and password: ```raspberry```.*

**This project was initially inspired by a post on https://forums.raspberrypi.com/viewtopic.php?t=306121&sid=6f23dece3a28a0281b971be8b0ec9763&start=25**
