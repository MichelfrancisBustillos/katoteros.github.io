---
title: Fix for Ubuntu 24 Noble LXC Container Not Booting
categories: [Fixes]
tags: [lxc, fixes, bug, ubuntu, proxmox]
---

## Fix for Ubuntu 24 Noble LXC Container Not Booting

- **Issue**: Blank Console when booting an Ubuntu 24 Noble LXC Container in Proxmox.

  Container appears to boot but does not get an IP address.
  Console connects but displays blank screen.
  Unable to SSH into Container

- **Cause**: Unknown as of 01/25/25

- **Resolution**: Enabled 'nesting' under Options > Features.
  Note: Must reboot container after making change for it to apply.
  ![alt text](assets/img/2025-01-23-NobleLXCFix/Blazewake_-_Proxmox_Virtual_Environment_-_Google_C_20250125-1819.png)
