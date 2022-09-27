# How to configure Device Tree

[home](README.md)

---
## Introduction

`dts` is an editable file, while `dtb` is binary hence, device-tree-compiler helps to convert between binary and editable format. (Use case in `boot` files)

---
## Setup
```bash
sudo apt-get install device-tree-compiler
# dtc -I dtb -O dts -f devicetree_file_name.dtb -o devicetree_file_name.dts
# dtc -I dts -O dtb -f devicetree_file_name.dts -o devicetree_file_name.dtb
```