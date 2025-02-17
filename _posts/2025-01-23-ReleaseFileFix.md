---
title: Fix for apt "no longer has a release file" error
categories: [Fixes]
tags: [apt, fixes, bug, ubuntu]
---

## Fixing the APT "no longer has a release file" error

- **Issue**: "no longer has a release file" error when running `apt update`
- **Cause**: Installed Ubuntu release is no longer supported.
- **Quick Fix**:
  This will blank the apt sources file, disabling all custom/non-standard repositories but allowing you to update the OS.
  ```bash
  sudo mv /etc/apt/sources.list ~/
  sudo touch /etc/apt/sources.list
  ```
- **Resolution**: Update to the latest Ubuntu/Debian release with `sudo do-release-upgrade`
