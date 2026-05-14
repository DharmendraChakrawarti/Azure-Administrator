# AZ-104 Quick Reference — Command Cheat Sheet

> All essential PowerShell and Azure CLI commands organized by exam domain.

---

## 🔐 1. Identity & Governance

### Users and Groups
```powershell
# PowerShell
New-AzureADUser -DisplayName "Name" -UserPrincipalName "user@domain.com" -AccountEnabled $true -PasswordProfile $pp
Remove-AzureADUser -ObjectId <UPN>
Set-AzureADUser -ObjectId <UPN> -Department "Engineering"
New-AzureADGroup -DisplayName "Group" -SecurityEnabled $true -MailEnabled $false -MailNickName "alias"
Add-AzureADGroupMember -ObjectId <GroupId> -RefObjectId <UserId>
```
```bash
# CLI
az ad user create --display-name "Name" --user-principal-name "user@domain.com" --password "P@ss"
az ad user delete --id <UPN>
az ad group create --display-name "Group" --mail-nickname "alias"
az ad group member add --group "Group" --member-id <UserId>
```

### RBAC
```powershell
New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Contributor" -Scope "/subscriptions/{sub-id}"
Get-AzRoleAssignment -SignInName "user@domain.com"
Remove-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Contributor" -Scope <scope>
New-AzRoleDefinition -InputFile "customrole.json"
```
```bash
az role assignment create --assignee "user@domain.com" --role "Contributor" --scope "/subscriptions/{sub-id}"
az role assignment list --assignee "user@domain.com"
az role definition create --role-definition customrole.json
```

### Policy
```powershell
New-AzPolicyAssignment -Name "PolicyName" -PolicyDefinition $def -Scope $scope
Get-AzPolicyAssignment -Scope $scope
```
```bash
az policy assignment create --name "PolicyName" --policy <def-id> --scope <scope>
az policy state trigger-scan --resource-group "myRG"
```

### Locks
```powershell
New-AzResourceLock -LockName "NoDelete" -LockLevel CanNotDelete -ResourceGroupName "myRG"
Remove-AzResourceLock -LockName "NoDelete" -ResourceGroupName "myRG"
```
```bash
az lock create --name "NoDelete" --lock-type CanNotDelete --resource-group "myRG"
az lock delete --name "NoDelete" --resource-group "myRG"
```

### Tags
```powershell
Update-AzTag -ResourceId <id> -Tag @{Env="Prod"} -Operation Merge
```
```bash
az tag update --resource-id <id> --operation Merge --tags Env=Prod
```

---

## 💾 2. Storage

### Storage Accounts
```powershell
New-AzStorageAccount -ResourceGroupName "myRG" -Name "account" -Location "eastus" -SkuName "Standard_LRS" -Kind "StorageV2"
Get-AzStorageAccountKey -ResourceGroupName "myRG" -Name "account"
New-AzStorageAccountKey -ResourceGroupName "myRG" -Name "account" -KeyName "key1"
```
```bash
az storage account create --name account --resource-group myRG --sku Standard_LRS --kind StorageV2
az storage account keys list --account-name account --resource-group myRG
az storage account keys renew --account-name account --key key1
```

### Blob Operations
```powershell
$ctx = New-AzStorageContext -StorageAccountName "acct" -StorageAccountKey "<key>"
New-AzStorageContainer -Name "container" -Context $ctx -Permission Off
Set-AzStorageBlobContent -Container "container" -File "file.txt" -Blob "file.txt" -Context $ctx
Get-AzStorageBlobContent -Container "container" -Blob "file.txt" -Destination "C:\" -Context $ctx
Set-AzStorageBlobTier -Container "container" -Blob "file.txt" -Tier "Archive" -Context $ctx
```
```bash
az storage container create --name container --account-name acct
az storage blob upload --container-name container --file file.txt --name file.txt --account-name acct
az storage blob download --container-name container --name file.txt --file file.txt --account-name acct
```

### SAS Tokens
```powershell
New-AzStorageAccountSASToken -Service Blob -ResourceType Container,Object -Permission "rl" -ExpiryTime (Get-Date).AddHours(2) -Context $ctx
New-AzStorageContainerSASToken -Name "container" -Permission "rl" -ExpiryTime (Get-Date).AddDays(7) -Context $ctx
```

### File Shares
```powershell
New-AzStorageShare -Name "share" -Context $ctx
Set-AzStorageFileContent -ShareName "share" -Source "file.txt" -Path "file.txt" -Context $ctx
```
```bash
az storage share-rm create --storage-account acct --name share --quota 1024
az storage file upload --share-name share --source file.txt --account-name acct
```

### Network Rules
```bash
az storage account update --name acct --default-action Deny
az storage account network-rule add --account-name acct --vnet-name vnet --subnet subnet
az storage account network-rule add --account-name acct --ip-address 1.2.3.4
```

---

## 🖥️ 3. Compute

