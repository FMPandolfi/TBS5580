# Prerequisites

sudo -s 

apt install git make patchutils libproc-processtable-perl linux-headers-$(uname -r)

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

cd ..

wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2

tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/

reboot

# Post-installation
cd tbsdriver/

dmesg |grep frontend

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
