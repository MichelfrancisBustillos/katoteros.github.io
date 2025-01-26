---
title: AppArmor Error Fix
categories: [fixes, ubuntu, apparmor]
---

## AppArmor Error Fix

- **Issue**: `apparmor_parser: Access Denied`
  
  Various errors regarding apparmor, including `access denied` and `You need policy admin privileges to manage profiles.`

- **Cause**: Unknown as of 01/26/2025

- **Quick Fix**: Disable & Remove AppArmor
   ```bash
   sudo systemctl stop apparmor.service
   sudo systemctl disable apparmor.service
   sudo apt purge --autoremove apparmor -y
   sudo reboot
   ```

- **Resolution**: Unknown