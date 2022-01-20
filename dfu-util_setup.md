# Simple Setup for DFU-Utils

Get the release from http://dfu-util.sourceforge.net/releases/ and download `dfu-util-0.11.tar.gz`

```
cd ~/Downloads
tar -zxf dfu-util-0.11.tar.gz --directory ~/dfu-util
cd ~/dfu-util
./configure
make
sudo make install
dfu-util -a 0 --dfuse-address 0x08000000 -D  <directory of .bin file> 
```
