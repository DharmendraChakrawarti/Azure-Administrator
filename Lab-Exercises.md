# AZ-104 Hands-On Lab Exercises

> Step-by-step lab exercises to practice for each exam domain. Use the Azure portal free tier or sandbox environment.

---

## 🔐 Lab 1 — Identity & Governance

### Lab 1.1: Create Users, Groups, and Administrative Units
1. Create 3 users: `user1@domain`, `user2@domain`, `user3@domain`
2. Create a security group "Developers" with **Dynamic membership** rule: `user.department -eq "Engineering"`
3. Set the department attribute on user1 and user2 to "Engineering"
4. Verify dynamic group membership updated automatically
5. Create an Administrative Unit "Engineering AU"
6. Add the Developers group to the AU
7. Assign the **Helpdesk Administrator** role to user3, scoped to the AU

### Lab 1.2: Configure RBAC and Locks
1. Create a resource group "Lab-RG"
2. Assign **Reader** role to user1 at subscription scope
3. Assign **Contributor** role to user1 at "Lab-RG" scope
4. Verify user1's effective permissions on "Lab-RG" using **Check Access**
5. Create a **Delete lock** on "Lab-RG"
6. Try to delete a resource in "Lab-RG" — verify it's blocked
7. Try to modify a resource — verify it succeeds
8. Remove the lock and apply a **ReadOnly lock** — test again

### Lab 1.3: Azure Policy and Tags
1. Create a custom policy that requires a "Project" tag on all resources
2. Assign the policy to "Lab-RG" with **Deny** effect
3. Try creating a storage account without the tag — verify it's denied
4. Create it with the tag — verify it succeeds
5. Apply the **"Inherit a tag from the resource group"** policy
6. Tag the RG with `Environment=Lab` and create a new resource
7. Verify the resource automatically inherits the tag

---

## 💾 Lab 2 — Storage

### Lab 2.1: Storage Account Security
1. Create a Standard GPv2 storage account
2. Create a blob container and upload a file
3. Generate an **Account SAS** with read-only permission, 1-hour expiry
4. Test accessing the blob using the SAS URL in a browser
5. Create a **Stored Access Policy** on the container
6. Generate a SAS linked to the stored access policy
7. Revoke access by modifying the stored access policy
8. Configure the storage firewall — set default action to Deny
9. Add your IP to the allowed list and verify access works

### Lab 2.2: Access Tiers and Lifecycle Management
1. Upload 3 blobs to a container
2. Set blob tiers: one to Hot, one to Cool, one to Archive
3. Try reading the Archive blob — observe the error
4. Rehydrate the Archive blob to Hot (standard priority)
5. Create a lifecycle management policy that:
   - Moves blobs to Cool after 30 days
   - Moves blobs to Archive after 90 days
   - Deletes blobs after 365 days

### Lab 2.3: Azure Files
1. Create a file share with a 5 GiB quota
2. Create a directory and upload files
3. Mount the file share on your Windows machine using `net use`
4. Create a snapshot of the file share
5. Delete a file, then restore it from the snapshot
6. Enable soft delete with 7-day retention

---

## 🖥️ Lab 3 — Compute

### Lab 3.1: Deploy with ARM Template
1. Create an ARM template that deploys a storage account with parameters for name and SKU
2. Deploy using **Incremental mode** — verify the resource is created
3. Run **What-If** to preview changes
4. Deploy the same template with **Complete mode** — observe behavior
5. Convert the ARM template to **Bicep** using `az bicep decompile`
6. Deploy the Bicep template and verify the result

### Lab 3.2: Virtual Machines
1. Create a Windows VM with Standard_B2s size
2. Add a 64 GB Premium SSD data disk
3. RDP into the VM and initialize the data disk in Disk Management
4. Install the **Custom Script Extension** to run a script
5. Resize the VM to Standard_DS2_v2
6. Configure **Auto-shutdown** for 7 PM daily
7. **Deallocate** the VM and verify billing stops

### Lab 3.3: Availability and Scale Sets
1. Create an **Availability Set** with 2 FDs and 3 UDs
2. Create 2 VMs in the Availability Set
3. Create a **Virtual Machine Scale Set** with 2 instances
4. Configure autoscale: scale out at 70% CPU, scale in at 30%
5. Generate load on one instance and watch autoscale trigger

