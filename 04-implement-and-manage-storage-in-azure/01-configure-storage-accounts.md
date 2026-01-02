## 1. Configure Storage Accounts

Azure Storage is Microsoft’s cloud solution for modern data storage, designed to handle large-scale, high-traffic scenarios with durability and quick recovery.

### Scenario Example
- Large e-commerce company storing and serving product images.
- Needs scalability, reliability, high traffic support, and fast data restoration.

### Learning Objectives
- Identify features and usage cases for Azure Storage accounts.
- Select the appropriate Azure Storage type and create storage accounts.
- Choose a storage replication strategy for durability.
- Configure secure network access to storage endpoints.

### Skills Measured
- Preparing for **Exam AZ-104: Microsoft Azure Administrator**.

### Prerequisites
- Experience using the Azure portal.
- Familiarity with managing different types of data storage.

## 2. Implement Azure Storage

Azure Storage is Microsoft’s cloud storage solution, designed for modern, scalable, and AI-ready applications. It supports files, messages, tables, and other data types for IaaS VMs, PaaS services, and developer applications.

### Data Categories

| Category            | Description                                                                 | Storage Examples |
|--------------------|-----------------------------------------------------------------------------|----------------|
| **Virtual machine data** | Disks and files for Azure VMs. Persistent block storage and managed file shares. | Azure Managed Disks, Azure File Storage |
| **Unstructured data**   | Non-relational, least organized data.                                      | Azure Blob Storage, Azure Data Lake Storage |
| **Structured data**     | Relational data with schema; rows, columns, keys.                         | Azure Table Storage, Azure Cosmos DB, Azure SQL Database |

### Key Considerations
- **Durability & Availability:** Data is replicated across datacenters or regions to survive hardware failures and disasters.
- **Secure Access:** Data is encrypted and access is finely controlled.
- **Scalability:** Supports massive scaling for storage and performance.
- **Manageability:** Azure handles hardware, updates, and critical issues.
- **Accessibility:** Data is accessible globally over HTTP/HTTPS with SDKs for .NET, Java, Node.js, Python, PHP, Ruby, Go, REST API, and tools like Azure PowerShell, CLI, portal, and Storage Explorer.

## 3. Explore Azure Storage Services

Azure offers several storage services for different scenarios, each optimized for specific types of data.

### Azure Blob Storage
- Object storage for massive amounts of **unstructured/non-relational data**.
- Ideal for:
  - Serving images or documents to browsers
  - File storage for distributed access
  - Streaming audio/video
  - Backup, restore, disaster recovery, archiving
  - Data analysis
- Access via **HTTP/HTTPS, REST API, Azure CLI, PowerShell, SDKs** (.NET, Java, Node.js, Python, PHP, Ruby)
- Supports **NFS protocol**.

### Azure Files
- Provides **highly available network file shares**.
- Accessible via **SMB, NFS, REST API, SDKs**.
- Use cases:
  - Migrating on-premises apps
  - Shared configuration files and tools
  - Storing logs, metrics, crash dumps
- Authentication via **storage account credentials**.

### Azure Queue Storage
- Stores and retrieves **messages** (up to 64 KB each, millions of messages per queue).
- Common for **asynchronous processing**.
- Example: Queue a message for image thumbnail creation after upload, then process it via an Azure Function.

### Azure Table Storage
- **Non-relational structured data** (NoSQL), key/attribute store, schemaless.
- Fast, cost-effective for many apps.
- Part of **Azure Cosmos DB Table API**: throughput-optimized, globally distributed, automatic secondary indexes.

### Considerations for Choosing Storage Services
- **Massive unstructured data:** Use Blob Storage for global access, streaming, and backup.
- **High availability for shared files:** Use Azure Files for network shares.
- **Message storage for async processing:** Use Queue Storage.
- **Structured non-relational data:** Use Table Storage or Cosmos DB Table API for modern app needs.

## 4. Determine Storage Account Types

Azure storage accounts are categorized as **Standard** or **Premium**, each with specific use cases.

### Standard Storage Accounts
- Backed by **HDD**.
- Lowest cost per GB.
- Suitable for bulk storage or infrequently accessed data.
- Supports:
  - Blob Storage (including Data Lake Storage)
  - Queue Storage
  - Table Storage
  - Azure Files
- Example: Most general-purpose scenarios, including blobs, file shares, queues, tables, and disks.

### Premium Storage Accounts
- Backed by **SSD** for low-latency, consistent performance.
- Ideal for **I/O-intensive applications** (e.g., VM disks, databases).
- Cannot convert Standard ↔ Premium; a new account is needed.
- Supports different types:

| Storage Account Type       | Supported Services               | Recommended Usage |
|----------------------------|---------------------------------|-----------------|
| Premium block blobs        | Blob Storage                     | High transaction rates; smaller objects; low latency required |
| Premium file shares        | Azure Files                      | Enterprise/high-performance scale; SMB & NFS file shares |
| Premium page blobs         | Page blobs only                  | OS disks, VM data disks, databases; index-based or sparse data |

> **Tip:** All storage accounts use **Storage Service Encryption (SSE)** for data at rest.

## 5. Determine Replication Strategies

Azure Storage replicates data to ensure **durability** and **high availability**. Replication options vary by cost, durability, and scope.

