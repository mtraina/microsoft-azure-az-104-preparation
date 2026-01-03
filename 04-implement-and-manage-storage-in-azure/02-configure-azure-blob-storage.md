## 1. Configure Azure Blob Storage – Introduction

Azure Blob Storage is an AI-ready cloud service designed to store large amounts of **unstructured data**, such as text and binary files. It is commonly used for media, backups, and data accessed at scale.

In this module scenario, a media company stores and serves a large video library accessed thousands of times daily. Azure Blob Storage is used to:
- Optimize **cost and performance** using access tiers
- Apply **lifecycle management** for older content
- Configure **object replication** for failover and resilience

### Learning Objectives
- Understand the purpose and benefits of Azure Blob Storage
- Create and configure Blob Storage accounts
- Manage containers and blobs
- Optimize performance and scalability
- Implement lifecycle management policies
- Choose appropriate pricing and access tiers

## 2. Implement Azure Blob Storage

Azure Blob Storage is a cloud service for storing **unstructured data** as objects called blobs (Binary Large Objects). It’s also known as object or container storage.

### Key Concepts
- Stores text and binary data (documents, images, videos, installers)
- Built on three core resources:
  - Azure Storage Account
  - Containers
  - Blobs

### Configuration Areas
- Blob container options
- Blob types and upload options
- Access tiers
- Lifecycle management rules
- Object replication options

### Common Use Cases
- Serve images or documents directly to browsers
- Provide distributed access to files
- Stream video and audio
- Store data for backup, disaster recovery, and archiving
- Support application data access and analysis

## 3. Create Blob Containers

Azure Blob Storage uses **containers** to organize and manage blobs. A blob cannot exist without a container.

### Key Concepts
- Every blob must be stored in a container  
- Containers group related blobs  
- A container can hold an unlimited number of blobs  
- A storage account can have unlimited containers  
- Containers must be created before uploading data  

### Container Configuration
When creating a container in the Azure portal, you configure:

**Name**
- Must be unique within the storage account  
- Uses lowercase letters, numbers, and hyphens only  
- Must start with a letter or number  
- Length: 3–63 characters  

**Public Access Level**
- **Private (default):** No anonymous access to container or blobs  
- **Blob:** Anonymous read access to blobs only  
- **Container:** Anonymous read and list access to the entire container  

## 4. Assign Blob Access Tiers

Azure Blob Storage supports multiple **access tiers** to optimize cost and performance based on how frequently data is accessed.

### Blob Access Tiers Overview

**Hot Tier**
- Designed for data accessed or modified frequently  
- Lowest access costs, highest storage costs  
- Ideal for active data and ongoing processing  

**Cool Tier**
- Intended for infrequently accessed data stored for at least 30 days  
- Lower storage costs, higher access costs than Hot  
- Suitable for short-term backups, disaster recovery, and older media  

**Cold Tier**
- Optimized for rarely accessed data stored for at least 90 days  
- Lower storage costs and higher access costs than Cool  
- Used for long-term retention data that still needs to be available  

**Archive Tier**
- Offline tier with retrieval times measured in hours  
- Data must remain for at least 180 days  
- Lowest storage cost, highest access cost  
- Used for long-term backups, raw data, and compliance data  
- Blobs can’t be read or modified directly; they must be rehydrated to another tier  

### Access Tier Comparison Highlights
- **Latency:** Hot, Cool, and Cold provide millisecond access; Archive takes hours  
- **Availability:** High availability across all tiers  
- **Cost model:** Storage costs decrease from Hot → Archive, while access costs increase  

Choosing the right tier helps balance **cost, performance, and data availability** based on usage patterns.


| Feature                         | Hot Tier        | Cool Tier       | Cold Tier       | Archive Tier    |
|---------------------------------|----------------|-----------------|-----------------|-----------------|
| Availability                    | 99.9%          | 99%             | 99%             | 99%             |
| Availability (RA-GRS reads)     | 99.99%         | 99.9%           | 99.9%           | 99.9%           |
| Latency (time to first byte)    | Milliseconds   | Milliseconds    | Milliseconds    | Hours           |
| Minimum storage duration        | N/A            | 30 days         | 90 days         | 180 days        |
| Storage cost                    | Highest        | Lower           | Lower           | Lowest          |
| Access cost                     | Lowest         | Higher          | Higher          | Highest         |
| Typical use cases               | Active data    | Infrequent data | Rarely accessed | Long-term archive |

## 5. Add Blob Lifecycle Management Rules

Azure Blob Storage supports lifecycle management using rule-based policies to optimize cost and performance as data ages. These rules apply to General Purpose v2 and Blob Storage accounts.

