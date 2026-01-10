## 1. Introduction to Azure Virtual Machines

You manage servers for a medical research company, but the aging hardware is struggling with new data analysis applications. Upgrading isn't ideal due to:

- Servers are globally distributed with minimal staff.
- Custom software across Windows/Linux with complex configurations needs testing.
- Growing business requires scaling for future load.

To solve these issues, you decide to explore Azure Virtual Machines (VMs). Azure VMs provide scalable, on-demand resources with full configuration control, and services for monitoring, security, and updates.

### Learning Objectives
- Compile a checklist for creating a virtual machine
- Understand options for creating and managing virtual machines
- Learn services for administering virtual machines

## 2. Compile a Checklist for Creating an Azure Virtual Machine

Migrating on-premises servers to Azure requires careful planning. You can move servers in small batches or individually. Before creating a VM, consider how your current infrastructure maps to Azure.

### What is an Azure Resource?
An Azure resource is any manageable item in Azure, such as:
- VM
- Disks
- Virtual Network
- Network Interface
- Network Security Group (NSG)
- IP addresses (public, private)

Azure creates these resources, or you can supply existing ones. Naming consistency is important, as Azure uses the VM name to generate resource names.

### Checklist for VM Creation

1. **Network Configuration**  
   Consider your network requirements before deploying VMs. Virtual Networks (VNets) provide private connectivity, and subnets segregate traffic. Security groups (NSGs) control traffic flow and act as firewalls.

2. **VM Name and Naming Convention**  
   VM names are critical for identification. A good convention includes:
   - **Environment** (dev, prod, QA)
   - **Location** (e.g., "eus" for East US)
   - **Instance** (01, 02)
   - **Product/Service** (e.g., service)
   - **Role** (sql, web)

   Example: `deveus-webvm01` for a dev web server in East US.

3. **VM Location**  
   Choose a region based on proximity to users, compliance, cost, and available configurations. Prices vary by region.

4. **VM Size**  
   Select VM size based on workload:

| **Option**             | **Description**                                                               |
|------------------------|-------------------------------------------------------------------------------|
| **General purpose**     | Balanced CPU-to-memory ratio. Ideal for testing, development, and small-medium databases. |
| **Compute optimized**   | High CPU-to-memory ratio. Suitable for web servers, batch processes, and application servers. |
| **Memory optimized**    | High memory-to-CPU ratio. Ideal for relational databases and in-memory analytics. |
| **Storage optimized**   | High disk throughput and I/O. Ideal for database workloads.                   |
| **GPU**                 | For graphics rendering, video editing, and deep learning model training.      |
| **High performance compute** | Fastest and most powerful CPU VMs with high-throughput network interfaces. |

   VM size affects both performance and cost.

5. **Resizing VMs**  
   You can change the VM size while it’s running (with potential reboots) or when stopped. Resizing affects costs.

6. **Billing for VMs**  
   Azure charges for:

| **Resource**             | **Description**                                                          | **Cost** |
|--------------------------|--------------------------------------------------------------------------|----------|
| **Compute**              | Billed per minute based on VM size and OS.                              | Varies  |
| **Storage**              | Charges for OS and data disks, independent of VM status.                | Varies  |

   Consider using Azure Hybrid Benefit for cost savings on OS licensing.

7. **Disk Types**  
   Choose from five disk types based on your needs:

| **Disk Type**           | **Scenario**                                                        | **Max Disk Size** | **Max IOPS** | **Max Throughput** |
|-------------------------|---------------------------------------------------------------------|-------------------|--------------|-------------------|
| **Ultra Disk**           | IO-intensive workloads (e.g., SAP HANA, SQL)                       | 65,536 GiB        | 160,000      | 4,000 MB/s        |
| **Premium SSD v2**       | Production, performance-sensitive workloads                         | 65,536 GiB        | 80,000       | 1,200 MB/s        |
| **Premium SSD**          | Performance-sensitive workloads                                     | 32,767 GiB        | 20,000       | 900 MB/s          |
| **Standard SSD**         | Web servers, lightly used applications                              | 32,767 GiB        | 6,000        | 750 MB/s          |
| **Standard HDD**         | Backup, non-critical workloads                                      | 32,767 GiB        | 2,000        | 500 MB/s          |

8. **Operating System**  
   Choose from pre-configured Azure OS images or use custom images for specific requirements. The OS choice impacts compute costs.

## 3. Exercise - Create a VM using the Azure portal

Once you’ve planned your network and identified VMs to migrate, you can choose how to create and manage VMs in Azure. The easiest method is through the Azure portal, which offers a web-based interface for creating and administering resources.

### Prerequisites
- **Azure Subscription**: Required to complete this exercise.
- **Resource Group**: You can either use an existing resource group or create a new one. Follow the steps to create a resource group if necessary.

### Steps to Create a VM using the Azure Portal

1. **Sign in to the Azure portal**.

2. **Create a New Resource**  
   - On the Azure home page, under Azure services, select **Create a resource**.
   - In the **Create a resource** pane, select **Virtual machine**.

