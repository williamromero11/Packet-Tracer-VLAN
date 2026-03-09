# Packet-Tracer-VLAN

A Cisco Packet Tracer project demonstrating enterprise-style network segmentation using VLANs and inter-VLAN routing with a Router-on-a-Stick architecture.

---

## Project Overview

This project simulates how organizations segment their networks to:

- **Improve security** – isolate department traffic so that hosts in one VLAN cannot communicate with another without explicit routing policy.
- **Reduce broadcast domains** – each VLAN is its own broadcast domain, limiting unnecessary traffic.
- **Control inter-department communication** – routing between VLANs is handled exclusively through a central router, making it easy to apply access-control lists (ACLs) at a single point.

The topology uses **IEEE 802.1Q trunking** between a Cisco Catalyst switch and a Cisco router. The router performs **inter-VLAN routing** through logical subinterfaces (the *Router-on-a-Stick* model).

---

## Network Topology

```
  [PC-HR]        [PC-IT]       [PC-Finance]    [PC-Mgmt]
  VLAN 10        VLAN 20         VLAN 30        VLAN 99
     |               |               |               |
  Fa0/1           Fa0/2           Fa0/3           Fa0/4
     |               |               |               |
 +---+---------------+---------------+---------------+---+
 |                  Cisco Switch (SW1)                    |
 |                  Trunk port: Gi0/1 (802.1Q)            |
 +---------------------------------------------------+----+
                                                     |
                                                  Gi0/1
                                          +--------+--------+
                                          |  Cisco Router   |
                                          |    (R1)         |
                                          | Gi0/1.10  VLAN10|
                                          | Gi0/1.20  VLAN20|
                                          | Gi0/1.30  VLAN30|
                                          | Gi0/1.99  VLAN99|
                                          +-----------------+
```

---

## VLAN Design

| VLAN ID | Name       | Subnet            | Gateway          | Department   |
|---------|------------|-------------------|------------------|--------------|
| 10      | HR         | 192.168.10.0/24   | 192.168.10.1     | Human Resources |
| 20      | IT         | 192.168.20.0/24   | 192.168.20.1     | Information Technology |
| 30      | Finance    | 192.168.30.0/24   | 192.168.30.1     | Finance      |
| 99      | Management | 192.168.99.0/24   | 192.168.99.1     | Network Management |

---

## End-Device IP Addressing

| Device      | VLAN | IP Address       | Subnet Mask     | Default Gateway  |
|-------------|------|------------------|-----------------|------------------|
| PC-HR       | 10   | 192.168.10.10    | 255.255.255.0   | 192.168.10.1     |
| PC-IT       | 20   | 192.168.20.10    | 255.255.255.0   | 192.168.20.1     |
| PC-Finance  | 30   | 192.168.30.10    | 255.255.255.0   | 192.168.30.1     |
| PC-Mgmt     | 99   | 192.168.99.10    | 255.255.255.0   | 192.168.99.1     |

---

## Configuration Files

| File | Description |
|------|-------------|
| [`configs/switch-config.txt`](configs/switch-config.txt) | Cisco switch: VLAN creation, access ports, and 802.1Q trunk |
| [`configs/router-config.txt`](configs/router-config.txt) | Cisco router: subinterfaces and 802.1Q encapsulation for inter-VLAN routing |

---

## Configuration Steps

### Switch (SW1)

1. Create VLANs 10, 20, 30, and 99 in the VLAN database.
2. Assign switch access ports to their respective VLANs.
3. Configure the uplink port toward the router as an **802.1Q trunk**, allowing all VLANs.

### Router (R1) — Router-on-a-Stick

1. Enable the physical interface connected to the switch.
2. Create a **subinterface** for each VLAN on that physical interface.
3. On each subinterface, configure:
   - `encapsulation dot1Q <vlan-id>` — tags frames with the correct VLAN ID.
   - An IP address that will serve as the **default gateway** for hosts in that VLAN.

---

## How Inter-VLAN Routing Works

When a host in VLAN 10 (HR) wants to reach a host in VLAN 20 (IT):

1. PC-HR sends the packet to its default gateway **192.168.10.1** (Router subinterface Gi0/1.10).
2. The switch tags the frame with **VLAN 10** and forwards it over the trunk link to the router.
3. The router receives the frame on subinterface **Gi0/1.10**, strips the 802.1Q tag, and routes the packet based on its routing table.
4. The router determines that 192.168.20.0/24 is reachable via subinterface **Gi0/1.20**.
5. The router re-tags the frame with **VLAN 20** and sends it back over the trunk to the switch.
6. The switch delivers the frame to PC-IT on port Fa0/2.

---

## Technologies Used

- **Cisco Packet Tracer** – network simulation
- **Cisco IOS** – switch and router operating system
- **IEEE 802.1Q** – VLAN trunking standard
- **Router-on-a-Stick** – inter-VLAN routing using subinterfaces
- **VLAN segmentation** – broadcast domain isolation

---

## Security Considerations

- Hosts in different VLANs cannot communicate directly; all traffic must pass through the router, enabling centralized policy enforcement via ACLs.
- VLAN 99 is reserved for management traffic and is not assigned to any user-facing access port.
- Native VLAN is explicitly set to VLAN 99 on the trunk to reduce the risk of VLAN-hopping attacks.
