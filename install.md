# Prerequisites

sudo -s 

apt install gcc linux-headers-$(uname -r) build-essential subversion mercurial openssl gettext libssl-dev screen libv4l-dev

apt install gcc git make patchutils libproc-processtable-perl

lsusb -vvv |grep 5580

# Main installation

sudo -s

mkdir tbsdriver

cd tbsdriver/

git clone https://github.com/tbsdtv/media_build.git

git clone --depth=1 https://github.com/tbsdtv/linux_media.git -b latest ./media

cd media_build/

make dir DIR=../media

make distclean

make -j 4

make install

# Firmware installation

cd ..

wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2

tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/

reboot

# Post-installation
cd tbsdriver/

dmesg | grep frontend

# Usage
cd /dev/dvb/

cd adapter0/

ln -s demux0 demux1

ln -s dvr0 dvr1

ln -s net0 net1

apt install dvblast

dvblast -f 650000000 -b 8 -a 0 -5 dvbt

sudo dvblast -f 11766000 -s 29900000 -v 13 -p -a 0 -n 1 -5 dvbs2 -m psk_8

sudo dvblast -f 11023000 -s 23500000 -v 18 -S 2 -a 0 -n 1 -5 dvbs2 -m psk_8 -R 0


***WARNING:*** You do not have the full kernel sources installed.
This does not prevent you from building the v4l-dvb tree if you have the
kernel headers, but the full kernel source may be required in order to use
make menuconfig / xconfig / qconfig.

If you are experiencing problems building the v4l-dvb tree, please try
building against a vanilla kernel before reporting a bug.

Vanilla kernels are available at http://kernel.org.
On most distros, this will compile a newly downloaded kernel:

cp /boot/config-`uname -r` <your kernel dir>/.config
cd <your kernel dir>
make all modules_install install

Please see your distro's web site for instructions to build a new kernel.

WARNING: This is the V4L/DVB backport tree, with experimental drivers
	 backported to run on legacy kernels from the development tree at:
		http://git.linuxtv.org/media-tree.git.
	 It is generally safe to use it for testing a new driver or
	 feature, but its usage on production environments is risky.
	 Don't use it in production. You've been warned.
