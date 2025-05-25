# Three-Layer Hierarchical Model in Cisco

This repository contains the configuration of a network in Cisco Packet Tracer following the **Three-Layer Hierarchical Model in Cisco**. Below are the details of the configuration steps:

## Network Diagram

![Three-Layer Hierarchical Model](figure\model.jpg)

## Network Information

- **Network Address:** 10.11.81.0/23

---

## Subnet Information

| Subnet | Required Hosts | Network Address | First Address | Broadcast Address |
| ------ | -------------- | --------------- | ------------- | ----------------- |
| VLAN10 | 40             | 10.11.81.64/26  | 10.11.81.65   | 10.11.81.127      |
| VLAN20 | 50             | 10.11.81.0/26   | 10.11.81.1    | 10.11.81.63       |
| VLAN60 | 10             | 10.11.81.160/28 | 10.11.81.161  | 10.11.81.175      |
| VLAN61 | 10             | 10.11.81.176/28 | 10.11.81.177  | 10.11.81.191      |
| VLAN80 | 20             | 10.11.81.128/27 | 10.11.81.129  | 10.11.81.159      |
| WAN1   | 2              | 10.11.81.192/30 | 10.11.81.193  | 10.11.81.195      |
| WAN2   | 2              | 10.11.81.196/30 | 10.11.81.197  | 10.11.81.199      |

## Device IP Addressing Information

| Device          | Interface | IP Address     | Subnet Mask     | Default Gateway |
| --------------- | --------- | -------------- | --------------- | --------------- |
| **R1**          | G0/0/0    | 10.11.81.193   | 255.255.255.252 | N/A             |
|                 | G0/0/1    | 10.11.81.197   | 255.255.255.252 | N/A             |
|                 | Se0/1/0   | 81.200.200.200 | 255.255.255.0   | N/A             |
| **S11**         | Gig1/0/1  | N/A            | N/A             | N/A             |
|                 | Gig1/0/2  | N/A            | N/A             | N/A             |
|                 | Gig1/0/3  | N/A            | N/A             | N/A             |
|                 | VLAN10    | 10.11.81.65    | 255.255.255.192 | N/A             |
|                 | VLAN20    | 10.11.81.1     | 255.255.255.192 | N/A             |
|                 | VLAN60    | 10.11.81.161   | 255.255.255.240 | N/A             |
|                 | VLAN61    | 10.11.81.177   | 255.255.255.240 | N/A             |
|                 | G1/0/24   | 10.11.81.194   | 255.255.255.252 | N/A             |
| **S12**         | Gig1/0/1  | N/A            | N/A             | N/A             |
|                 | Gig1/0/2  | N/A            | N/A             | N/A             |
|                 | Gig1/0/3  | N/A            | N/A             | N/A             |
|                 | VLAN10    | 10.11.81.66    | 255.255.255.192 | N/A             |
|                 | VLAN20    | 10.11.81.2     | 255.255.255.192 | N/A             |
|                 | VLAN60    | 10.11.81.162   | 255.255.255.240 | N/A             |
|                 | VLAN61    | 10.11.81.178   | 255.255.255.240 | N/A             |
|                 | Gig1/0/24 | 10.11.81.198   | 255.255.255.252 | N/A             |
| **PC0**         | NIC       | DHCP           | DHCP            | 10.11.81.126    |
| **PC1**         | NIC       | DHCP           | DHCP            | 10.11.81.126    |
| **PC2**         | NIC       | DHCP           | DHCP            | 10.11.81.62     |
| **PC3**         | NIC       | DHCP           | DHCP            | 10.11.81.62     |
| **DHCP/DNS**    | Fa0       | 10.11.81.170   | 255.255.255.240 | 10.11.81.174    |
| **File Server** | Fa0       | 10.11.81.171   | 255.255.255.240 | 10.11.81.174    |
| **Web Server**  | Fa0       | 10.11.81.181   | 255.255.255.240 | 10.11.81.190    |
| **S21**         | VLAN10    | 10.11.81.67    | 255.255.255.192 | 10.11.81.126    |
| **S22**         | VLAN20    | 10.11.81.3     | 255.255.255.192 | 10.11.81.62     |
| **S23**         | VLAN60    | 10.11.81.163   | 255.255.255.240 | 10.11.81.174    |
| **S24**         | VLAN61    | 10.11.81.179   | 255.255.255.240 | 10.11.81.190    |

---

## Part 2: Configuring EtherChannel

EtherChannel is a technology that combines multiple physical links into a single logical link to:

