## 1. Configure Azure Files — Introduction

Azure Files provides fully managed cloud-based file shares that can be accessed using standard file-sharing protocols. Azure File Sync allows Azure file shares to be cached on on-premises Windows Servers or Azure virtual machines for improved performance and local access.

**Scenario**
An organization with offices in multiple geographic regions needs a centralized, up-to-date repository for shared documents. Azure Files can provide a single, cloud-based location while ensuring users always access the latest versions.

**Learning objectives**
- Identify storage options for file shares
- Compare Azure file shares with Blob Storage
- Configure Azure file shares, snapshots, and soft delete
- Access file shares using Azure Storage Explorer

**Prerequisites**
- Familiarity with shared file systems
- Basic experience navigating the Azure portal

## 2. Compare storage for file shares and blob data

Azure Files provides fully managed cloud file shares accessible via SMB, NFS, and HTTP from Windows, Linux, and macOS. It’s designed to behave like a traditional file server without infrastructure management.

### Azure Files key characteristics
- Serverless, fully managed PaaS file shares
- Supports hierarchical folders and files
- Up to 100 TiB per share; files up to 4 TiB
- Encrypted at rest and in transit
- Accessible globally with internet connectivity
- Integrates with Microsoft Entra ID and AD DS
- Supports snapshots, Previous Versions, and Azure Backup
- Built-in data redundancy based on storage account settings

### Common use cases for Azure Files
- Replace or supplement on-premises file servers or NAS
- Provide global file access across operating systems
- Lift-and-shift applications that expect file system access
- Use Azure File Sync for local caching and replication
- Store shared app configurations, tools, logs, and diagnostics

### Azure Files vs Azure Blob Storage

| Azure Files (File Shares) | Azure Blob Storage (Blobs) |
|---------------------------|----------------------------|
| Access via SMB, NFS, REST | Access via REST and SDKs |
| True directory and file hierarchy | Flat namespace within containers |
| Shared file access across multiple VMs | Accessed as objects in containers |
| Ideal for lift-and-shift apps using file APIs | Ideal for unstructured, large-scale data |
| Good for shared tools and app data | Optimized for streaming and random access |

## 3. Manage Azure file shares

Azure Files supports two protocols—SMB and NFS—for mounting file shares. You cannot use both protocols on the same share, but you can create SMB and NFS shares within the same storage account.

### Storage tiers

| Tier     | Description |
|----------|-------------|
| **Premium** | Uses SSDs, available in FileStorage accounts. Offers consistent high performance and low latency. Supports LRS (locally redundant storage) and sometimes ZRS (zone-redundant storage). Not available in all regions. |
| **Standard** | Uses HDDs, available in GPv2 (general-purpose v2) accounts. Suitable for general-purpose workloads and dev/test environments. Supports LRS, ZRS, GRS (geo-redundant storage), and GZRS. Available in all regions. |

### Authentication methods

| Method | Description |
|--------|-------------|
| **Identity-based over SMB** | Provides single sign-on (SSO), similar to on-premises file shares. |
| **Access key** | Uses static storage account keys. Full control access, less flexible, bypasses access restrictions. Best practice: avoid sharing keys. |
| **Shared Access Signature (SAS)** | Dynamic URI based on storage keys. Provides restricted access (permissions, expiry, IP, protocol). Used mainly for REST API access. |

### Creating SMB Azure file shares

1. **Open port 445** – SMB requires TCP port 445. Ensure the firewall allows this. If blocked, use VPN or ExpressRoute with private endpoints.  
2. **Enable secure transfer** – Only allow HTTPS connections to enhance security. HTTP connections will be rejected if this is enabled.

### Mounting Azure file shares

**On Windows:**
- Use the portal to specify the drive letter and authentication method.
- Portal provides PowerShell commands for mounting.

**On Linux:**
- Use the CIFS kernel client.
- Mount manually with `mount` or persistently via `/etc/fstab`.

## 4. Create file share snapshots

Azure Files lets you take **share snapshots**, which are read-only, point-in-time copies of a file share. Snapshots help protect against accidental changes, deletions, or application errors.

### Key characteristics

- **File share level:** Snapshots are created at the share level, not per individual file (though you can restore individual files from a snapshot).  
- **Incremental:** Only changes since the last snapshot are saved. This reduces storage usage and creation time.  
- **Restoration:** You can restore a single file or the entire share from a snapshot.  
- **Deletion impact:** Deleting a file share removes all its snapshots.

### Benefits

| Benefit | Description |
|---------|-------------|
| **Protect against application errors/data corruption** | Take a snapshot before deploying new code. If bugs overwrite or corrupt data, restore from a previous snapshot. |
| **Protect against accidental deletions/changes** | Recover previous versions of deleted or modified files. |
| **Support backup and recovery** | Periodically snapshot file shares to maintain historical versions for audits or disaster recovery. |

