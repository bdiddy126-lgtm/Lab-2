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
