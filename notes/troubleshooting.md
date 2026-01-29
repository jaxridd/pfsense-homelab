\# Troubleshooting Log



This document shows the issues I encountered during the pfSense VirtualBox homelab setup and the steps taken to diagnose and resolve them.



---



\## 1. pfSense GUI Unreachable from LAN Client



\*\*Errors\*\*

\- Ubuntu client could not access the pfSense web GUI at `https://192.168.1.1`

\- ICMP traffic from the client to the pfSense LAN IP failed



\*\*Analysis\*\*

\- Verified pfSense was running and LAN IP was configured

\- Determined the issue was not firewall rules (default allow LAN rule present)

\- Suspected a DHCP issue rather than a routing issue



\*\*Resolution\*\*

\- Saw that the Ubuntu client was not receiving an IPv4 address

\- Identified that the VirtualBox host-only network was not correctly passing DHCP traffic

\- Deleted and recreated the VirtualBox host-only network

\- Disabled VirtualBox DHCP to ensure pfSense was the authoritative DHCP server

\- Reattached VM network adapters and rebooted both VMs



\*\*Result\*\*

\- Ubuntu client successfully obtained a DHCP lease

\- LAN connectivity and GUI access were back.



---



\## 2. pfSense PHP / CLI Errors After Reconfiguration



\*\*Errors\*\*

\- pfSense console displayed PHP fatal errors related to `notices.inc`

\- Menu options produced errors or failed to load

\- System became unstable after multiple reconfiguration attempts



\*\*Analysis\*\*

\- Identified corrupted PHP notice databases and inconsistent configuration state

\- Determined that further recovery attempts would be inefficient



\*\*Resolution\*\*

\- Chose to perform a clean pfSense reinstall

\- Recreated the VM with known-good VirtualBox network settings

\- Reinstalled pfSense using default options

\- Reconfigured WAN/LAN interfaces and DHCP cleanly



\*\*Result\*\*

\- pfSense booted cleanly with no PHP errors

\- System stability restored



---



\## 3. Boot Loop Into Installer After Reboot



\*\*Errors\*\*

\- pfSense repeatedly returned to the installer after reboot

\- Installation appeared to restart automatically



\*\*Analysis\*\*

\- Determined the installer ISO was still attached to the VM

\- VirtualBox was booting from the optical drive instead of disk



\*\*Resolution\*\*

\- Powered off the pfSense VM

\- Removed the installer ISO from VirtualBox storage settings

\- Verified boot order prioritized the virtual hard disk



\*\*Result\*\*

\- pfSense booted normally into the installed system



---



\## What I learned



\- VirtualBox host-only networking can silently break and requires manual cleanup

\- pfSense issues often originate from virtualization layers, not firewall configuration

\- Verifying DHCP assignment is critical before troubleshooting routing or firewall rules

\- Knowing when to rebuild a system is an important operational skill

\- Systematic diagnosis is more effective than repeated resets

