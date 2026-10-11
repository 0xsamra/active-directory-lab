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
Nid Mao a<br>> Specify virtual hardware<br>> Virtual machine name and operating system<br><!-- End of picture text -->



# **Network Design** 

The VM has two network adapters, each with a different job: 

**Adapter Type Purpose** Adapter Gives the server internet access for Windows activation and NAT 1 updates Adapter Internal Network Private network where the server and the client will talk to each 2 (adlab) other, isolated from my real home network 

Keeping domain traffic on an isolated internal network means any attack simulation I run later cannot reach my real network or other devices. 



<!-- Start of picture text -->
Network<br>@ Enable Network Adapter<br><!-- End of picture text -->



<!-- Start of picture text -->
Network<br>Adapter?  Adapter2 Adapter3 Adapter4<br>@ Enable Network Adapter<br>Attached to Internal Network v<br>co ES The way this virtual adapter is attachedt<br>Adapter Type Intel PRO/1000 MT Desktop (82540EM) v<br>Promiscuous Mode Deny V<br>MAC Address 080027190198 °<br>@ Virtual Cable Connected<br><!-- End of picture text -->

# **Final VM Summary** 



<!-- Start of picture text -->
Gos Gg HG AF<br>General DD Preview<br>Name AD-DCOt<br>IE system<br>Base Memory: 4096 MB AD-DCO1<br>Boot Order: Floppy, Optical, Hard Disk<br>Acceleration: Nested Paging, Hyper-V Paravrtualization<br>D display<br>Recording Disabled<br>Storage<br>SATA Port1: (Optical Drive] 26100.32230.260111-0550.t release svc.refresh SERVER EVAL x64FRE_en-usiso (7.59 GB)<br>Problems Faced Audio<br>Controller: Intel<br>HD Audio<br>ISO download timeout. IB Network  The Microsoft Evaluation Center page loaded and registration worked,<br>but the file download failed with ERR_CONNECTION_TIMED_OUT from the download server  AdapterAdapter1: Intel2:_Intel PRO/1000PRO/1000 MT Desktop (Internal(NAT) Network, ‘adlab})<br><!-- End of picture text -->

(software-static.download.prss.microsoft.com). This was a connection issue between my network and Microsoft's file server, not a problem with the page itself. 

**Fix:** Retried the download using a different network route until it succeeded. The Windows Server and Windows 11 ISOs together took about 4 hours to download. 

**Disk size default.** VirtualBox suggested a disk size of about 27 GB, which is too small for Windows Server plus updates and logs. I increased it to 50 GB. 

# **Next Step** 

Boot the VM from the ISO and install Windows Server, then rename the machine to DC01 and configure a static IP on the internal network adapter. See 02-domain-setup.md (coming next). 