- Increase bandwidth
- Improve redundancy
- Optimize load balancing across links

### Steps:

1. Access the interfaces to be configured (e.g., G1/0/1-3) and switch them to trunk mode.
2. Group the interfaces into a single channel group using `channel-group` with `mode auto` and `mode desirable`.
3. Use the PAgP (Port Aggregation Protocol), a Cisco proprietary protocol, with the following modes:
   - **auto:** Passive mode, waiting for PAgP requests.
   - **desirable:** Active mode, sending PAgP requests.
   - **on:** Enables EtherChannel without PAgP or LACP (manual configuration).

For EtherChannel to work correctly:

- One switch port must be in `desirable` mode, and the other in `auto` mode, or both in `desirable` mode.

---

## Part 3: Configuring VLAN and Trunking

### VLAN Configuration:

- On **S11** and **S12**, create 5 VLANs: VLAN10, VLAN20, VLAN60, VLAN61, and VLAN80.
- Assign IP addresses to each VLAN interface for inter-VLAN communication.

### Trunking:

- Configure trunk mode on ports connecting Layer 2 switches to Layer 3 switches.
- Layer 3 switches handle inter-VLAN routing.

### Additional Steps:

- On Layer 2 switches:
  - Assign VLANs to specific ports (e.g., S21 → VLAN10, S22 → VLAN20).
  - Configure access mode for end device ports and trunk mode for ports connecting to Layer 3 switches.
  - Disable DTP using `switchport nonegotiate`.
  - Shut down unused ports with the `shutdown` command.

---

## Part 4: Configuring PortFast

PortFast is a feature of Spanning Tree Protocol (STP) that reduces the time for a switch port to transition to the forwarding state.

### Steps:

- Enable PortFast on access ports using the command `spanning-tree portfast`.

---

## Part 5: Configuring Port Security

Port Security is a Layer 2 security mechanism on Cisco switches.

### Features:

- Limits the number of MAC addresses learned on a port.
- Allows only specific MAC addresses to connect.
- Takes action when a violation occurs.

### Steps:

1. Enable port security with `switchport port-security`.
2. Set the maximum number of MAC addresses with `switchport port-security maximum 2`.
3. Use `switchport port-security mac-address sticky` to learn and save MAC addresses dynamically.
4. Configure violation mode with `switchport port-security violation shutdown`.

---

## Part 6: Configuring HSRP

HSRP (Hot Standby Router Protocol) provides high availability by creating a virtual gateway for devices in the LAN.

### Steps:

1. Disable switchport mode on **S11** and **S12** to enable routing.
2. Assign IP addresses and configure a virtual IP as the last address in the subnet.
3. Set priorities to determine the active and standby devices:
   - VLAN10 and VLAN60: **S11** as active (higher priority).
   - VLAN20 and VLAN61: **S12** as active (higher priority).
4. Enable preemption to allow standby devices to take over when the active device fails.

---

## Part 7: Configuring Routing

### Steps:

1. On **R1**, configure a default route with the exit interface `s0/1/0` for internet traffic.
2. Configure static routes for VLANs on **R1** with metrics to prioritize traffic:
   - VLAN10 and VLAN60: Prefer **S11**.
   - VLAN20 and VLAN61: Prefer **S12**.
3. Enable routing on Layer 3 switches with the `ip routing` command.

---

## Part 8: Configuring DHCP

### Steps:

1. Assign static IPs to PCs to ensure connectivity with the DNS/DHCP server.
2. Configure DHCP pools on the server.
3. Set **S11** and **S12** as relay agents using the `ip helper-address <DHCP server IP>` command on VLAN interfaces.

---

## Part 9: Configuring NAT

### Steps:

1. On **R1**, create an access list to allow VLANs 10, 20, 60, and 61 for NAT.
2. Configure NAT overload on interface `s0/1/0` for internet access.
3. Set up static NAT to map the Web Server's private IP (10.11.81.180) to a public IP (20.81.1.8).

---

## Part 10: Configuring ACL

### Steps:

1. On routers and Layer 3 switches, create extended ACLs:
   - Allow VLAN10 and deny others for SSH (Port 22).
   - Apply the ACL to `line vty 0 4`.
2. Deny traffic between VLAN10 and VLAN20 while permitting other traffic.
3. Restrict internet access to VLAN60 and VLAN61 for HTTP (Port 80) and HTTPS (Port 443).

---

This repository provides a comprehensive guide to configuring a network using Cisco Packet Tracer, following best practices for the Three-Layer Hierarchical Model.
