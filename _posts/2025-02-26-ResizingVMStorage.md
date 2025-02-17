---
title: Expanding VM Storage
categories: [Guides]
tags: [Proxmox, VM, storage]
---

## Expanding VM Storage

1. From within Proxmox:
   1. **Shut down the VM**.
   2. **Resize the disk** in the VM settings.
   3. **Boot the VM**.

2. From within the VM:
   1. Check if the additional space is recognized by the system:
       ```bash
       sudo cfdisk
       ```
       If not, rescan the disk:
       ```bash
       echo 1 > /sys/class/block/sda/device/rescan # Replace 'sda' with the appropriate disk
       ```
   2. **Resize the partition**:
       1. Launch `cfdisk`:
           ```bash
           sudo cfdisk
           ```
       2. Select the partition to resize. Typically, `/dev/sda3`.
       3. Select "Resize" from the bottom menu.
       4. Press ENTER to select.
       5. Press ENTER to confirm.
       6. Select "Write" from the bottom menu.
       7. Type `yes` and press ENTER to confirm.
       8. Press 'Q' to quit.
   3. **Resize the physical volume**:
       1. Resize the physical volume:
           ```bash
           sudo pvresize /dev/sda3
           ```
       2. Verify the physical volume has been resized:
           ```bash
           sudo pvdisplay
           ```
   4. **Resize the logical volume**:
       1. Confirm Volume Group free space:
           ```bash
           sudo vgdisplay
           ```
       2. Resize the logical volume:
           ```bash
           sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
           ```
   5. **Resize the filesystem**:
       1. Resize the filesystem:
           ```bash
           sudo resize2fs /dev/sda3
           ```
       2. Verify the filesystem has been resized:
           ```bash
           df -h
           ```
