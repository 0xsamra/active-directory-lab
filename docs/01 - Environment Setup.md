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

