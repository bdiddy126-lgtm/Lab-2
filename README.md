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
