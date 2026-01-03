## 1. Introduction

Azure Storage provides a comprehensive set of security capabilities that help developers build secure applications.

In this module, you’re responsible for securing sensitive data stored in Azure Storage, including personal information used by both internal teams and external developers. Your goal is to configure secure access and protect data appropriately.

### Learning objectives

- Configure shared access signatures (SAS), including URIs and parameters  
- Configure Azure Storage encryption  
- Implement customer-managed keys  
- Recommend improvements to Azure Storage security  

## 2. Review Azure Storage security strategies

Azure Storage uses multiple security strategies to help protect data, following a defense-in-depth approach. These strategies combine encryption, authentication, authorization, and monitoring to secure storage resources.

### Core security features

- **Encryption at rest**  
  All data written to Azure Storage is automatically encrypted using Storage Service Encryption (SSE) with 256-bit AES. This includes virtual hard disks (VHDs), using BitLocker for Windows and dm-crypt for Linux.

- **Encryption in transit**  
  Data is protected during transfer by enforcing HTTPS for REST API calls and SMB 3.0 for Azure Files. Secure transfer can be required at the storage account level, blocking insecure connections.

- **Encryption models**  
  Azure supports multiple models:
  - Server-side encryption with Microsoft-managed keys  
  - Customer-managed keys stored in Azure Key Vault  
  - Customer-managed keys stored on customer-controlled hardware  
  - Client-side encryption with keys managed outside Azure  

- **Authorization**  
  Microsoft Entra ID with managed identities is the recommended method for authorizing access to blob, queue, and table data, offering stronger security than Shared Key authorization.

- **Role-based access control (RBAC)**  
  RBAC allows fine-grained access control by assigning roles to users, groups, or applications at the storage account scope.

- **Storage analytics**  
  Logging and metrics help track requests, analyze usage patterns, and diagnose issues within a storage account.

### Authorization strategies

| Authorization method | Description |
|----------------------|-------------|
| Microsoft Entra ID | Cloud-based identity and access management with RBAC for fine-grained permissions |
| Shared Key | Uses storage account access keys to sign requests |
| Shared Access Signatures (SAS) | Delegates limited access to storage resources with specific permissions and time limits |
| Anonymous access | Allows public read access to containers or blobs without authorization |

## 3. Create shared access signatures

A shared access signature (SAS) is a URI that grants limited, time-bound access to Azure Storage resources without exposing the storage account keys. SAS is commonly used to allow users or applications to read or write data securely.

### SAS types

- **User delegation SAS**  
  Secured with Microsoft Entra ID credentials and SAS permissions. Supported for Blob Storage and Data Lake Storage.

- **Account-level SAS**  
  Grants access to multiple services and resource types, including actions such as creating file systems.

- **Service-level SAS**  
  Grants access to specific resources within a storage service, such as downloading a blob or listing files.

- **Stored access policies**  
  Used with service-level SAS to centrally manage, group, and revoke permissions without regenerating account keys.

### Risk mitigation best practices

| Recommendation | Description |
|---------------|-------------|
| Always use HTTPS | Prevents interception of SAS tokens during creation and distribution |
| Use stored access policies | Allows revocation and control without rotating account keys |
| Set short expiration times | Limits damage if a SAS is compromised |
| Require SAS renewal | Ensures continued access while allowing retries before expiration |
| Plan SAS start times carefully | Avoids failures caused by clock skew |
| Grant minimum permissions | Reduces impact if a SAS is leaked |
| Validate written data | Protects against corrupted or malicious uploads |
| Evaluate if SAS is appropriate | In some cases, a middle-tier service or public access is safer |

## 4. Identify URI and SAS parameters

When a shared access signature (SAS) is created, it forms a URI composed of:
- The Azure Storage resource URI
- A SAS token containing parameters that define access scope, permissions, and validity

### SAS URI structure
A SAS URI combines the storage endpoint with query parameters that control access to the resource.

### Key URI and SAS parameters

