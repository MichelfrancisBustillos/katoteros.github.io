---
title: VM Setup Setups
categories: [Guides, VMs]
---

# VM Creation In Proxmox

1. Select Node for VM to run on
2. Enter VM Name (can be changed later)
  ![alt text](/assets/lib/vm_setup/image.png)
3. Choose ISO Location and Image
  ![alt text](/assets/lib/vm_setup/image-2.png)
4. For non-Winters Machines, the default system settings are valid.
5. Check 'Qemu Agent'
![alt text](/assets/lib/vm_setup/image-3.png)
6. Select VHD Storage location. Either local-lvm (lost host storage) or mounted share
7. Choose Disk Size
![alt text](/assets/lib/vm_setup/image-4.png)
8. Choose CPU Cores
![alt text](/assets/lib/vm_setup/image-5.png)
9. Choose default and minimum RAM to allocate and ballooning (automatically allocate more RAM as needed upto the default value.)
*Note: GPU passthrough cannot be enabled with ballooning.*
![alt text](/assets/lib/vm_setup/image-6.png)
10. Adjust Network settings as needed. Defaults are valid.
![alt text](/assets/lib/vm_setup/image-7.png)
