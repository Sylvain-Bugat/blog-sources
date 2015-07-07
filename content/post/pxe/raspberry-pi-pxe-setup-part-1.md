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

set DHCP network

sudo /etc/init.d/isc-dhcp-server restart

### Install and setup TFTP

Install the TFTP(Trivial FTP) server with this command:
sudo apt-get install tftp-hpa


sudo vi /etc/default/tftpd-hpa

set TFTP main directory to /srv/tftp:
TFTP_DIRECTORY="/srv/tftp"

### Install and setup PXE

sudo apt-get install pxe

sudo /etc/init.d/pxe restart


### Setup network boot menu

pxelinux.cfg/default

files:
chain.c32
images
ldlinux.c32
libcom32.c32
libutil.c32
menu.cfg
pxelinux.0
pxelinux.cfg
pxelinux.cfg/default
vesamenu.c32


### Setup network boot images

Example for CentOS 7.1:
images
images/centos
images/centos/7.1
images/centos/7.1/x86_64
images/centos/7.1/x86_64/initrd.img
images/centos/7.1/x86_64/upgrade.img
images/centos/7.1/x86_64/vmlinuz

## Next step?

Next operations will be in the part 2, stay tuned!