3. **Configure the VM**  
   - **Project details**:
     - **Subscription**: Select your subscription.
     - **Resource group**: Choose an existing or create a new resource group (e.g., `myResourceGroupName`).

   - **Instance details**:
     - **VM Name**: Enter `test-ubuntu-cus-vm`.
     - **Region**: Select a geographical region close to you.
     - **Availability options**: Select **No infrastructure redundancy required**.
     - **Security type**: Select **Standard**.
     - **Image**: Choose **Ubuntu Server 24.04 LTS - Gen2**.
     - **VM architecture**: Select **x64**.
     - **Run with Azure Spot discount**: Leave unchecked.
     - **Size**: Select **Standard D2s V3**.

   - **Administrator account**:
     - **Authentication type**: Choose **SSH public key**.
     - **Username**: Enter a username.
     - **SSH public key source**: Select **Generate a new key pair**.
     - **Key pair name**: Enter `test-ubuntu-cus-vm_key`.

   - **Inbound port rules**:
     - **Public inbound ports**: Select **Allow selected ports**.
     - **Select inbound ports**: Choose **SSH (22)**.

4. **Review + Create**  
   - After configuring the VM, click **Review + create** to validate your settings.
   - If everything is correct, select **Create** to deploy the VM.

5. **Generate SSH Key Pair**  
   - The **Generate new key pair** window will appear. Click **Download private key** to download the private key and create the resource.

6. **Monitor Deployment**  
   - Track the progress of the deployment in the **Deployment details** pane or through the **Notifications** pane.
   - Once deployment is complete, you will receive a notification confirming success.

7. **Access the VM**  
   - Select **Go to resource** to view your VM’s **Overview** page.
   - The **Public IP address** of your VM will be displayed. Use this IP and the SSH key to connect to your VM.

## 4. Describe the options available to create and manage an Azure Virtual Machine

The Azure portal is a simple and user-friendly option for creating and managing resources, but it's not always the most efficient way, especially when dealing with multiple VMs or resources. Azure provides several other tools to streamline the process of creating and managing virtual machines (VMs).

### Options for Creating and Managing Azure Virtual Machines

1. **Azure Resource Manager Templates**
   - **Resource Manager templates** are JSON files that define the resources required for your solution. You can create a template from your VM and use it to deploy new VMs with identical configurations.
   - **Key Feature**: Parameterize fields like VM names, network names, and storage accounts to customize each environment.
   - **Example**: Use the `Export template` option to generate a template from a VM.

2. **Azure CLI (Command-Line Interface)**
   - The **Azure CLI** is a cross-platform tool for managing Azure resources. It's available for Linux, macOS, Windows, or via the Azure Cloud Shell.
   - **Example Command**: Create a VM using the command below:
     ```bash
     az vm create \
         --resource-group TestResourceGroup \
         --name test-wp1-eus-vm \
         --image Ubuntu2204 \
         --admin-username azureuser \
         --generate-ssh-keys
     ```

3. **Azure PowerShell**
   - **Azure PowerShell** is useful for automating tasks and managing resources on Azure.
   - **Example Command**: Use `New-AzVM` to create a new VM:
     ```powershell
     New-AzVm `
         -ResourceGroupName "TestResourceGroup" `
         -Name "test-wp1-eus-vm" `
         -Location "East US" `
         -Image Debian11 `
         -VirtualNetworkName "test-wp1-eus-network" `
         -SubnetName "default" `
         -SecurityGroupName "test-wp1-eus-nsg" `
         -PublicIpAddressName "test-wp1-eus-pubip" `
         -GenerateSshKey `
         -SshKeyName myPSKey
         -OpenPorts 22
     ```

4. **Terraform**
   - **Terraform** allows you to define and manage infrastructure with configuration files in HCL (HashiCorp Configuration Language). 
   - **Feature**: Terraform supports previewing infrastructure changes and applying them to deploy VMs.

5. **Programmatic APIs**
   - Azure offers programmatic access to resources using **REST APIs** or **Client SDKs**. You can integrate VM management into larger applications.
   - **Example**: Create a VM with a **C# SDK**:
     ```csharp
     var azure = Azure
         .Configure()
         .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
         .Authenticate(credentials)
         .WithDefaultSubscription();

     azure.VirtualMachines.Define("test-wp1-eus-vm")
         .WithRegion(Region.USEast)
         .WithExistingResourceGroup("TestResourceGroup")
         .WithExistingPrimaryNetworkInterface(networkInterface)
         .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
         .WithAdminUsername("jonc")
         .WithAdminPassword("aReallyGoodPasswordHere")
         .WithComputerName("test-wp1-eus-vm")
         .WithSize(VirtualMachineSizeTypes.StandardDS1)
         .Create();
     ```

6. **Azure VM Extensions**
   - **VM extensions** are small applications that automate tasks on VMs after initial deployment, such as installing software or configuring settings.
   - For more information, see [Azure Virtual Machine Extensions](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/overview).

