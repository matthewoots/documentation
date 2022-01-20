# Setup to read Radxa Zero Filesystem

Guide to this is from https://wiki.radxa.com/Zero/dev/u-boot

```
sudo apt install pip3
sudo pip3 install pyamlboot
# Press the boot button before connecting the PC to the Radxa Zero
lsusb
# lsusb should show Armlogic, Inc. GX-CHIP
# You should download the rz-udisk-loader.bin from the the files folder in this repo
boot-g12.py <path to rz-udisk-loader.bin>
```

```
# Current ssh option (each radxa-zero is called by their 2 digit ID)
# ssh rock@radxa-zero-00.local
```
