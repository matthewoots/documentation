# Flash or Backup Radxa Zero Filesystem

[home](../README.md)

[back](radxa-uboot-usb.md)

---
## Introduction
How to flash or backup a Radxa Zero with an existing image or a custom image

---

## Steps
If you want to backup (clone an image) of an existing Radxa Zero board
```bash
# To clone pre-existing image
lsblk -p 

# $SD_CARD_DEVICE_NAME = /dev/sdX shown on lsblk -p output
# $IMAGE_FILE_NAME = ~/Downloads/sample.img
sudo dd bs=4M if=[$SD_CARD_DEVICE_NAME] of=[$IMAGE_FILE_NAME] conv=fsync
sudo chown $USER: [$IMAGE_FILE_NAME]

# Installation of PiShrink
cd ~
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
sudo mv pishrink.sh /usr/local/sbin/pishrink
sudo pishrink [$IMAGE_FILE_NAME]

cd ~/Downloads/
tar -czf [$IMAGE_NAME].tar.gz [$IMAGE_NAME].img
```

If you want to flash a Radxa Zero board
```bash
# Go and get Balena Etcher at https://github.com/balena-io/etcher/releases
# or you can get this release link for v1.7.9
cd Downloads
wget https://github.com/balena-io/etcher/releases/download/v1.7.9/balenaEtcher-1.7.9-x64.AppImage 

sudo chmod +x balenaEtcher-1.7.9-x64.AppImage
./balenaEtcher-1.7.9-x64.AppImage
# Load the image onto the board
```

To acquire Radxa images, you will need to go to this site https://github.com/radxa/debos-radxa/releases. You can get the `ubuntu-focal` (ubuntu 20.04) images such as `radxa-zero-ubuntu-focal-server-arm64-2022XXXX-XXXX-mbr.img.xz`

---

After this you can proceed to [Creating Radxa Access Point](radxa-access-point.md)