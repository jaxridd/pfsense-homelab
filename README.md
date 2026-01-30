# pfSense Firewall Homelab (VirtualBox)

## Overview

This README documents the design, deployment, and troubleshooting of a pfSense firewall in a VirtualBox homelab environment. The lab simulates a small internal network protected by a stateful firewall, with client systems receiving network access via DHCP and routing traffic through pfSense to the internet.

The primary goal of this project was to gain experience with networking and security fundamentals, including firewall policy design, NAT, DHCP, interface segmentation, and systematic troubleshooting of Layer 2 and Layer 3 connectivity issues.

This lab intentionally mirrors scenarios commonly encountered IT, network administration, and security operations roles.

---

## Lab Architecture

### Components

- **Firewall:** pfSense Community Edition  
- **Client:** Ubuntu Desktop  
- **Hypervisor:** Oracle VirtualBox  

### Network Design

- **WAN:** VirtualBox NAT (internet access)  
- **LAN:** VirtualBox Host-Only Network 

### Logical Traffic Flow

1. The client VM requests an IP address via DHCP  
2. pfSense assigns an IP address and default gateway on the LAN interface  
3. Client traffic is routed to pfSense at `192.168.1.1`  
4. pfSense performs outbound NAT on the WAN interface  
5. Traffic is forwarded to the internet via VirtualBox NAT  
6. Return traffic is statefully tracked and delivered back to the client  

This flow demonstrates correct DHCP operation, routing, NAT translation, and stateful firewall behavior.

---

## Network Configuration

### pfSense Interfaces

| Interface | Assignment | Purpose |
|---------|------------|---------|
| em0 | WAN | Internet access via VirtualBox NAT |
| em1 | LAN | Internal network (192.168.1.0/24) |

- **LAN IP Address:** `192.168.1.1/24`  
- **DHCP Server:** Enabled on LAN (pfSense only)  
- **Internal Domain:** `home.arpa`  

---

## Firewall Policy

### LAN Firewall Rules

The following firewall rules were configured and verified on the LAN interface:

- **Anti-lockout Rule**  
  - Allows administrative access to the pfSense webConfigurator (ports 80/443)

- **Default allow LAN to any (IPv4)**  
  - Permits internal clients to access external networks and services

- **Default allow LAN IPv6 to any**  
  - Retained for completeness and future IPv6 support

These rules establish a **permissive internal security model** appropriate for a trusted LAN while preserving pfSense’s default **deny-by-default posture on the WAN interface**.

### NAT Configuration

- **Outbound NAT Mode:** Automatic outbound NAT rule generation  

This configuration enables internal LAN traffic to be translated and routed through the WAN interface without manual rule creation.

---

## Client Configuration (Ubuntu Desktop)

- **Network Adapter:** Host-Only Adapter (`vboxnet0`)  
- **IP Address:** Dynamically assigned via pfSense DHCP  
- **Default Gateway:** `192.168.1.1`  

The client VM represents a standard internal workstation behind the firewall.

---

## Validation and Testing

The following tests were performed to validate the environment:

- Successful DHCP lease assignment from pfSense  
- ICMP connectivity to pfSense (`192.168.1.1`)  
- ICMP connectivity to external hosts (`8.8.8.8`)  
- HTTPS access to the pfSense web management interface  

These tests confirm:

- Correct DHCP operation  
- Proper routing and NAT translation  
- Functional firewall rule evaluation  
- End-to-end network connectivity  

---

## Troubleshooting Highlights

Several real-world issues were encountered and resolved during setup:

- **Conflicting DHCP servers**  
  - VirtualBox DHCP conflicted with pfSense DHCP  
  - Resolved by disabling VirtualBox DHCP and validating lease assignment from pfSense only  

- **Host-only network misconfiguration**  
  - Prevented LAN reachability between pfSense and the client  
  - Diagnosed by isolating Layer 2 vs Layer 3 connectivity and correcting adapter assignments  

- **pfSense webConfigurator service availability**  
  - Management interface unavailable on initial boot  
  - Resolved by restarting services and validating firewall access rules  

These issues were resolved using systematic troubleshooting techniques, emphasizing root cause analysis over trial-and-error.

---

## Lessons Learned

- Importance of isolating DHCP services to avoid address conflicts  
- How pfSense evaluates firewall rules top-down on a per-interface basis  
- Practical differences between NAT, host-only, and bridged networking modes in virtualized environments  
- Structured approaches to diagnosing firewall, routing, and service-layer issues  

---

## Screenshots

All configuration and validation screenshots are stored in the `screenshots/` directory and correspond to the sections above.

---

## Author

**Jax Riddell**

