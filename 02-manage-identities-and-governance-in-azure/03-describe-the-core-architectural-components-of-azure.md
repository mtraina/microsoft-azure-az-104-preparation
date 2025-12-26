## 2. What Is Microsoft Azure

### Overview

**Microsoft Azure** is a continually expanding cloud computing platform that helps organizations meet current and future business challenges. It enables you to build, manage, and deploy applications on a massive global network using a wide range of tools and frameworks.

### What Azure Offers

- **Limitless innovation**: Build intelligent applications and solutions using advanced cloud, AI, and data services.
- **Bring ideas to life**: Use industry-leading AI and cloud technologies on a trusted platform.
- **Seamless unification**: Manage infrastructure, data, analytics, and AI solutions efficiently through a single integrated platform.
- **Innovate on trust**: Rely on Microsoft’s strong focus on security, compliance, and responsible cloud practices.

### What You Can Do With Azure

- Access **100+ cloud services** covering compute, storage, networking, AI, analytics, and more.
- Run existing applications on **virtual machines (VMs)** in the cloud.
- Go beyond VMs by using cloud-native services that scale automatically and reduce infrastructure management.
- Build advanced solutions using **AI and machine learning**, including services for vision, speech, and natural language.
- Store and manage massive amounts of data with **scalable storage solutions**.

### Key Takeaway

Azure is more than just a place to host virtual machines—it is a comprehensive cloud platform that enables innovation, scalability, and intelligent solutions that are not possible with traditional on-premises infrastructure.

## 3. Get Started with Azure Accounts

To use Azure services, you need an **Azure account** and at least one **subscription**, which organizes resources and billing. You can create multiple subscriptions under one account to separate workloads like development or production.

### Free Account Options
- **Azure Free Account**: 12 months of free services, a 30-day credit, and 25+ always-free products (credit card required for verification).
- **Azure Free Student Account**: $100 credit for 12 months, free developer tools, and no credit card required.

### Important Notes
- Most learning labs require your own subscription.
- Always clean up resources after exercises to avoid unexpected costs.

## 4. Exercise – Explore Interacting with Azure

This exercise introduces different ways to interact with Microsoft Azure, primarily through the **Azure portal** and the **Azure command-line interface (CLI)**.

### Azure Portal
- Web-based graphical interface at **portal.azure.com**
- Used to manage subscriptions, accounts, and Azure resources
- Provides access to the Azure CLI through **Cloud Shell**

### Azure CLI and Cloud Shell
- Cloud Shell runs directly in the portal
- Supports **PowerShell** and **BASH**
- PowerShell uses commands like `Get-Date`
- BASH uses standard Linux commands like `date`
- Azure-specific commands typically start with `az`

### Switching Environments
- Switch between PowerShell and BASH using `pwsh` or `bash`
- CLI prompt indicates the active shell

### Azure CLI Interactive Mode
- Started with `az interactive`
- Provides autocompletion, command help, and examples
- Commands can be run without typing `az`
- Exit using the `exit` command

The exercise demonstrates how to choose the right interface depending on your workflow and familiarity with command-line tools.

## 5. Describe Azure Physical Infrastructure

Azure’s core architecture is built on **physical infrastructure** and **management infrastructure**. This unit focuses on the physical side.

### Datacenters and Regions
- Azure datacenters are global facilities with power, cooling, and networking.
- Datacenters are grouped into **regions**, which are geographic areas with one or more nearby datacenters connected by low-latency networks.
- Some services are region-specific, while others (like Microsoft Entra ID and Azure DNS) are global.

### Availability Zones
- **Availability zones** are physically separate datacenters within a region.
- Each zone has independent power, cooling, and networking to provide fault isolation.
- At least three zones exist in zone-enabled regions.
- Used to improve resiliency for services such as VMs, managed disks, load balancers, and SQL databases.

### Zone Service Types
- **Zonal**: Pinned to a specific zone (e.g., VMs).
- **Zone-redundant**: Automatically replicated across zones (e.g., zone-redundant storage).
- **Non-regional**: Globally available and resilient to zone and region failures.

### Region Pairs
- Most regions are paired with another region in the same geography, at least 300 miles apart.
- Region pairs support disaster recovery, planned update sequencing, and data residency.
- Failover and replication may require customer configuration, depending on the service.

### Sovereign Regions
- Isolated Azure instances for compliance or legal needs.
- Examples include U.S. Government regions and China regions operated with local partners.

## 6. Describe Azure Management Infrastructure

Azure management infrastructure organizes and governs resources, subscriptions, and accounts.

### Resources and Resource Groups
- **Resource**: Basic Azure component (VMs, databases, networks, apps, etc.).
- **Resource group**: Logical container for resources; actions on a group affect all contained resources.
- Resources can only belong to one group; groups cannot be nested.
- Useful for grouping by environment, access schema, or lifecycle.

### Azure Subscriptions
- Units for **management, billing, and scale**.
- Link to an Azure account (identity in Microsoft Entra ID).
- Multiple subscriptions allow separate **billing** and **access control**.
- Common uses:
  - Separate environments (dev/test/prod)
  - Reflect organizational structures
  - Track and manage costs

### Azure Management Groups
- Containers above subscriptions for governance and policy management.
- Policies and access assigned to a management group automatically inherit to all nested subscriptions, resource groups, and resources.
- Can have nested groups, up to six levels deep.
- Benefits:
  - Apply policies across multiple subscriptions
  - Assign access to multiple subscriptions efficiently
  - Enable large-scale enterprise governance

## 7. Exercise - Create an Azure Resource

This exercise demonstrates creating a virtual machine (VM) in Azure and observing how resource groups organize resources.

### Task 1: Create a Virtual Machine
1. Sign in to the [Azure portal](https://portal.azure.com).  
2. Navigate: **Create a resource > Compute > Virtual Machine > Create**.  
3. In the **Basics** tab, enter the following:

| Setting | Value |
|---------|-------|
| Subscription | Select your subscription |
| Resource group | Create new: `IntroAzureRG` |
| VM name | `my-VM` |
| Region | Default |
| Availability options & zone | Default / self-selected zone |
| Security type | Default |
| Image & VM architecture | Default |
| Run with Azure Spot discount | Unchecked |
| Size | Default |
| Authentication | Password: `azureuser` + custom password |
| Public inbound ports | None |

4. Select **Review + Create**, then **Create**.  
5. Wait until **Deployment is complete**.

### Task 2: Verify Resources Created
1. Go to **Home > Resource groups**.  
2. Select `IntroAzureRG` to view the VM and all associated resources automatically created in the resource group.

### Clean Up
1. Navigate to **Resource groups**.  
2. Select `IntroAzureRG` and choose **Delete resource group**.  
3. Confirm by entering `IntroAzureRG` and delete to avoid extra costs.

**Outcome:** You created a VM and observed Azure automatically grouping associated resources in a resource group.

