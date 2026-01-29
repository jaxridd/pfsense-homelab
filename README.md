# pfSense Firewall Homelab (VirtualBox)

## Overview
This project documents the deployment and configuration of a **pfSense Community Edition firewall** in a VirtualBox-based homelab environment. The lab simulates a small internal network protected by a stateful firewall, with client systems receiving network access via DHCP and routing through pfSense to the internet.

The goal of this project is to gain hands-on experience with real-world networking and security concepts, including firewall configuration, NAT, DHCP, and troubleshooting Layer 2 and Layer 3 connectivity issues.

---

## Lab Architecture

**Components:**
- **Firewall:** pfSense Community Edition
- **Client:** Ubuntu Desktop
- **Hypervisor:** VirtualBox

**Network Design:**
- **WAN:** VirtualBox NAT (internet access)
- **LAN:** VirtualBox Host-only network (`vboxnet0`)

**Logical Traffic Flow:**
