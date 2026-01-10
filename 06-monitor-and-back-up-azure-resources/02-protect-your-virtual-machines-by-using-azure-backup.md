## 1. Protect Azure VMs with Azure Backup

### Introduction
As a solution architect, you need to **ensure recovery of critical Azure VMs** from data loss or corruption using **Azure Backup**.  

Azure Backup supports:
- Azure VMs  
- On-premises servers  
- Azure file shares  
- SQL Server or SAP HANA on Azure VMs  
- Other workloads

## 2. Azure Backup Features and Scenarios

### Overview
Azure Backup provides **secure, zero-infrastructure backup** for Azure-managed data assets and on-premises workloads.  
Supports VMs, SQL Server, SAP HANA, Azure Files, and other workloads. Managed easily via the Azure portal.

### Azure Backup vs Azure Site Recovery
| Feature | Azure Backup | Azure Site Recovery |
|---------|-------------|------------------|
| Purpose | Maintain copies of data for restore | Replicate workloads for failover |
| Use Case | Accidental deletion, data corruption, ransomware | Network/power outages, region-wide disaster |
| Recovery | RPO/RTO depends on policy | Almost real-time failover |

### Benefits of Azure Backup
- **Zero-infrastructure**: No backup servers or storage to manage  
- **Long-term retention**: Automated lifecycle management  
- **Security**: RBAC, encryption (Microsoft- or customer-managed keys), soft delete  
- **High availability**: LRS, GRS, ZRS replication options  
- **Network**: Uses Azure backbone; no internet access required  
- **Centralized management**: Built-in monitoring via Recovery Services vault

### Supported Scenarios
- **Azure VMs**: Windows/Linux, backups isolated in Recovery Services vault  
- **On-premises**: Files, folders, system state via MARS agent, MABS, or DPM  
- **Azure Files**: Snapshot management  
- **SQL Server & SAP HANA on Azure VMs**: Stream-based, workload-aware backups, supporting full, differential, and log backups with 15-min RPO and point-in-time recovery

## 3. Back Up an Azure Virtual Machine Using Azure Backup

### Overview
Azure Backup enables **easy backup and restore** of Azure VMs without extra software.  
Backups are taken as **disk snapshots** at intervals defined in a **backup policy** and stored in a **Recovery Services vault**.

### Recovery Services Vault
- Manages and stores backup data
- Acts as an RBAC boundary for secure access
- Handles storage automatically; no need to manage storage accounts

### Snapshots
- **Point-in-time backups** of all VM disks
- Extensions:
  - **VM Snapshot (Windows)**: Uses VSS for application-consistent backups
  - **VM SnapshotLinux (Linux)**: Snapshot of the disk; app consistency requires scripts
- Consistency levels:
  - **Application-consistent**: Full VM + app memory/I/O
  - **File system-consistent**: Only file system; apps clean up on restart
  - **Crash-consistent**: Captures disks only; no memory/I/O

### Backup Policy
- Define **frequency** (daily, weekly, or hourly with Enhanced policy) and **retention**
- Supports **Selective Disk Backup**: Back up or restore a subset of VM disks
- **Access tiers**:
  - **Snapshot tier**: Local storage, max 5 days, fast instant restore
  - **Vault tier**: Longer retention, secure storage in Recovery Services vault

### Backup Process
1. Select VM for backup; backup job triggered by policy
2. Backup extension installed (Windows or Linux)
3. Snapshot taken, stored locally, then transferred to the vault
4. Each VM disk backed up in parallel
5. Only changed blocks (delta) are transferred after initial backup
6. Vault encryption via **customer-managed keys (CMK)** optional
7. **Enhanced soft delete** protects backups from accidental/malicious deletion

### Notes
- Backup may take several hours at peak times; total VM backup < 24h for daily policies
- Instant restore recommended from snapshot tier for operational recoveries

Got it! Here’s the **corrected Markdown** with all code blocks properly closed:

## 4. Exercise: Back Up an Azure Virtual Machine

### Overview
Protect both Windows and Linux Azure VMs using **Azure Backup** via:
- Azure portal
- Azure CLI
- PowerShell  

Steps include creating VMs, enabling backup, performing initial backup, and monitoring.

### Setup Environment
1. Open **Azure Cloud Shell**.
2. Create a **resource group**:
```bash
RGROUP=$(az group create --name vmbackups --location westus2 --output tsv --query name)
````

3. Create a **virtual network and subnet**:

```bash
az network vnet create \
  --resource-group $RGROUP \
  --name NorthwindInternal \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name NorthwindInternal1 \
  --subnet-prefixes 10.0.0.0/24
```

---

### Create Virtual Machines

**Windows VM (NW-APP01):**

```bash
az vm create \
  --resource-group $RGROUP \
  --name NW-APP01 \
  --size Standard_DS1_v2 \
  --public-ip-sku Standard \
  --vnet-name NorthwindInternal \
  --subnet NorthwindInternal1 \
  --image Win2016Datacenter \
  --admin-username admin123 \
  --no-wait \
  --admin-password "<password>"
```

**Linux VM (NW-RHEL01):**

```bash
az vm create \
  --resource-group $RGROUP \
  --name NW-RHEL01 \
  --size Standard_DS1_v2 \
  --image RedHat:RHEL:8-gen2:latest \
  --authentication-type ssh \
  --generate-ssh-keys \
  --vnet-name NorthwindInternal \
  --subnet NorthwindInternal1
