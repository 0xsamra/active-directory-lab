# 02 - Windows Server Installation

**Date:** October 12, 2026
**Status:** Complete

## Goal

Install Windows Server 2025 on the `AD-DC01` virtual machine so it can later be promoted to a Domain Controller.

## Edition Chosen

**Windows Server 2025 Standard Evaluation (Desktop Experience)**

I chose Desktop Experience because the graphical interface makes Active Directory setup easier to learn and to document. The non-GUI edition is command-line only.

## Steps Taken

**1. Booted the VM from the Windows Server ISO** and pressed a key at the "Press any key to boot from CD or DVD" prompt.

**2. Selected the edition** (Standard Evaluation, Desktop Experience).

<img width="1024" height="768" alt="edition selection" src="https://github.com/user-attachments/assets/007bc394-5d2f-469e-9706-abae7ca484f5" />

**3. Chose Custom: Install Windows only** and removed leftover partitions from the earlier interrupted attempt, so the installer had a single clean 50 GB unallocated space.

<img width="1024" height="768" alt="disk selection" src="https://github.com/user-attachments/assets/aecf479e-7850-43ca-9615-7c2bcc5a46e1" />


**4. Installation ran** and the VM restarted automatically. I did not press a key at the second boot prompt, so it booted from the new install.

<img width="1024" height="768" alt="installing process server" src="https://github.com/user-attachments/assets/50111318-ec8f-49e7-9553-cd2ab1d30ade" />


**5. Set the Administrator password** and logged in using Ctrl+Alt+Del (VirtualBox: Input > Keyboard > Insert Ctrl-Alt-Del).

**6. Server Manager opened** automatically after the first login.

<img width="1024" height="768" alt="server manager" src="https://github.com/user-attachments/assets/a783193e-dd8f-48c3-b327-13de1585dc08" />


**7. Verified internet access** by loading a website in Edge inside the VM. This confirms the NAT adapter works, which is needed for activating the evaluation license within 10 days.

<img width="1024" height="768" alt="website loaded" src="https://github.com/user-attachments/assets/7c0041ab-95e4-491e-a8be-5e87883d9702" />


## Problems Faced and Fixes

**1. VM booted to "An operating system wasn't found"**
- **Cause:** I missed the "Press any key to boot from CD or DVD" prompt, so the VM tried to boot from the empty disk.
- **Fix:** Restarted the VM (Ctrl+Alt+Del) and pressed a key as soon as the prompt appeared.

**2. Interrupted first install**
- **Cause:** I stopped the first install at 11% because I had not taken screenshots yet.
- **Fix:** Restarted the install and deleted the leftover partitions on the disk selection screen so the new install started clean.

**3. VirtualBox error VERR_DISK_FULL (BLKCACHE_IOERR)**
- **Cause:** The host drive (C:) ran out of free space. The VM disk files were stored on C:, and the Kali, Wazuh and AD-DC01 disks together took roughly 40 GB.
- **Fix:** Moved the VM storage to drive D: and set D: as the default machine folder in VirtualBox (File > Preferences > General > Default Machine Folder).

**4. VM stuck while saving its state**
- **Cause:** Not enough free disk space on C: to write the saved state.
- **Fix:** Freed up disk space and powered the VM off before restarting the install.

**5. VirtualBox error VERR_FILE_NOT_FOUND**
- **Cause:** The VM settings still pointed to the old virtual disk path on C: after the files were moved.
- **Fix:** Created a new 50 GB dynamically allocated virtual disk on D: and attached it to AD-DC01, then reinstalled.

## Result

Windows Server 2025 is installed on `AD-DC01`, I can log in as Administrator, Server Manager loads, and the VM has internet access through the NAT adapter.

## Lessons Learned

- Check free disk space on the host drive before creating VMs, since dynamically allocated disks grow as the OS installs.
- Store VM files on a drive with plenty of free space, and set it as VirtualBox's default machine folder.
- Take screenshots at each installer screen as you go, so the install doesn't need repeating.

## Next Step

Rename the server to `DC01` and set a static IP on the internal network adapter. See `03-network-config.md`.
