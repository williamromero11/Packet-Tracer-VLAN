# VLAN Segmentation and Inter-VLAN Routing (Cisco Packet Tracer)

## Overview

This project demonstrates the configuration of a segmented enterprise-style network using Cisco Packet Tracer. Multiple VLANs were created on a Cisco switch and connected to a router using IEEE 802.1Q trunking to enable inter-VLAN routing through router subinterfaces (Router-on-a-Stick architecture).

The goal of this project was to simulate how organizations segment networks to improve security, manage broadcast domains, and control communication between departments.

---

## Network Architecture

The network consists of:

- 1 Cisco Router
- 1 Cisco Switch
- 6 Workstations
- 3 VLANs

Each VLAN represents a separate logical network with its own subnet.

| VLAN | Name | Subnet | Devices |
|-----|-----|-----|-----|
| VLAN 10 | Zone10 | 192.168.10.0/28 | PC1, PC2 |
| VLAN 20 | Zone20 | 192.168.20.0/28 | PC3, PC4 |
| VLAN 30 | Zone30 | 192.168.30.0/28 | PC5, PC6 |

Inter-VLAN communication is handled by router subinterfaces using 802.1Q tagging.

![Network Topology](VLAN/Screenshot%202026-03-09%20093509.png)
---

## Technologies Used

- Cisco Packet Tracer
- Cisco IOS CLI
- IEEE 802.1Q VLAN Tagging
- Router-on-a-Stick Architecture
- Subnetting (/28 networks)
- ICMP Connectivity Testing

---
![VLAN Configuration](VLAN/Screenshot%202026-03-09%20093607.png)
## Key Networking Concepts Demonstrated

### VLAN Segmentation
Each department was placed in a separate VLAN to isolate broadcast domains and improve network organization.

### Trunking
A trunk link was configured between the switch and router to carry traffic from multiple VLANs over a single physical interface.

### Inter-VLAN Routing
Router subinterfaces were configured to allow communication between VLAN networks.

### Subnetting
Each VLAN was assigned a /28 subnet to efficiently allocate IP addresses.

![VLAN Configuration](VLAN/Screenshot%202026-03-09%20093607.png)
---

## Router Configuration (Example)
