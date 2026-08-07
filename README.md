### Lab 2: Guest OS Installation & Static Network Configuration
- **Operating System:** Windows Server Evaluation (Standard)
- **Proxmox VM ID:** 100 (Lab-Client)
- **Virtual Network Card Model:** Intel E1000 (^model=e1000^)
- **Static IPv4 Settings:**
  - IP Address: ^192.168.1.50^
  - Subnet Mask: ^255.255.255.0^
  - Default Gateway: ^192.168.1.254^ (AT&T Gateway)
  - DNS Servers: ^1.1.1.1^ / ^8.8.8.8^
- **Verification:** Successfully validated local network routing via ICMP ping to default gateway (^192.168.1.254^).

### Lab 2: Active Directory & DHCP Core Infrastructure Deployment
- **Active Directory Domain Services (AD DS):**
  - **Deployment Role:** Primary Domain Controller / First Forest Root
  - **New Forrest Root Domain Name:** ^mydomain.local^
  - **Legacy NetBIOS Name:** ^MYDOMAIN^
  - **Core Directory Services Integration:** Integrated Local Domain Name System (DNS) server active on loopback pathway.
 
- **DHCP Server Scope Parameters:**
  - **Scope Name:** ^Lab Scope^
  - **Active IP Lease Distribution Range:** ^192.168.1.100^ to ^192.168.1.150^
  - **Network Subnet Mask Prefix:** ^255.255.255.0^ (CIDR Length: /24)
  - **Scope Options Distributed to Clients:**
    - Router / Default Gateway Address: ^192.168.1.254^ (AT&T Gateway)
    - Primary Domain Controller DNS Resolver Option: ^192.168.1.50^
  - **Scope Activation Status:** Complete & Live 

# Proxmox VE 8 & Windows Server 2012 R2 Enterprise Network Lab

## 📌 Project Overview
An enterprise-grade virtualization infrastructure lab built on Proxmox VE 8 hosting isolated Active Directory domain environments. This repository tracks active network baselining, layer-2/layer-3 core routing configurations, and system troubleshooting artifacts.

## 🛠️ The Technical Challenge (Case Study)
During the initial spin-up of the Windows Server VM (`VM 102`), the virtual machine encountered total packet drops (`PING: transmit failed. General failure`).

### Root Cause Analysis & Resolution Pipeline:
1. **Hypervisor Collision:** A secondary experimental bridge (`vmbr1`) created a kernel-level layer-2 loop, competing for the physical host interfaces. This was forcefully disabled via the host shell (`ip link set vmbr1 down`).
2. **Gateway Cache Misconfiguration:** The system initially pointed traffic toward a dead static gateway (`192.168.1.1`). Network diagnostics via personal endpoint profiling revealed the true upstream AT&T commercial residential gateway was resting at `192.168.1.254`.
3. **MAC Address Filter Lock:** The AT&T residential gateway dropped initial frames from the newly spun virtual bridge adapter due to an unauthenticated interface profile state. This was bypassed by forcefully overwriting and generating a fresh hardware profile identifier straight from the Proxmox cluster runtime terminal (`qm set 102 --net1 model=e1000,bridge=vmbr0`).
4. **Endpoint Resolution:** Fully reconfigured the static IP parameters via the Windows network subsystem utilizing `netsh interface ipv4 set address name="Ethernet 3" static 192.168.1.211 255.255.255.0 192.168.1.254`.

## 📊 Captured Network Evidence (.pcapng)
The verified network data files uploaded to this repository demonstrate successful architectural routing post-mitigation:

* **`gateway-arp-fix.pcapng`**: Captures a targeted ARP cache flush (`arp -d 192.168.1.254`) and the subsequent clean broadcast request/reply sequence verifying successful Layer-2 physical address resolution with the AT&T gateway router.
* **`icmp-internet-routing.pcapng`**: Documents an unfragmented 4-packet ICMP echo request/reply sequence out to public infrastructure DNS endpoints (`8.8.8.8`) maintaining stable 11ms response metrics with absolute 0% packet degradation.
* **`dns-resolution.pcapng`**: Verifies structured application-layer translation handling standard UDP Port 53 iterative A-record query lookups for `google.com`.
