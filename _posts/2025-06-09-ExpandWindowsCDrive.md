---
title: Expanding Windows C Drive
categories: [Guides]
tags: [windows, resize]
---

## Expanding Windows C Drive

1. ### From within Proxmox

   1. **Shut down the VM**.
   2. **Resize the disk** in the VM settings.
   3. **Boot the VM**.

2. ### From within Windows

   1. **Delete the Recovery Partition**:
      - **Warning**: This will remove the Windows Recovery Environment.
      - **Note**: Ensure you have a backup of your data before proceeding.
      1. Open `cmd` as Administrator.
      2. Run the command:

            ```batch
            reagentc /disable
            diskpart
            list disk
            select disk <Boot_Disk_Number>
            list partition
            select partition <Recovery_Partition_Number>
            delete partition override
            exit
            ```

   2. **Resize the C drive**:
      1. Launch Disk Manager (`diskmgmt.msc`).
      2. Right-click the C drive and select **Extend Volume**.
      3. Follow the prompts to extend the volume using the unallocated space.
         **Note**: Leave 1024mb unallocated space at the end of the disk for Windows Recovery.
   3. **Configure the Recovery Partition**:
      1. Launch Disk Manager (`diskmgmt.msc`).
      2. Right-click the unallocated space and select **New Simple Volume**.
      3. Follow the prompts to create a new partition.
         - **Assign a Drive Letter or Path**: "Do not assign a drive letter or drive path"
         - **Size**: 1024 MB
         - **File System**: NTFS
         - **Label**: Recovery
      4. Open `cmd` as Administrator.
      5. Run the command:

         ```batch
         diskpart
         select disk <Boot_Disk_Number>
         list partition
         select partition <Recovery_Partition_Number>
         set id=27
         exit
         reagentc /enable
         ```
