## 1. Introduction to Azure Backup

- **Purpose:**  
  Protect organizational data with a cost-effective, secure, and simple backup solution. Azure Backup addresses both on-premises and Azure-based scenarios.

- **Key Scenarios Supported:**  
  - Azure VMs  
  - Azure Managed Disks  
  - Azure Files  
  - SQL Server in Azure VMs  
  - SAP HANA in Azure VMs  
  - Azure Database for PostgreSQL & MySQL (Flexible servers)  
  - Azure Blobs  
  - Azure Kubernetes clusters  

- **Benefits:**  
  - Secure, cloud-based storage.  
  - Long-term retention options (e.g., for compliance audits).  
  - Daily monitoring of backup jobs.  
  - Protection against human error, ransomware, and on-premises limitations.

- **Example Use Case:**  
  SQL Server in an always-on availability group across 3 Azure VMs, backed up to Azure for 10-year retention, with daily monitoring.

## 2. What is Azure Backup?

- **Definition:**  
  Azure Backup is a **cost-effective, secure, zero-infrastructure service** for backing up and recovering Azure-managed and on-premises data.

- **Core Benefits:**  
  - No backup server or storage infrastructure required.  
  - Centralized management via **Backup Center** (also supports APIs, PowerShell, Azure CLI).  
  - Built-in security: encryption, private endpoints, alerts.  
  - Scales automatically to meet enterprise needs.  

- **Supported Workloads:**  
  - On-premises files, folders, system state  
  - Azure Virtual Machines and Managed Disks  
  - Azure File Shares  
  - SQL Server & SAP HANA in Azure VMs  
  - Azure Database for PostgreSQL & MySQL (Flexible servers)  
  - Azure Blobs  
  - Azure Kubernetes clusters  

- **Key Concepts:**  
  - **Recovery Time Objective (RTO):** Max time to restore after a failure.  
  - **Recovery Point Objective (RPO):** Max tolerable data loss (time).  

- **Example:**  
  - RPO = 1 hour → backups every hour, max data loss = 1 hour  
  - RTO = 3 hours → restore must complete within 3 hours

## 3. How Azure Backup Works

Azure Backup provides **secure, centralized, and automated data protection** for Azure and on-premises workloads.

### Key Components

- **Workload Integration Layer (Backup Extension):**
  - Integrates with Azure VMs, SQL, SAP HANA, and Azure Files.
  - Backup extensions generate backups at scheduled times.
  - Supports **snapshot backups** for storage or **stream backups** for databases.

- **Data Plane – Access Tiers:**
  - **Snapshot tier:** Fast restores; stored locally in customer subscription.
  - **Vault-standard tier:** Online storage in Microsoft-managed tenant; extra protection.
  - **Archive tier:** Long-term retention for compliance; rarely accessed.
  - **Storage redundancy:** LRS, GRS, ZRS options for high availability.

- **Data Plane – Security:**
  - Encryption for data at rest and in transit.
  - Azure RBAC controls backup and restore permissions.
  - **Soft delete:** Retains deleted backups for 14 days for recovery.

- **Management Plane:**
  - **Recovery Services vault / Backup vault:** Orchestrates backups and stores recovery points.
  - **Backup Center:** Centralized console for managing multiple vaults, subscriptions, workloads, and regions.
  - **Backup Policies:** Define schedules, retention, and frequency.

### Backup Types

- **Full backup:** Entire workload; max 1/day.
- **Differential backup:** Changes since last full backup; max 1/day.
- **Incremental backup:** Only changed blocks; reduces storage and network load.
- **Transactional log backup:** Point-in-time recovery; can run every 15 minutes.
- **Selective disk backup:** Backup/restore specific VM disks only.

### How it Works

1. Backup extension generates data (snapshot or stream).  
2. Data is sent securely to Azure Backup storage.  
3. Stored in chosen tier with replication and encryption.  
4. Management plane orchestrates policies, recovery points, and monitoring.

### Benefits

- Automated, zero-infrastructure backups.  
- Multiple tiers with different **RTO/RPO** options.  
- Secure, compliant, and highly available backup solution.  
- Centralized monitoring and management for large-scale environments.

## 4. When to Use Azure Backup

Azure Backup is ideal for organizations that need **secure, zero-infrastructure, centrally managed backup solutions** for Azure workloads and on-premises resources.

### Key Scenarios

1. **Ensuring Data Availability**
   - Protects against accidental deletion, corruption, or ransomware.
   - Self-service restores enable operational recovery by application admins.

2. **Protecting Azure Workloads**
   - Azure VMs (Windows and Linux)
   - Azure Disks
   - SQL Server in Azure VMs
   - SAP HANA databases in Azure VMs
   - Azure Files and Blobs
   - Azure Database for PostgreSQL/MySQL
   - Kubernetes clusters

3. **Securing Your Data**
   - Centralized management through Recovery Services vaults or Backup vaults.
   - Azure RBAC controls access to backup operations.
   - Encryption in transit and at rest.
   - Soft delete retains deleted backups for 14 days.

### Decision Criteria

| Criteria | Consideration |
|----------|---------------|
| Azure workloads | Supports VMs, SQL Server, SAP HANA, Azure Files/Blobs, PostgreSQL |
| Compliance | Custom backup policies with long-term retention across zones/regions |
| Operational recoveries | Self-service backup/restore for app admins to handle accidental deletion or corruption |

### SQL Server-Specific Benefits

- Workload-aware backups: full, differential, and transactional log backups.
- 15-minute RPO with frequent log backups.
- Point-in-time recovery up to the second.
- Individual database-level backup and restore.

### Compliance Features

- Long-term retention (weekly, monthly, yearly) for planned or on-demand backups.
- On-demand backups allow granular backups outside scheduled policies.
- Policies enforce retention rules and can be applied across multiple workloads.

### Monitoring and Administration

- Integrated with Log Analytics and Workbooks for reporting.
- Job monitoring scoped per vault.
- Backup Explorer aggregates all vaults and subscriptions for centralized monitoring.
- Provides detailed drill-down for troubleshooting and compliance reporting.

## 6. Azure Backup Summary

### Goal
Evaluate if **Azure Backup** meets your data-protection needs.

### Key Benefits
- **Availability:** Ensures data can be recovered.  
- **Workload Protection:** Supports Azure VMs, files/folders, system state.  
- **Security & Compliance:** Encryption, RBAC, retention policies, long-term retention.

### Example Scenario
- SQL Server on multiple Azure VMs:  
  - Backup full VMs or specific files/folders.  
  - Supports SQL-specific backups with point-in-time recovery.

### Management
- **Backup Center:** Centralized console to discover, govern, monitor, and optimize backups.  
- Automates protection against **ransomware, malicious admins, and accidental deletions**.  
- Simplifies **operational efficiency at scale**.