7. **Azure Automation Services**
   - **Azure Automation** helps automate frequent, time-consuming tasks, reducing human errors.
   - **Key Features**:
     - **Process Automation**: Automate responses to specific events like error reporting.
     - **Configuration Management**: Manage software updates for VMs.
     - **Update Management**: Automate the application of patches and updates to VMs.

8. **Auto-shutdown**
   - **Auto-shutdown** allows you to automatically turn off your VMs to save costs when they aren't needed. You can set a schedule for daily or weekly shutdowns.
   - Navigate to the **Auto-shutdown** feature in the Azure portal under the **Operations** section for the VM.

## 5. Manage the availability of your Azure VMs

The success of services companies often depends on meeting service level agreements (SLA) with customers, particularly ensuring high availability and data security. Azure provides several tools to enhance VM availability, streamline management tasks, and keep your data safe.

### What is Availability?

**Availability** is the percentage of time a service is accessible. For example, if you have a website and expect customers to always access it, your target would be 100% availability for website access.

### Why Availability Matters in Azure

Azure VMs run on physical servers hosted in Azure datacenters, and like any physical hardware, these servers can fail. If this happens, the VM on that server will also fail. However, Azure will automatically move the VM to a healthy server, though this process could take several minutes, during which the VM may not be available.

Azure also performs periodic maintenance, such as software updates and hardware upgrades, which can sometimes require a reboot of the virtual machine. While Azure tries to minimize disruptions, availability can still be impacted during these updates.

### Key Azure Services for Improving VM Availability

1. **Availability Zones**
   - Availability Zones are physically separate zones within an Azure region, each with its own power, network, and cooling infrastructure.
   - By deploying your VMs across multiple Availability Zones, you protect against the failure of a single data center. If one zone experiences an outage, the replicated VMs in other zones will continue to function, ensuring service continuity.

2. **Virtual Machine Scale Sets**
   - Azure VM Scale Sets allow you to deploy and manage a group of load-balanced VMs that automatically scale based on demand.
   - **Key Feature**: You can deploy VMs across Availability Zones or within a single zone or region. This adds resilience to your application, ensuring high availability.
   - **Cost**: There's no extra cost for the scale set itself; you only pay for the VM instances.

3. **Load Balancer**
   - Azure's **Load Balancer** distributes traffic between multiple VMs to ensure that no single VM is overwhelmed. It works well when combined with Availability Zones or Availability Sets to improve resiliency.
   - **Standard tier VMs** include the Load Balancer by default, while other tiers may not.

4. **Azure Storage Redundancy**
   - **Storage redundancy** ensures that your data is protected by maintaining multiple copies across different physical locations. This guarantees data durability even in the event of failures.
   - **Considerations**: Choose the redundancy option based on factors like cost, required availability, and whether data should be replicated to a geographically distant region for disaster recovery.

5. **Failover Across Locations with Azure Site Recovery**
   - **Azure Site Recovery** allows you to replicate workloads to a secondary location and automatically fail over in the event of a primary site failure.
   - **Benefits**:
     - Eliminates the need for maintaining a secondary physical datacenter.
     - Enables testing of failovers without impacting production environments, ensuring that your disaster recovery plan is functional.
     - Allows you to replicate both Azure-based and on-premises virtual machines.
   
   **Site Recovery Features**:
   - Works with Hyper-V, VMware, and physical servers in on-premises environments.
   - Orchestrates replication, failover, and recovery of workloads, making it a crucial part of your business continuity and disaster recovery strategy.

By leveraging these services, you can ensure the availability and resilience of your virtual machines and data, which is vital for maintaining a reliable service for your customers.

## 6. Back up your virtual machines

Azure Backup is a backup-as-a-service solution for both on-premises and cloud data. It ensures data protection for a variety of use cases, including:

- Files and folders on Windows OS machines (physical/virtual, local/cloud)
- Application-aware snapshots (Volume Shadow Copy Service)
- Microsoft workloads (SQL Server, SharePoint, Exchange)
- Azure Virtual Machines (Windows/Linux)
- Linux and Windows 10 client machines

### Benefits of Azure Backup

- **Automatic Storage Management**: Pay-as-you-go model for storage.
- **Unlimited Scaling**: High availability and scalability.
- **Multiple Storage Options**: LRS (same region) and GRS (secondary region).
- **Unlimited Data Transfer**: No limits or charges for data transfer.
- **Data Encryption**: Secure transmission and storage.
- **Application-Consistent Backup**: Ensures complete restore points.
- **Long-Term Retention**: No limits on backup duration.

### Key Components

1. **Azure Backup Agent**: For individual file/folder backups.
2. **System Center Data Protection Manager**: For Microsoft workloads.
3. **Azure Backup Server**: Centralized backup management.
4. **Azure Backup VM Extension**: For backing up Azure VMs.

Backups are stored in a **Recovery Services vault** with Azure Storage blobs for efficient, long-term storage. Define backup policies to schedule and retain snapshots.