| Parameter | Example | Description |
|---------|--------|-------------|
| Resource URI | https://myaccount.blob.core.windows.net/ | Defines the Azure Storage endpoint and the resource being accessed |
| Storage version | sv=2015-04-05 | Specifies the Azure Storage API version to use |
| Storage service | ss=bf | Indicates the storage services the SAS applies to (Blob, File) |
| Start time | st=2015-04-29T22:18:26Z | Optional UTC start time when the SAS becomes valid |
| Expiry time | se=2015-04-30T02:23:26Z | UTC time when the SAS expires |
| Resource type | sr=b | Specifies the type of resource (blob, container, etc.) |
| Permissions | sp=rw | Defines allowed operations such as read, write, delete |
| IP range | sip=168.1.5.60-168.1.5.70 | Restricts access to specific client IP ranges |
| Protocol | spr=https | Limits access to HTTPS only |
| Signature | sig=CODE HERE | HMAC-SHA256 signature used to authenticate the SAS |

### Key considerations
- Omitting the start time makes the SAS valid immediately
- Short expiration times reduce risk if a SAS is compromised
- Using HTTPS-only access improves security
- Permissions should follow the principle of least privilege

## 5. Determine Azure Storage encryption

Azure Storage automatically encrypts data at rest to help meet security and compliance requirements. Encryption and decryption happen transparently, so no application or code changes are required.

### Key points about Azure Storage encryption
- All data is encrypted before being written to storage and decrypted when accessed
- Encryption uses 256-bit Advanced Encryption Standard (AES)
- Encryption is enabled by default for all storage accounts and can’t be disabled
- Key management and encryption processes are transparent to users

### Access keys and key management
- Each storage account has two 512-bit access keys
- These keys can be used for Shared Key authorization or to sign SAS tokens
- Microsoft recommends storing and rotating keys using Azure Key Vault
- Keys can be rotated manually or automatically without application downtime

### Encryption configuration options
- **Platform-managed keys (PMK):** Keys are generated and managed entirely by Azure
- **Customer-managed keys (CMK):** Keys are managed by the customer using Azure Key Vault or HSM (including Bring Your Own Key scenarios)
- **Infrastructure encryption:** Optional extra layer that encrypts data twice using different algorithms and keys, at both service and infrastructure levels

## 6. Create customer-managed keys

Azure Storage supports customer-managed keys (CMKs) to give you greater control over data encryption by using Azure Key Vault.

Customer-managed keys allow you to:
- Create, rotate, disable, audit, and manage access to encryption keys
- Use your own keys instead of Microsoft-managed keys
- Meet stricter security and compliance requirements

CMKs are stored in Azure Key Vault and can be newly created or existing keys. The storage account and key vault must be in the same Azure region, but they can be in different subscriptions.

### Configuring customer-managed keys
In the Azure portal, you configure CMKs by selecting:
- **Encryption type:** Microsoft-managed keys or customer-managed keys
- **Encryption key:** A key selected from an Azure Key Vault or provided via key URI

Using Azure Key Vault with customer-managed keys provides centralized key management and enhanced security control for Azure Storage.

## 7. Apply Azure Storage security best practices

Azure Storage Insights provides comprehensive monitoring to help secure and optimize Azure Storage accounts.

**Key benefits**
- Detailed metrics and logs for visibility into latency, throughput, capacity, and transactions
- Enhanced security and compliance through actionable insights and alerts
- Integration with Azure security features such as RBAC, Microsoft Entra ID, connection strings, and ACLs
- A unified view of storage performance, capacity, and availability

**Security use cases**
- Real-time monitoring to detect anomalies and track usage trends
- Security auditing using detailed logs to support compliance
- Health analysis and optimization to maintain secure and efficient storage accounts

## 8. Exercise: Manage Azure Storage

https://microsoftlearning.github.io/AZ-104-MicrosoftAzureAdministrator/Instructions/Labs/LAB_07-Manage_Azure_Storage.html



