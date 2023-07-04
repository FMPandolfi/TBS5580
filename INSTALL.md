# Prerequisites
  1) **Enter super-user mode**\
sudo -s 

  2) **Install building tools**\
apt install git gcc make patchutils libproc-processtable-perl linux-headers-$(uname -r) build-essential subversion mercurial openssl gettext libssl-dev screen libv4l-dev bin86 byacc elfutils fakeroot libbison-dev liblz4-dev liblz4-tool libelf-dev libfl-dev ubuntu-dev-tools

  3) **Install kernel sources**\
apt install linux-source-5.19.0

  4) **Check if USB device is connected**\
lsusb -vvv | grep 5580

# Main installation
  1) **Setup working environment**\
mkdir tbsdriver && cd tbsdriver/ \

  2) **Get sources**\
git clone https://github.com/tbsdtv/media_build.git \
git clone --depth=1 https://github.com/tbsdtv/linux_media.git -b latest ./media

  3) **Compile**\
cd media_build\
make dir DIR=../media\
make distclean\
make -j 4\
make install\
cd ..

# Firmware installation
  1) **Get firmware**\
wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2

  2) **Unzip and install firmware**\
tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/ \
reboot

# Post-installation check
cd tbsdriver/ \
dmesg | grep frontend

# DVB tools setup
  1) **Simbolic links setup**\
cd /dev/dvb/adapter0/ \
ln -s demux0 demux1\
ln -s dvr0 dvr1\
ln -s net0 net1

## dvblast installation
apt install dvblast

### dvblast usage examples
- **DVB-T reception**\
dvblast -f 650000000 -b 8 -a 0 -5 dvbt

- **DVB-S2/S2X TS stream**\
sudo dvblast -f 11766000 -s 29900000 -v 13 -p -a 0 -n 1 -5 dvbs2 -m psk_8

- **DVB-S2X GSE stream**\
sudo dvblast -f 11023000 -s 23500000 -v 18 -S 2 -a 0 -n 1 -5 dvbs2 -m psk_8 -R 0\
*NOTE: Not available for TBS5580*

## dvbsnoop installation
apt install dvbsnoop

### dvbsnoop usage examples
- **PID Scan**\
dvbsnoop -s pidscan -frontend /dev/dvb/adapter0/frontend1 -pd 9 -ph 3 -maxdmx 20

- **"Spider scan" from PAT pid**\
dvbsnoop -s sec -spiderpid -frontend /dev/dvb/adapter0/frontend1 -ph 3 -N 1 0

- **"Spider scan" from SDT pid**\
dvbsnoop -s sec -spiderpid -frontend /dev/dvb/adapter0/frontend1 -ph 3 -N 1 17

## TSDuck installation

### TSDuck usage examples
- **Simple tuning on a satellite TS using a DVB device as input**\
tsp -I dvb --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 
- **Analyze the DVB-S2 TS from a DVB device**\
tsp -I dvb --signal-timeout 4 --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 -P until --seconds 4 -P analyze -O drop
- **Same as previous but with normalize output to simplify parsing operations**\
tsp -I dvb --signal-timeout 4 --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 -P until --seconds 4 -P analyze --normalized -O drop \
- **Write TS tables from satellite TS**\
tsp -I dvb --signal-timeout 4 --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 -P until --seconds 4 -P tables --xml-output tsp-dvb-tables-dump-test.xml -O drop 
- **Write TS tables in PSI/SI format from satellite TS**\
tsp -I dvb --signal-timeout 4 --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 -P until --seconds 4 -P tables --psi-si -O drop
- **SDT table dump in xml format**\
tsp -I dvb --signal-timeout 4 --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 -P until --seconds 4 -P tables --pid 17 --xml-output tsp-dvb-tables-sdt-pid17-dump-test.xml -O drop
- **EIT table dump in xml format**\
tsp -I dvb --signal-timeout 4 --device-name /dev/dvb/adapter0:1 -f 11766000000 -s 29900000 -m 8-PSK --roll-off 35 --delivery-system DVB-S2 -P until --seconds 4 -P tables --pid 18 --xml-output tsp-dvb-tables-eit-pid18-dump-test.xml -O drop