## 5. Implement soft delete for Azure Files

Azure Files supports **soft delete**, which allows you to recover deleted files or entire file shares instead of losing them permanently.

### Key characteristics

- **Enabled at storage account level:** Soft delete applies to all file shares within a storage account.  
- **Soft deleted state:** Deleted files/shares are moved to a recoverable state rather than being permanently erased.  
- **Retention period:** You can configure retention from **1 to 365 days**. During this period, data can be restored.  
- **Applicable to new and existing shares:** Soft delete can be turned on anytime.

### Benefits

| Benefit | Description |
|---------|-------------|
| **Recover from accidental data loss** | Restore deleted or corrupted files and shares easily. |
| **Upgrade scenarios** | Roll back to a known good state after a failed upgrade. |
| **Ransomware protection** | Recover data without paying ransom. |
| **Long-term retention** | Meet compliance and data retention requirements. |
| **Business continuity** | Ensure critical workloads remain available. |

## 6. Use Azure Storage Explorer

**Azure Storage Explorer** is a standalone tool for managing Azure Storage data on Windows, macOS, and Linux. It provides access to multiple accounts, subscriptions, and storage types.

### Key Characteristics

- Requires **management (Azure Resource Manager) and data layer permissions** via Microsoft Entra ID.
- Allows connecting to:
  - Storage accounts in your Azure subscription
  - Shared storage accounts from other subscriptions
  - Local storage using the **Azure Storage Emulator**

### Common Scenarios

| Scenario | Description |
|----------|-------------|
| **Connect to Azure subscription** | Manage storage resources in your own subscription. |
| **Work with local development storage** | Use Azure Storage Emulator for local testing. |
| **Attach to external storage** | Access storage accounts from other subscriptions or national clouds using account name, key, and endpoints. |
| **Attach with SAS** | Connect to a storage account or specific service (blob, queue, table) using a **Shared Access Signature**. |

### Access Keys

- Each storage account has **two access keys**.
- Access keys provide full account access; **store them securely**.
- Regenerate keys regularly to maintain security.
- When regenerating, update all applications that use the keys. This does **not** interrupt VM disk access.

### Tips

- Use SAS tokens instead of access keys when possible to reduce risk.
- Azure Storage Explorer simplifies sharing storage across teams and subscriptions.

## 7. Consider Azure File Sync

**Azure File Sync** lets you cache multiple Azure Files shares on on-premises Windows Servers or cloud VMs, combining cloud centralization with local performance.

### Key Characteristics

- Transforms Windows Server into a **fast cache** of Azure Files shares.
- Supports **any protocol available on Windows Server** (SMB, NFS, FTPS) for local access.
- Supports **multiple caches worldwide**.

### Cloud Tiering

- Optional feature that keeps frequently accessed files locally and tiers others to Azure Files.
- Tiered files are replaced locally with a **reparse point (pointer)** to the file in Azure.
- When users access a tiered file, it is recalled seamlessly from Azure Files.
- Tiered files show **greyed icons with an offline `O` attribute** to indicate they reside in the cloud.

### Scenarios & Benefits

| Scenario | Benefit |
|----------|---------|
| **Application lift and shift** | Move applications between Azure and on-premises, providing consistent write access across servers. |
| **Branch office support** | Set up servers in branch offices that sync with Azure storage for backup and access. |
| **Backup & disaster recovery** | Use Azure Backup with File Sync for fast restore of metadata and data recall. |
| **File archiving with cloud tiering** | Keep only recently accessed data locally; older files are stored in Azure. |

## 9. Azure Files Summary and Resources

### Overview
Azure Administrators are familiar with **Azure Files** and the **Azure File Sync agent**. They can implement fully managed file shares in the cloud using **industry-standard protocols** and understand how to cache Azure Files shares on **on-premises Windows Server** or **cloud virtual machines**.

### Key Takeaways

#### Protocols and Access
- Azure Files provides **SMB** and **NFS protocols**, client libraries, and a REST interface.
- Access your files from **anywhere**.

#### Use Cases
- Ideal for **lift-and-shift applications** that use native file system APIs.
- Share data between apps running in Azure.

#### File Share Types
- **Standard**: Stores data on HDDs.
- **Premium**: Stores data on SSDs for high performance.

#### Data Protection
- **File share snapshots**: Point-in-time, read-only copies.
- **Soft delete**: Recover deleted file shares.

#### Tools
- **Azure Storage Explorer**: Standalone app for managing Azure storage on **Windows, macOS, Linux**.
- **Azure File Sync**: Cache file shares on-premises or in the cloud.