+++
categories = ["linux","raspberry pi"]
series = ["pxe"]
date = "2015-06-20T00:18:54+02:00"
description = "PXE setup on Rapberry PI part 1"
keywords = ["PXE","Rapberry PI","setup","raspbian","Linux","blog"]
title = "PXE setep on Rapberry PI part 1"
draft = true

+++

## Raspberry PI PXE setup

### Install and setup DHCPD

sudo vi /etc/dhcp/dhcpd.conf

sudo /etc/init.d/isc-dhcp-server restart

### Install and setup TFTP

sudo apt-get install tftp-hpa

sudo vi /etc/default/tftpd-hpa

### Install and setup PXE

sudo apt-get install pxe

sudo /etc/init.d/pxe restart

## Next step?

Next operations will be in the part 2, stay tuned!
