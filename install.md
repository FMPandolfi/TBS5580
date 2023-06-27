# Prerequisites

sudo -s 

## *Install building tools*

apt install git gcc make patchutils libproc-processtable-perl linux-headers-$(uname -r) build-essential subversion mercurial openssl gettext libssl-dev screen libv4l-dev bin86 byacc elfutils fakeroot libbison-dev liblz4-dev liblz4-tool libelf-dev libfl-dev ubuntu-dev-tools

## *Install kernel sources*

apt install linux-source-5.19.0

## *Check if USB device is connected*

lsusb -vvv | grep 5580

# Main installation

sudo -s

mkdir tbsdriver && cd tbsdriver/

## *Get sources*

git clone https://github.com/tbsdtv/media_build.git

git clone --depth=1 https://github.com/tbsdtv/linux_media.git -b latest ./media

## *Compile*

cd media_build/

make dir DIR=../media

make distclean

make -j 4

make install

cd ..

# Firmware installation

## *Get firmware*

wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2

## *Unzip and install firmware*

tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/

reboot

# Post-installation check

cd tbsdriver/

dmesg | grep frontend

# DVB tools setup

## *Simbolic links setup*
cd /dev/dvb/

cd adapter0/

ln -s demux0 demux1

ln -s dvr0 dvr1

ln -s net0 net1

## *Install dvblast*

apt install dvblast

# Usage examples

dvblast -f 650000000 -b 8 -a 0 -5 dvbt

sudo dvblast -f 11766000 -s 29900000 -v 13 -p -a 0 -n 1 -5 dvbs2 -m psk_8

sudo dvblast -f 11023000 -s 23500000 -v 18 -S 2 -a 0 -n 1 -5 dvbs2 -m psk_8 -R 0
