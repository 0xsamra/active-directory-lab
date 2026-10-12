# active-directory-lab
Active Directory home lab with Sysmon logging and Atomic Red Team attack simulation
# **01 - Environment Setup** 

**Date:** October 11, 2026 **Status:** Complete - VM created, not yet booted 

# **Goal** 

Create the virtual machine that will become the Domain Controller (DC) for the Active Directory lab, and design the network the lab machines will use. 

# **Tools Used** 

**Tool Purpose** VirtualBox Hypervisor that runs the lab VMs Windows Server 2025 Operating system for the Domain Controller (free 180-day Evaluation ISO evaluation from Microsoft Evaluation Center)

# **VM Specifications** 

|**Setting**|**Value**|
|---|---|
|VM name|AD-DC01|
|Operating system      i|i   Windows Server 2025 Standard Evaluation (Desktop Experience)|
|RAM|4096 MB|
|CPUs|2|
|Virtual disk|50 GB, VDI, dynamically allocated|



Unattended install  Skipped (manual installation, to learn each setup step) 



<!-- Start of picture text -->
<img width="596" height="410" alt="scrnshot1" src="https://github.com/user-attachments/assets/f8aebefe-90a4-4595-ab34-7297e157d40f" />

<img width="686" height="410" alt="scrnshot2 harddisk" src="https://github.com/user-attachments/assets/befaa188-37f4-4dcc-bb28-34bceb82b798" />

<img width="683" height="412" alt="virtual harddisk scrnshot3" src="https://github.com/user-attachments/assets/f06a6ea2-a076-4761-80c8-be06070e7cb3" />


<!-- End of picture text -->



# **Network Design** 

The VM has two network adapters, each with a different job: 

**Adapter Type Purpose** Adapter Gives the server internet access for Windows activation and NAT 1 updates Adapter Internal Network Private network where the server and the client will talk to each 2 (adlab) other, isolated from my real home network 

Keeping domain traffic on an isolated internal network means any attack simulation I run later cannot reach my real network or other devices. 



<!-- Start of picture text -->

<img width="431" height="218" alt="adapter1" src="https://github.com/user-attachments/assets/18dbb789-bfb3-4868-ac53-39d0466a2abf" />



<img width="421" height="224" alt="adapter2" src="https://github.com/user-attachments/assets/a4581aad-0304-458d-961f-fe3ba37b8dcf" />

<!-- End of picture text -->


# **Final VM Summary** 



<!-- Start of picture text -->
<img width="527" height="395" alt="main panel" src="https://github.com/user-attachments/assets/757237ba-0342-408d-bce3-2240d4635464" />

<!-- End of picture text -->

(software-static.download.prss.microsoft.com). This was a connection issue between my network and Microsoft's file server, not a problem with the page itself. 

**Fix:** Retried the download using a different network route until it succeeded. The Windows Server and Windows 11 ISOs together took about 4 hours to download. 

**Disk size default.** VirtualBox suggested a disk size of about 27 GB, which is too small for Windows Server plus updates and logs. I increased it to 50 GB. 

# **Next Step** 

Boot the VM from the ISO and install Windows Server, then rename the machine to DC01 and configure a static IP on the internal network adapter. See 02-domain-setup.md (coming next). 

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
