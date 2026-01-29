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

---

## Network Configuration

### pfSense Interfaces
| Interface | Assignment | Purpose |
|----------|------------|---------|
| em0 | WAN | Internet access via VirtualBox NAT |
| em1 | LAN | Internal network (192.168.1.0/24) |

- **LAN IP Address:** `192.168.1.1`
- **DHCP Server:** Enabled on LAN (pfSense only)
- **Internal Domain:** `home.arpa`

---

## Firewall Rules

### LAN Rules
The following firewall rules were configured and verified on the LAN interface:

- **Anti-lockout Rule**
  - Allows management access to the pfSense web interface (ports 80/443)
- **Default allow LAN to any (IPv4)**
  - Permits internal clients to access external networks and services
- **Default allow LAN IPv6 to any**
  - Left enabled for completeness and future IPv6 use

These rules establish a permissive internal policy while maintaining pfSense’s default deny posture on the WAN interface.

---

## NAT Configuration

- **Outbound NAT Mode:** Automatic outbound NAT rule generation

This configuration allows internal LAN traffic to be translated and routed through the WAN interface to the internet.

---

## Client Configuration (Ubuntu Desktop)

- Network Adapter: Host-only (`vboxnet0`)
- IP Address: Assigned dynamically via pfSense DHCP
- Default Gateway: `192.168.1.1`

The client VM represents an internal workstation behind the firewall.

---

## Validation and Testing

The following tests were successfully performed to validate the setup:

- DHCP lease assignment from pfSense
- ICMP connectivity to pfSense (`192.168.1.1`)
- ICMP connectivity to external hosts (`8.8.8.8`)
- Access to pfSense web management interface via HTTPS

These tests confirmed correct DHCP operation, routing, NAT functionality, and firewall rule behavior.

---

## Troubleshooting Highlights

Several real-world networking issues were encountered and resolved during setup, including:

- Conflicting DHCP servers between VirtualBox and pfSense
- Host-only network misconfiguration preventing LAN reachability
- pfSense webConfigurator service not running on initial boot

These issues were resolved by isolating Layer 2 versus Layer 3 problems, correcting VirtualBox network settings, and restarting pfSense services.

---

## Lessons Learned

- Importance of isolating DHCP services to avoid network conflicts
- How firewall rules are evaluated top-down on interfaces
- Differences between host-only, NAT, and bridged networking in virtualized environments
- Practical troubleshooting techniques for firewall and routing issues

---

## Next Steps

Planned extensions to this homelab include:
- Deploying **AdGuard Home** for DNS filtering
- Enforcing DNS policies through pfSense
- Adding logging and visibility features
- Expanding documentation with network diagrams and screenshots

---

## Screenshots
Screenshots documenting the configuration and validation steps are located in the `screenshots/` directory.

---

## Author
Jax Riddell