### 1. Locally Redundant Storage (LRS)
- Stores **3 copies** within a single data center.
- **Lowest cost**, but lowest durability.
- Suitable for:
  - Easily reconstructable or non-critical data.
  - Frequently changing data where loss is acceptable.
  - Data restricted to a single location.

### 2. Zone-Redundant Storage (ZRS)
- Synchronously replicates data across **3 availability zones** in a single region.
- Provides **high availability** and low latency.
- Not available in all regions.
- Switching to ZRS requires moving data physically.

### 3. Geo-Redundant Storage (GRS)
- Replicates data to a **secondary region** (hundreds of miles away).
- Designed for **16 9’s durability**.
- Options:
  - **GRS**: Secondary region is read-only; failover occurs only if Microsoft initiates it.
  - **RA-GRS**: Secondary region can be read at any time, even without failover.
- Primary replication uses LRS; secondary replication is asynchronous.

### 4. Geo-Zone-Redundant Storage (GZRS)
- Combines **zone redundancy** in primary region + **geo-replication** to secondary region.
- Data remains available if an availability zone or entire primary region fails.
- **RA-GZRS** enables read access from secondary region.

### Replication Summary Table

| Replication Type | Node unavailable | Entire data center unavailable | Region-wide outage | Read access during region-wide outage |
|-----------------|-----------------|-------------------------------|------------------|-------------------------------------|
| LRS              | ✅              | ❌                             | ❌                | ❌                                   |
| ZRS              | ✅              | ✅                             | ❌                | ❌                                   |
| GRS              | ✅              | ✅                             | ✅                | ❌                                   |
| RA-GRS           | ✅              | ✅                             | ✅                | ✅                                   |
| GZRS             | ✅              | ✅                             | ✅                | ❌                                   |
| RA-GZRS          | ✅              | ✅                             | ✅                | ✅                                   |

> **Tip:** Use **GZRS** for high durability, availability, performance, and disaster recovery; enable **RA-GZRS** for read access from secondary region during disasters.

## 6. Access Storage

Every object in Azure Storage has a **unique URL**. The storage account name forms the subdomain of the URL, and the service determines the domain.

### Default Endpoints

| Service          | Default Endpoint                                |
|-----------------|-------------------------------------------------|
| Blob Storage     | `//<storage-account>.blob.core.windows.net`    |
| Table Storage    | `//<storage-account>.table.core.windows.net`   |
| Queue Storage    | `//<storage-account>.queue.core.windows.net`   |
| File Storage     | `//<storage-account>.file.core.windows.net`    |

**Example:** Access a blob named `myblob` in `mycontainer`: //mystorageaccount.blob.core.windows.net/mycontainer/myblob.

### Custom Domains

- You can map a **custom domain** to your Azure Blob Storage.
- Example: map `blobs.contoso.com` to `mystorageaccount.blob.core.windows.net` using a **CNAME DNS record**.
- Users can then access blob data via your custom domain instead of the default endpoint.

The following example shows how a subdomain is mapped to an Azure storage account to create a CNAME record in the domain name system (DNS):
- Subdomain: blobs.contoso.com
- Azure storage account: \<storage account>\.blob.core.windows.net
- Direct CNAME record: contosoblobs.blob.core.windows.net

## 7. Secure Storage Endpoints

Azure allows you to **restrict network access** to storage accounts using **firewalls and virtual networks**.

### Key Points

- Configure **Firewalls and virtual networks** in the Azure portal for your storage account.
- Add **subnets or public IP ranges** that should have access.
- Only subnets/virtual networks in the **same Azure region or paired region** as the storage account are supported.
- **Service endpoints** provide the base URL for accessing blob, queue, table, or file objects.
- Always **test service endpoints** to ensure access is correctly restricted.

> Tip: Use Azure training modules on service endpoints and NSGs to practice restricting access.

## Azure Storage – Quick Summary

### Data Types
- **VM Data:** Disks, files for IaaS VMs  
- **Unstructured Data:** Blob Storage, Data Lake Storage  
- **Structured Data:** Table Storage, Cosmos DB, SQL Database  

### Storage Services
| Service | Purpose / Use Case |
|---------|------------------|
| **Blob Storage** | Store unstructured data, media files, backup, archiving |
| **Files** | Managed SMB/NFS file shares for multiple VMs |
| **Queue Storage** | Message storage for asynchronous processing |
| **Table Storage** | Non-relational structured data (NoSQL) |

### Storage Account Types
| Type | Storage Medium | Use Case |
|------|----------------|---------|
| **Standard** | HDD | Low-cost, bulk storage, infrequently accessed data |
| **Premium** | SSD | Low-latency, high IOPS, for VMs or databases |

### Replication Strategies
| Strategy | Scope | Notes |
|----------|-------|------|
| **LRS** | Local | Copies within a single data center |
| **ZRS** | Zone | Copies across multiple zones in a region |
| **GRS / RA-GRS** | Geo | Replicates to secondary region; RA-GRS allows read access |
| **GZRS / RA-GZRS** | Geo + Zone | Zone + geo redundancy; high durability and disaster recovery |

### Access & Security
- Each object has a **unique URL endpoint**  
- **Custom domains** can be mapped via DNS (CNAME)  
- **Secure endpoints** using Firewalls & virtual networks  
- Always **test endpoints** to verify restricted access  

**Tip:** Choose **storage type, service, and replication strategy** based on your app’s **performance, cost, and durability requirements**.