### Lab 3.4: Containers and App Service
1. Create an **Azure Container Instance** running nginx
2. Access the container via public FQDN
3. View container logs
4. Create an **App Service Plan** (Standard S1)
5. Create a **Web App** and deploy a sample app
6. Create a **staging** deployment slot
7. Deploy a v2 of the app to staging
8. **Swap** staging to production and verify zero-downtime

---

## 🌐 Lab 4 — Networking

### Lab 4.1: VNets, Subnets, and Peering
1. Create **VNet-A** (10.0.0.0/16) with subnet Web (10.0.1.0/24) and subnet App (10.0.2.0/24)
2. Create **VNet-B** (10.1.0.0/16) with subnet DB (10.1.1.0/24)
3. Create VMs in VNet-A/Web and VNet-B/DB
4. Verify VMs cannot communicate across VNets
5. Create **VNet peering** between VNet-A and VNet-B (both directions)
6. Verify VMs can now ping each other
7. Create a **Private DNS Zone** and link to VNet-A with auto-registration
8. Verify VM DNS records are created automatically

### Lab 4.2: NSGs and Security
1. Create an NSG with a rule to allow HTTP (port 80) inbound
2. Associate the NSG with the Web subnet
3. Create a rule to deny SSH (port 22) from the internet
4. Use **IP Flow Verify** to test if SSH from your IP is blocked
5. Create **Application Security Groups** for WebServers and DbServers
6. Assign VMs to ASGs
7. Create NSG rules using ASGs instead of IPs

### Lab 4.3: Load Balancing
1. Create a **Standard Load Balancer** with a public IP
2. Create a backend pool with 2 VMs
3. Configure a TCP health probe on port 80
4. Create a load balancing rule for port 80
5. Install a web server on both VMs (different content for each)
6. Access the load balancer public IP and refresh — observe traffic distribution
7. Stop one VM and verify traffic goes only to the healthy VM

### Lab 4.4: Network Watcher
1. Use **IP Flow Verify** to test connectivity to a VM
2. Use **Next Hop** to trace the route for traffic
3. Use **Connection Troubleshoot** to test TCP connectivity between two VMs
4. Enable **NSG Flow Logs** with version 2
5. View the flow log data in the storage account
6. Enable **Traffic Analytics** and explore the dashboards

---

## 📊 Lab 5 — Monitoring & Backup

### Lab 5.1: Azure Monitor and Alerts
1. Create a **Log Analytics workspace**
2. Enable **Diagnostic Settings** on a VM to send metrics to the workspace
3. Install **Azure Monitor Agent** on the VM
4. Open **Metrics Explorer** and chart CPU usage over the last hour
5. Write a KQL query: find events where CPU > 50% in the last 24h
6. Create an **Action Group** with your email
7. Create a **Metric Alert**: CPU > 80% for 5 minutes
8. Create an **Activity Log Alert**: when any resource is deleted

### Lab 5.2: Backup and Recovery
1. Create a **Recovery Services vault**
2. Set storage redundancy to **LRS**
3. Enable backup for a VM with a daily backup policy
4. Trigger an **on-demand backup**
5. Wait for the backup to complete
6. Perform **File Recovery** — mount the recovery point and browse files
7. Test **full VM restore** to a new VM
8. Verify **soft delete** — stop backup with delete data, then check soft-deleted items

---

## 💡 Tips for Labs

- Use the **Azure Free Account** ($200 credit for 30 days)
- Use **Azure Sandbox** from Microsoft Learn (free, no credit card needed)
- **Delete resources** after each lab to avoid charges
- Use `az group delete --name Lab-RG --yes --no-wait` to clean up quickly
- Take **screenshots** of key steps for your study notes
- Time yourself — exam labs have time limits

---

## 🧹 Cleanup Commands

```bash
# Delete all lab resource groups
az group delete --name Lab-RG --yes --no-wait
az group delete --name Lab-Networking --yes --no-wait
az group delete --name Lab-Compute --yes --no-wait
az group delete --name Lab-Storage --yes --no-wait
az group delete --name Lab-Monitoring --yes --no-wait
```