```

> If you get `securityProfile.securityType is invalid`, register the feature:

```bash
az feature register --name UseStandardSecurityType --namespace Microsoft.Compute
az feature show --name UseStandardSecurityType --namespace Microsoft.Compute
```

---

### Enable Backup

**Via Azure Portal:**

1. Go to **Virtual machines → NW-RHEL01 → Capabilities → Backup**.
2. Select **Standard**, accept defaults for:

   * Backup vault: `vaultXXX`
   * Backup policy: `DailyPolicy-xxxx`
3. Click **Enable backup**, then **Backup now** for first backup.

**Via Azure CLI:**

1. Create **Recovery Services vault**:

```bash
az backup vault create --resource-group vmbackups --location westus2 --name azure-backup
```

2. Enable backup for Windows VM:

```bash
az backup protection enable-for-vm \
  --resource-group vmbackups \
  --vault-name azure-backup \
  --vm NW-APP01 \
  --policy-name EnhancedPolicy
```

3. Monitor job progress:

```bash
az backup job list --resource-group vmbackups --vault-name azure-backup --output table
```

4. Perform initial backup immediately:

```bash
az backup protection backup-now \
  --resource-group vmbackups \
  --vault-name azure-backup \
  --container-name NW-APP01 \
  --item-name NW-APP01 \
  --retain-until 18-10-2030 \
  --backup-management-type AzureIaasVM
```

### Monitor Backups

**Single VM:**

* Portal → **Virtual machines → NW-APP01 → Capabilities → Backup**
* Check **Last backup status**

**Vault-wide:**

* Portal → **Recovery Services vault → Backup tab**
* View **summary of backup items, storage, and job statuses**

## 5. Restore Virtual Machine Data

After backing up Azure VMs, you can test recovery options as part of a BCDR plan. Azure Backup offers multiple restore types.

### Restore Types

| Option | Details |
|--------|---------|
| **Create a new VM** | Quickly create a new VM from a restore point in the same region. |
| **Restore disk** | Restore a disk to create a new VM, attach to an existing VM, or customize via template/PowerShell. |
| **Replace existing** | Replace a disk on an existing VM. Azure Backup snapshots the existing disk first. Not available if VM is deleted. |
| **Cross region restore** | Restore VMs/disks in a secondary (paired) region. Supported for Create VM and Restore Disks. |
| **Cross Subscription Restore** | Restore VMs/disks to another subscription in the same tenant. Requires vault settings enabled. Works with MSI; unsupported for snapshots or ADE-encrypted VMs. |
| **Cross Zonal Restore** | Restore VMs/disks across availability zones. Works only for managed VMs with ZRS-enabled vaults; unsupported for snapshots or encrypted VMs. |
| **Selective disk backup** | Restore a subset of VM disks using Enhanced policy. Useful for critical data or OS-only backups. |
| **File-level restore** | Mount snapshot via iSCSI to recover individual files. |
| **Encrypted VM restore** | Azure Backup supports VMs encrypted with Azure Disk Encryption. Limitations: only standalone keys supported, no file-level restore, and Replace Existing VM not available. |

### Key Notes
- **Application scenarios**:
  - Replace existing disk requires VM to exist.
  - Cross Subscription/Zonal restore applies to managed VMs only.
  - Encrypted VMs require key management via Azure Key Vault.
- **Granularity**: Selective disk backup allows restoring specific disks; file-level restore possible for non-encrypted VMs.

Here’s a **compact Markdown summary** for the “Exercise – Restore Azure virtual machine data” unit:

## 6. Exercise: Restore Azure Virtual Machine Data

This exercise demonstrates restoring a VM disk to a live server using Azure Backup and monitoring the restore process.

### Steps to Restore a VM

1. **Create a staging storage account**
   - In Azure portal → Storage accounts → Create
   - **Resource group**: `vmbackups`
   - **Storage account name**: `restorestagingYYYYMMDD`
   - **Region**: West US 2
   - Complete creation and wait for deployment.

2. **Stop the virtual machine**
   - VM must be stopped to restore successfully.
   - In Azure portal → Virtual Machines → `NW-APP01` → Stop → Confirm.

3. **Restore the VM**
   - Go to VM → Operations → Backup → Restore VM
   - Select a **restore point** (e.g., from 07/05/2021)
   - Configure restore:
     - **Restore type**: Replace existing
     - **Staging location**: Select the storage account created
   - Click **Restore** to start the process.

4. **Track the restore**
   - Azure portal → Backup Jobs → View all jobs
   - Select **View details** for the restore job
   - Monitor:
     - **Job details**: Information about the restore
     - **Job status**: Real-time progress
     - **Subtasks**: Task-level status within the restore job

### Notes
- VM must be stopped before restore; otherwise, an error occurs.
- The staging storage account is used for temporary storage during the restore.
- Notifications in Azure portal show the restore process status.

## 7. Summary

In this module, you learned the importance of a tested backup and recovery strategy. Key takeaways:

- **Azure Backup types**: Different options exist depending on your scenario.  
- **Backup targets**: Azure VMs and on-premises machines can be backed up.  
- **Backup and restore**: You practiced backing up an Azure VM and restoring it using various options.  
- **Monitoring**: You can track backup and restore progress.  

Azure Backup helps protect your environment against data loss or disk corruption and supports business continuity and disaster recovery plans.
