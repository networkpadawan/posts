Title: GNS3 with cloud connection on OSX 10.6.6
Author: Tiago Sousa
Date: 2011-01-16 12:48:34
Tags: CCNP, GNS3 &amp; IOU


Because i need it for my CCNP studies, i was getting around installing GNS3 and connecting it to "real" interfaces.

After all the "googling"  i got everything running and here is the simple how-to:

Download the OSX [package ](http://downloads.sourceforge.net/gns-3/GNS3-0.7.3-intel-x86_64.dmg?download)and the dynamips [binary](http://downloads.sourceforge.net/gns-3/dynamips-0.2.8-RC2-OSX-Leopard.intel.bin?download).

Copy the package to the Applications Folder.

Copy the bynary to  the path/Applications/GNS3.app/Contents/Resources

Download and Install [tuntaposx](http://downloads.sourceforge.net/tuntaposx/tuntap_20090913.tar.gz)

Go to the path above and change owner and group of dynamips binary as root and wheel:
__

_sudo chown root:wheel dynamips-0.2.8-RC2-OSX-Leopard.intel.bin_

Set setuid and setgid bits accordingly:
__

_sudo chmod 6755 dynamips-0.2.8-RC2-OSX-Leopard.intel.bin_

Done!

Now anytime a tap interface is needed, GNS3 will call it (to max of 15) via the cloud interface

PS.: don´t forget to go to terminal and add an interface to the tap interface:_ ifconfig tap0 ipaddr netmask_