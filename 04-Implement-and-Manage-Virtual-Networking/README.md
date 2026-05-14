# 4. Implement and Manage Virtual Networking (20–25%)

This section covers configuring virtual networks, securing network access, load balancing, and monitoring network resources.

---

## 📑 Topics

| # | Topic | File |
|---|-------|------|
| 4.1 | Configure Virtual Networks | [4.1-Configure-Virtual-Networks.md](./4.1-Configure-Virtual-Networks.md) |
| 4.2 | Configure Secure Access to Virtual Networks | [4.2-Configure-Secure-Access.md](./4.2-Configure-Secure-Access.md) |
| 4.3 | Configure Load Balancing | [4.3-Configure-Load-Balancing.md](./4.3-Configure-Load-Balancing.md) |
| 4.4 | Monitor Virtual Networking | [4.4-Monitor-Virtual-Networking.md](./4.4-Monitor-Virtual-Networking.md) |
| 4.5 | Practice Questions (10 scenarios) | [4.5-Practice-Questions.md](./4.5-Practice-Questions.md) |

---

## 🔑 Key Sub-Topics

### 4.1 Configure Virtual Networks
- VNet fundamentals (address space, subnets, reserved IPs)
- Public and private IP addresses (SKUs, allocation methods)
- DNS configuration (Azure DNS, Private DNS zones, auto-registration)
- VNet peering (regional, global, non-transitive)
- User-Defined Routes (UDR) and IP forwarding
- Service endpoints vs Private endpoints

### 4.2 Configure Secure Access to Virtual Networks
- Network Security Groups (NSGs) — rules, defaults, priority
- Application Security Groups (ASGs)
- Service Tags
- Azure Bastion (secure RDP/SSH without public IPs)
- VPN Gateway (S2S, P2S, VNet-to-VNet)
- ExpressRoute
- Azure Firewall (rule types, SKUs)

### 4.3 Configure Load Balancing
- Azure Load Balancer (Layer 4 — TCP/UDP, SKUs, health probes)
- Application Gateway (Layer 7 — HTTP/HTTPS, WAF, URL routing)
- Azure Front Door (global Layer 7, CDN, WAF)
- Traffic Manager (DNS-based routing methods)
- Choosing the right load balancer

### 4.4 Monitor Virtual Networking
- Azure Network Watcher tools
- IP Flow Verify, Next Hop, Effective Security Rules
- Connection Troubleshoot and Connection Monitor
- NSG Flow Logs (versions, storage)
- Traffic Analytics
- Packet Capture
- Network Topology

---

> **Key exam areas**: NSG rules and defaults, VNet peering (non-transitive), Azure Bastion, Load Balancer vs Application Gateway, Traffic Manager routing methods, and Network Watcher diagnostic tools.
