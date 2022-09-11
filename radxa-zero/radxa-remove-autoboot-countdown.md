# Create an Access Point from Radxa Zero

[home](../README.md)

[back](radxa-access-point.md)

---
## Introduction
How to remove the `Hit any key to stop autoboot:  3 2 1 0` error that happens on boot. The problem is when starting up the board, any input from `UARTs` etc `/dev/ttyAML0` and `/dev/ttyAML1` will cause the board to run in a repeated boot.

3 websites that this documentation is reference from:
- https://forum.radxa.com/t/instruction-of-how-to-fix-2-seconds-delay-at-boot-and-not-working-reboot/7898
- https://forum.radxa.com/t/install-os-direct-to-emmc-with-ubuntu/7435
- https://community.nxp.com/t5/Layerscape/How-to-disable-uboot-couting-down-feature-during-serial-console/m-p/940513#M4605

---

## Steps

Install dependencies
```bash
sudo apt-get install -y wget bc nano mc build-essential autoconf libtool cmake pkg-config git python-dev swig libpcre3-dev libnode-dev gawk wget diffstat bison flex device-tree-compiler libncurses5-dev gcc-aarch64-linux-gnu g++-aarch64-linux-gnu binfmt-support binfmt-support qemu-user-static gcc-aarch64-linux-gnu gcc-arm-linux-gnueabihf fastboot
```

Git clone 2 packages
```bash
git clone https://github.com/radxa/u-boot.git
nano /u-boot/configs/radxa-zero_defconfig
# Add "CONFIG_BOOTDELAY=-2" before "#CONFIG_DISPLAY_CPUINFO is not set"
git clone https://github.com/radxa/fip.git
```

Compile from `u-boot`
```bash
cd u-boot
export ARCH=arm
export CROSS_COMPILE=aarch64-linux-gnu-
make radxa-zero_defconfig
make
```

Compile from `fip/radxa-zero` **IMPORTANT** There will be no output after `make` since `MAKEFLAGS += --silent` which indicates no verbose
```bash
cd u-boot
cp u-boot.bin ../fip/radxa-zero/bl33.bin
cd ../fip/radxa-zero
make
```

Use the uboot method as described in [here](radxa-uboot-usb.md) and use `dd` to write over the `eMMC` this would be better since fastboot does not have permission shown in the `Troubleshooting and Explanation` section below
```bash
lsusb
# ...
# Bus 001 Device 008: ID 1b8e:c003 Amlogic, Inc. GX-CHIP
# ...
lsblk
# check for where the blk is located at etc sda or sdb
# in this case replace sdX with it
cd ~/fip/radxa-zero
sudo dd if=u-boot.bin of=/dev/sdX bs=512 seek=1
```

After this reboot

---

## Troubleshooting and Explanation
**IMPORTANT**, somehow for the guides the address `0x200` works for them but for the newer boards, there is an error in writing to the eMMC, therefore use the `dd` method which is a cleaner approach

```bash
U-Boot 2022.04-00001-gaf4d42d7 (Jun 29 2022 - 15:19:49 +0800) radxa-zero                          
                                                                                                  
Model: Radxa Zero                                                                                 
SoC:   Amlogic Meson G12A (S905Y2) Revision 28:c (30:2)                                           
DRAM:  3.8 GiB                                                                                    
Core:  375 devices, 20 uclasses, devicetree: separate                                             
MMC:   sd@ffe03000: 0, sd@ffe05000: 1, mmc@ffe07000: 2                                            
Loading Environment from nowhere... OK                                                            
In:    serial                                                                                     
Out:   serial                                                                                     
Err:   serial                                                                                     
Net:   No ethernet found.                                                                         
starting USB...                                                                                   
Bus usb@ff500000: Register 3000140 NbrPorts 3                                                     
Starting the controller                                                                           
USB XHCI 1.10                                                                                     
scanning bus usb@ff500000 for devices... 1 USB Device(s) found                                    
       scanning usb for storage devices... 0 Storage Device(s) found                              
Hit any key to stop autoboot:  0                                                                  
Enter fastboot mode. Press Ctrl+C to stop...
** Bad device specification mmc 0x200_a **                                                        
Couldn't find partition mmc 0x200_a                                                               
Couldn't find partition mmc 0x200                                                                 
Couldn't find partition mmc 0x200                                                                 
Starting download of 1408368 bytes                                                                
..........                                                                                        
downloading of 1408368 bytes finished                                                             
Couldn't find partition mmc 0x200
```



---
