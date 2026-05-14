# AZ-104 Quick Reference — Key Limits, Comparisons & Facts

> Consolidated reference of all critical numbers, limits, and comparisons for exam day.

---

## 📊 Critical Azure Limits

### Identity & Governance

| Resource | Limit |
|----------|-------|
| Custom RBAC roles per tenant | 5,000 |
| Administrative Units per tenant | 5,000 |
| Tags per resource/RG/subscription | 50 |
| Tag name max length | 512 characters |
| Tag value max length | 256 characters |
| Management group depth | 6 levels (excl. root + subscription) |
| Management groups per tenant | 10,000 |
| Resource groups per subscription | 980 |
| Deleted users recoverable window | 30 days |

### Storage

| Resource | Limit |
|----------|-------|
| Storage account name | 3–24 chars, lowercase + numbers |
| Max storage accounts per region | 250 |
| Max blob size (block blob) | ~190.7 TB |
| Max file size (Azure Files) | 4 TiB |
| Max file share size | 100 TiB |
| Max snapshots per file share | 200 |
| Stored access policies per container | 5 |
| User Delegation SAS max validity | 7 days |
| Blob metadata max size | 8 KB |
| Soft delete retention range | 1–365 days |
| VNet rules per storage account | 200 |
| IP rules per storage account | 200 |

### Compute

| Resource | Limit |
|----------|-------|
| Availability Set — max Fault Domains | 3 |
| Availability Set — max Update Domains | 20 |
| VMs per region per subscription | 25,000 |
| App Service deployment slots — Standard | 5 |
| App Service deployment slots — Premium | 20 |
| ACI max CPU per container | 4 cores |
| ACI max memory per container | 16 GB |

### Networking

| Resource | Limit |
|----------|-------|
| VNets per subscription per region | 1,000 |
| Reserved IPs per subnet | 5 |
| Smallest subnet | /29 (3 usable IPs) |
| NSG rule priority range | 100–4096 |
| AzureBastionSubnet minimum size | /26 |
| AzureFirewallSubnet minimum size | /26 |
| GatewaySubnet recommended size | /27 |
| VPN Gateway creation time | 30–45 minutes |

### Monitoring & Backup

| Resource | Limit |
|----------|-------|
| Metrics retention | 93 days |
| Log Analytics default retention | 30 days |
| Log Analytics max retention | 730 days (2 years) |
| Activity Log default retention | 90 days |
| Backup soft delete retention | 14 days |
| Azure SQL PITR range | 1–35 days |
| Azure SQL LTR max retention | 10 years |
| VM backup instant restore | 1–5 days |

---

## 🔄 Key Comparisons

### SLA Comparison

| Configuration | SLA |
|--------------|-----|
| Single VM (Premium SSD) | 99.9% |
| Availability Set | 99.95% |
| Availability Zones | 99.99% |

### Storage Redundancy

| Option | Copies | Regions | Read Secondary | Durability |
|--------|--------|---------|---------------|------------|
| LRS | 3 | 1 DC | ❌ | 11 nines |
| ZRS | 3 | 1 region, 3 zones | ❌ | 12 nines |
| GRS | 6 | 2 regions | ❌ | 16 nines |
| RA-GRS | 6 | 2 regions | ✅ | 16 nines |
| GZRS | 6 | 2 regions (3 zones primary) | ❌ | 16 nines |
| RA-GZRS | 6 | 2 regions (3 zones primary) | ✅ | 16 nines |

### Access Tiers

| Tier | Min Duration | Storage Cost | Access Cost | Online? |
|------|-------------|-------------|-------------|---------|
| Hot | None | Highest | Lowest | ✅ |
| Cool | 30 days | Lower | Higher | ✅ |
| Cold | 90 days | Lower still | Higher still | ✅ |
| Archive | 180 days | Lowest | Highest | ❌ Offline |

### Load Balancer Decision Matrix