Lifecycle management allows you to:
- Automatically move blobs to cooler tiers (Hot → Cool → Cold → Archive).
- Delete current blobs, previous versions, or snapshots when data expires.
- Apply rules to the entire storage account, specific containers, or selected blobs using prefixes or index tags.

Lifecycle policies are useful when data is accessed frequently at first and less often over time. For example, data can start in the Hot tier, move to Cool after a few weeks, and then transition to Archive after a month.

Rules are configured using **If–Then** conditions:
- **If** defines the age-based condition (for example, more than a certain number of days since last modification).
- **Then** defines the action to take, such as moving data to another tier or deleting it.

Available actions include:
- Move to Cool storage
- Move to Cold storage
- Move to Archive storage
- Delete the blob

By aligning storage tiers with data age, lifecycle management helps minimize storage costs while maintaining accessibility when needed.

## 6. Determine Blob Object Replication

Blob object replication asynchronously copies blobs between containers based on configured policy rules. Replication includes blob data, metadata, and versions.

Key points about object replication:
- Requires **blob versioning** enabled on both source and destination accounts.
- Supports blobs in **Hot, Cool, or Cold** tiers (tiers can differ between accounts).
- Does **not** support blob snapshots.
- Uses a **replication policy** that defines source and destination storage accounts.
- Policies contain rules mapping source containers to destination containers and identifying which blobs to replicate.

Benefits and use cases:
- **Reduce latency** by serving data from regions closer to users.
- **Improve compute efficiency** by processing identical data sets in multiple regions.
- **Support data distribution** by replicating processed results across regions.
- **Optimize costs** by combining replication with lifecycle policies to move replicated data to lower-cost tiers.

Versioning considerations:
- Blob versioning preserves previous versions of blobs.
- Enables recovery from accidental modification or deletion.
- Required for object replication to function.

## 7. Manage Blobs

A blob can store any type of data and any file size. Azure Blob Storage supports three blob types:

**Block blobs**
- Default blob type
- Optimized for storing text and binary data (files, images, videos)
- Most common Blob Storage use case

**Append blobs**
- Optimized for append operations
- Ideal for logging scenarios where data grows over time

**Page blobs**
- Up to 8 TB in size
- Optimized for frequent read/write operations
- Used by Azure Virtual Machines for OS and data disks

> **Note:** Blob type can’t be changed after creation.

### Managing blob storage

**Azure portal**
- Suitable for uploading and managing a small number of files
- Configure blob type, block size, container, access tier, and encryption

**Tools for large-scale operations**
- **Azure Storage Explorer**: GUI tool to manage blobs, files, queues, tables, permissions, and preview data
- **AzCopy**: Command-line tool for high-performance data transfer across containers and storage accounts
- **Azure Data Box Disk**: Physical SSD-based transfer for very large datasets or limited network connectivity

## 8. Determine Blob Storage Pricing

Understanding access patterns, durability, and availability needs helps manage Azure Blob Storage costs. The **Azure Pricing Calculator** is the primary tool to estimate costs based on workload-driven inputs.

### Key cost factors for Blob Storage

- **Volume of data stored per month**: More data increases storage cost.
- **Operations performed**: Charges depend on the type and number of read/write operations.
- **Data redundancy option**: Different replication strategies (LRS, ZRS, GRS, etc.) affect cost.

### Billing considerations

- **Performance tiers**: Hot, Cool, and Archive tiers have decreasing storage costs as the tier gets cooler.
- **Data access costs**: Cool and Archive tiers incur additional per-gigabyte charges for reads.
- **Transaction costs**: All tiers incur a per-transaction charge, increasing for cooler tiers.
- **Geo-replication transfer costs**: Applies to GRS and RA-GRS accounts, billed per GB.
- **Outbound data transfer**: Billed per GB for bandwidth usage.
- **Changes to storage tier**: 
  - Hot → Cool: billed for writing all data into the Cool tier.  
  - Cool → Hot: billed for reading all existing data.

Use the Azure Pricing Calculator to model your storage costs for migration, monthly usage, and future planning.

## 11. Summary and Resources

In this module, you learned about **Azure Blob Storage** and how to configure it. Blob Storage is Microsoft's object storage solution for the cloud, optimized for storing massive amounts of unstructured data such as text or binary files. You explored its features, use cases, and configuration options, including access tiers, lifecycle management, and object replication.

### Main takeaways

- **Azure Blob Storage** is ideal for storing unstructured data like text documents, images, and videos.  
- **Access tiers** (Hot, Cool, Cold, Archive) help optimize performance and cost based on data usage patterns.  
- **Lifecycle management policies** can automatically transition data between access tiers and set expiration rules.  
- **Object replication** allows asynchronous copying of blobs between containers in different regions, providing redundancy and reducing read latency.