### Virtual Machines
```powershell
New-AzVm -ResourceGroupName "myRG" -Name "vm" -Location "eastus" -Image "Win2022Datacenter" -Size "Standard_DS2_v2"
Start-AzVM -ResourceGroupName "myRG" -Name "vm"
Stop-AzVM -ResourceGroupName "myRG" -Name "vm"              # Deallocates
Restart-AzVM -ResourceGroupName "myRG" -Name "vm"
Get-AzVMSize -ResourceGroupName "myRG" -VMName "vm"          # Available sizes
```
```bash
az vm create --resource-group myRG --name vm --image Ubuntu2204 --size Standard_B2s --generate-ssh-keys
az vm start --resource-group myRG --name vm
az vm stop --resource-group myRG --name vm                    # Stopped (still billed!)
az vm deallocate --resource-group myRG --name vm              # Deallocated (no compute charge)
az vm resize --resource-group myRG --name vm --size Standard_DS3_v2
az vm list-sizes --location eastus --output table
```

### ARM/Bicep Deployment
```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "template.json" -WhatIf
```
```bash
az deployment group create --resource-group myRG --template-file main.bicep
az deployment group what-if --resource-group myRG --template-file template.json
az deployment group validate --resource-group myRG --template-file template.json
```

### Containers
```bash
az container create --resource-group myRG --name cont --image nginx --ports 80 --dns-name-label demo
az container logs --resource-group myRG --name cont
az acr create --resource-group myRG --name myACR --sku Standard
az acr login --name myACR
```

### App Service
```bash
az appservice plan create --name plan --resource-group myRG --sku S1
az webapp create --resource-group myRG --plan plan --name app --runtime "DOTNET:8.0"
az webapp deployment slot create --resource-group myRG --name app --slot staging
az webapp deployment slot swap --resource-group myRG --name app --slot staging
az webapp config appsettings set --resource-group myRG --name app --settings KEY=VALUE
```

---

## 🌐 4. Networking

### VNets and Subnets
```bash
az network vnet create --resource-group myRG --name vnet --address-prefix 10.0.0.0/16 --subnet-name sub --subnet-prefix 10.0.1.0/24
az network vnet subnet create --resource-group myRG --vnet-name vnet --name sub2 --address-prefixes 10.0.2.0/24
az network vnet peering create --resource-group myRG --name peer --vnet-name vnet1 --remote-vnet vnet2 --allow-vnet-access
```

### NSG
```bash
az network nsg create --resource-group myRG --name nsg
az network nsg rule create --resource-group myRG --nsg-name nsg --name AllowHTTP --priority 100 --direction Inbound --access Allow --protocol TCP --destination-port-ranges 80
az network vnet subnet update --resource-group myRG --vnet-name vnet --name sub --network-security-group nsg
```

### Public IP and DNS
```bash
az network public-ip create --resource-group myRG --name pip --sku Standard --allocation-method Static
az network dns zone create --resource-group myRG --name contoso.com
az network dns record-set a add-record --resource-group myRG --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
az network private-dns zone create --resource-group myRG --name private.contoso.com
```

### Load Balancing
```bash
az network lb create --resource-group myRG --name lb --sku Standard --public-ip-address pip
az network lb probe create --resource-group myRG --lb-name lb --name probe --protocol TCP --port 80
az network lb rule create --resource-group myRG --lb-name lb --name rule --protocol TCP --frontend-port 80 --backend-port 80 --probe-name probe
```

### Network Watcher
```bash
az network watcher test-ip-flow --resource-group myRG --vm vm --direction Inbound --protocol TCP --local 10.0.1.4:80 --remote 1.2.3.4:*
az network watcher show-next-hop --resource-group myRG --vm vm --source-ip 10.0.1.4 --dest-ip 10.0.2.5
az network watcher flow-log create --resource-group myRG --name log --nsg nsg --storage-account acct --enabled true --log-version 2
```

---

## 📊 5. Monitoring & Backup

### Azure Monitor
```bash
az monitor metrics alert create --resource-group myRG --name alert --scopes <vm-id> --condition "avg Percentage CPU > 80" --window-size 5m --action myAG
az monitor action-group create --resource-group myRG --name myAG --short-name AG --action email admin admin@contoso.com
az monitor activity-log list --resource-group myRG --output table
az monitor log-analytics workspace create --resource-group myRG --workspace-name ws --location eastus
az monitor diagnostic-settings create --resource <id> --name diag --workspace ws --metrics '[{"category":"AllMetrics","enabled":true}]'
```

### Backup
```bash
az backup vault create --resource-group myRG --name vault --location eastus
az backup protection enable-for-vm --resource-group myRG --vault-name vault --vm vm --policy-name DefaultPolicy
az backup protection backup-now --resource-group myRG --vault-name vault --container-name vm --item-name vm
az backup recoverypoint list --resource-group myRG --vault-name vault --container-name vm --item-name vm
az backup restore files mount-rp --resource-group myRG --vault-name vault --container-name vm --item-name vm --rp-name <rp>
```
