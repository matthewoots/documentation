# Setup to read Radxa Zero Filesystem

[home](../README.md)

---
## Introduction
How to expose Radxa Zero Filesystem as a usb device, some are taken from these official websites from Radxa 
- https://wiki.radxa.com/Zero/dev/u-boot
- https://wiki.radxa.com/Zero/dev/maskrom

---

## Steps
```bash
sudo apt install pip3
sudo pip3 install pyamlboot
# Press the boot button before connecting the PC to the Radxa Zero
lsusb
# lsusb should show Armlogic, Inc. GX-CHIP
# You should download the rz-udisk-loader.bin from the the files folder in this repo
sudo boot-g12.py <path to rz-udisk-loader.bin>
```

If the board is a new board, you may need to use `radxa-zero-erase-emmc.bin` instead to clear the EMMC then expose it as a device

---

After this you can proceed to [Flashing Radxa with Image](radxa-flash-backup-image.md)
