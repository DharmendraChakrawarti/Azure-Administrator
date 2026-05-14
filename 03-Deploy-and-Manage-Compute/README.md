# 3. Deploy and Manage Azure Compute Resources (20–25%)

This section covers automating deployment with ARM templates and Bicep, creating and managing VMs, provisioning containers, and configuring Azure App Service.

---

## 📑 Topics

| # | Topic | File |
|---|-------|------|
| 3.1 | Automate Deployment Using ARM Templates or Bicep | [3.1-Automate-Deployment-ARM-Bicep.md](./3.1-Automate-Deployment-ARM-Bicep.md) |
| 3.2 | Create and Configure Virtual Machines | [3.2-Create-Configure-VMs.md](./3.2-Create-Configure-VMs.md) |
| 3.3 | Provision and Manage Containers | [3.3-Provision-Manage-Containers.md](./3.3-Provision-Manage-Containers.md) |
| 3.4 | Create and Configure Azure App Service | [3.4-Create-Configure-App-Service.md](./3.4-Create-Configure-App-Service.md) |
| 3.5 | Practice Questions (10 scenarios) | [3.5-Practice-Questions.md](./3.5-Practice-Questions.md) |

---

## 🔑 Key Sub-Topics

### 3.1 Automate Deployment Using ARM Templates or Bicep
- ARM template structure (schema, parameters, variables, resources, outputs)
- Bicep syntax and modules
- Deploy templates (PowerShell, CLI, Portal)
- Deployment modes (Incremental vs Complete)
- Template Specs and What-If

### 3.2 Create and Configure Virtual Machines
- Create and configure VMs (sizing, images, networking)
- Availability Sets (Fault Domains, Update Domains)
- Availability Zones
- Virtual Machine Scale Sets (VMSS) and autoscaling
- Manage VM disks and encryption
- VM extensions and Custom Script Extension
- Generalize and capture VM images (Azure Compute Gallery)

### 3.3 Provision and Manage Containers
- Azure Container Instances (ACI) — container groups, restart policies
- Azure Container Registry (ACR) — tiers, authentication, roles
- Azure Container Apps — serverless containers
- Azure Kubernetes Service (AKS) — basics

### 3.4 Create and Configure Azure App Service
- App Service Plans (tiers, scaling)
- Web Apps (creation, configuration, app settings)
- Deployment slots (staging, swap, slot settings)
- Custom domains and SSL/TLS certificates
- App Service networking (VNet integration, access restrictions)
- Application Insights

---

> **Key exam areas**: ARM template sections, VM availability options (Sets vs Zones), VMSS autoscaling, deployment slots, ACI vs AKS, and App Service plan tiers.