| Need | Solution |
|------|----------|
| TCP/UDP (Layer 4) | Azure Load Balancer |
| HTTP/HTTPS (Layer 7) — single region | Application Gateway |
| HTTP/HTTPS (Layer 7) — global + CDN | Azure Front Door |
| DNS-based global routing | Traffic Manager |

### Lock Types

| Lock | Read | Update | Delete |
|------|------|--------|--------|
| ReadOnly | ✅ | ❌ | ❌ |
| Delete | ✅ | ✅ | ❌ |

### NSG Default Rules (Inbound)

| Priority | Name | Source → Dest | Action |
|----------|------|--------------|--------|
| 65000 | AllowVnetInBound | VNet → VNet | Allow |
| 65001 | AllowAzureLBInBound | AzureLB → Any | Allow |
| 65500 | DenyAllInBound | Any → Any | Deny |

### NSG Default Rules (Outbound)

| Priority | Name | Source → Dest | Action |
|----------|------|--------------|--------|
| 65000 | AllowVnetOutBound | VNet → VNet | Allow |
| 65001 | AllowInternetOutBound | Any → Internet | Allow |
| 65500 | DenyAllOutBound | Any → Any | Deny |

### Disk Types

| Type | Max IOPS | Max Throughput | Use Case |
|------|---------|---------------|----------|
| Standard HDD | 500 | 60 MB/s | Backup, non-critical |
| Standard SSD | 6,000 | 750 MB/s | Web servers, dev/test |
| Premium SSD | 20,000 | 900 MB/s | Production, databases |
| Ultra Disk | 160,000 | 4,000 MB/s | SAP HANA, heavy workloads |

### Container Services

| Feature | ACI | Container Apps | AKS |
|---------|-----|---------------|-----|
| Complexity | Simple | Medium | Complex |
| Scaling | Manual | Auto (to zero) | Auto |
| Orchestration | None | Managed K8s | Full K8s |
| Use case | Simple tasks | Microservices | Full control |

### App Service Plan Tiers

| Tier | Dedicated | Auto-scale | Slots | VNet Integration |
|------|-----------|-----------|-------|-----------------|
| Free/Shared | ❌ | ❌ | 0 | ❌ |
| Basic | ✅ | ❌ | 0 | ❌ |
| Standard | ✅ | ✅ | 5 | ✅ |
| Premium | ✅ | ✅ | 20 | ✅ |
| Isolated (ASE) | ✅ | ✅ | 20 | ✅ (full) |

---

## ⚡ Key "Gotchas" for the Exam

1. **Tags do NOT inherit** from RG to resources — use Azure Policy (Modify effect)
2. **Stopped ≠ Deallocated** — OS shutdown still bills; deallocate to stop billing
3. **NotActions ≠ Deny** — it only subtracts from Actions
4. **VNet peering is NOT transitive** — A↔B, B↔C ≠ A↔C
5. **Standard SKU public IPs are secure by default** — all inbound blocked
6. **Memory is NOT a platform metric** — needs Azure Monitor Agent
7. **Archive tier is offline** — must rehydrate before access
8. **Infrastructure encryption** must be set at storage account creation
9. **Vault redundancy** must be set before first backup
10. **Complete deployment mode** deletes resources not in the template
11. **Generalized VMs cannot be restarted** — they're permanently unusable
12. **Deny assignments** are only created by Blueprints/managed apps
13. **Soft delete = 14 days** for backup data; enabled by default
14. **Activity Log = 90 days** default retention
15. **5 reserved IPs per subnet** — plan address space accordingly
16. **Dynamic groups require Azure AD Premium P1**
17. **Admin SSPR always requires 2 methods** — no security questions
18. **Subscription transfer** removes all RBAC, policies, and locks
19. **Slot settings (sticky)** do NOT swap — everything else does
20. **User Delegation SAS max = 7 days** — most secure SAS type
