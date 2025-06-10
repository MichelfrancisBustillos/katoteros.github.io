---
title: Windows VM Creation
categories: [Guides]
tags: [onboarding, vm, windows]
---

## Windows VM Creation in Proxmox

1. Preperation:
   1. Download the Windows ISO from the [official Microsoft website](https://www.microsoft.com/software-download/windows10ISO).
   2. Upload the ISO to Proxmox:
      - Navigate to the Proxmox web interface.
      - Go to `Datacenter` > `Storage` > `ISO Images`.
      - Click on `Upload` and select the downloaded Windows ISO file.
   3. Download the VirtIO drivers ISO from the [Fedora Project](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/).
      - Upload the VirtIO ISO to Proxmox in the same way as the Windows ISO.

2. VM Creation
   1. In the Proxmox web interface, click on `Create VM`.
   2. Fill in the following details:
        - General:
           - **Node**: Select the Proxmox node where you want to create the VM.
           - **VM ID**: Leave it as default or set a custom ID.
           - **Name**: Enter a name for your VM (e.g., `Windows-VM`).
        - OS:
           - **ISO Image**: Select the Windows ISO you uploaded.
           - **Guest OS Type**: Choose `Microsoft Windows`.
           - **Version**: Select the appropriate version (e.g., `Windows 10/2016`).
           - **Add additional drive for VirtIO drivers**: Check this option to add the VirtIO drivers ISO.
           - **VirtIO Drivers ISO**: Select the VirtIO ISO you uploaded.
        - System:
           - **BIOS**: Select `OVMF (UEFI)` if you want UEFI support, otherwise leave it as `SeaBIOS`.
           - **Machine**: Leave it as default.
           - **Qemu Agent**: Enable this option for better integration.
        - Disks:
           - **Hard Disk**: Set the disk size (e.g., `50 GB`).
           - **Cache**: Set to `Write back` for better performance.
        - CPU:
           - **Cores**: Set the number of cores (e.g., `2`).
           - **Type**: Leave it as default.
        - Memory:
           - **Memory**: Set the amount of RAM (e.g., `4096 MB`).
           - **Ballooning**: Enable this option for dynamic memory management.
        - Network:
           - **Model**: Select `virtio` for better performance.
           - **Bridge**: Choose the appropriate bridge (e.g., `vmbr0`).
   3. Click `Finish` to create the VM.

3. Installing Windows
    1. Start the VM by selecting it in the Proxmox web interface and clicking on `Start`.
    2. Open the console for the VM.
    3. Follow the Windows installation prompts.
    4. When prompted for drivers, load the VirtIO drivers from the additional CD drive.
       1. Click on `Load driver`.
       2. Browse to the VirtIO CD drive and navigate to the appropriate folder for your Windows version (e.g., `viostor` for storage drivers).
          1. **Hard Disk**: `vioscsi\w10\amd64`
          2. **Network**: `NetKVM\w10\amd64`
          3. **Ballooning**: `vioser\w10\amd64`
       3. Select the appropriate driver and click `Next`.
    5. Complete the Windows installation as usual.

4. Post-Installation
    1. Install the QEMU guest agent for better performance and management.
       1. Run `qemu-ga-x86_64.msi` from the VirtIO drivers ISO.
    2. Install remaining VirtIO drivers:
       1. Run `virtio-win-gt-x64.msi` from the VirtIO drivers ISO.
    3. Configure Windows settings as needed (e.g., network, updates).
