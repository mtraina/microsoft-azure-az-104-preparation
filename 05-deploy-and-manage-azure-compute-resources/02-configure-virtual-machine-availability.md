## 1. Configure Virtual Machine Availability (AZ-104)

### Overview
This module covers the essentials of configuring **virtual machine availability** in Azure, focusing on dynamic scaling and high availability to manage fluctuating workloads.

### Learning Objectives
- Implement **availability sets** and **availability zones**.
- Set up **update domains** and **fault domains**.
- Deploy and manage **Azure Virtual Machine Scale Sets**.
- Configure **autoscaling** for VMs.

## 2. Plan for Maintenance and Downtime

### Overview
Azure Administrators must plan for both **unplanned** and **planned maintenance** to ensure high availability and minimize downtime.

### Key Concepts

- **Unplanned Hardware Maintenance**: Azure predicts potential failures in hardware and migrates virtual machines (VMs) to healthy machines using **Live Migration**. This operation causes a short pause, and performance may be reduced.
  
- **Unexpected Downtime**: Failures like local network issues or disk failures trigger **automatic migration** to healthy hardware in the same datacenter. VMs may experience downtime during this recovery process, and in some cases, temporary storage may be lost.

- **Planned Maintenance**: Periodic updates by **Microsoft** to enhance platform **reliability**, **performance**, and **security**. Azure updates hardware and platform software, but you must manage **VM OS** and other software updates independently.

### Note
Microsoft does **not automatically update your VM OS** or software—those updates are your responsibility.

## 3. Create Availability Sets

### Overview
An **availability set** ensures that a group of related virtual machines (VMs) are deployed together, reducing the risk of downtime during maintenance or failure. This grouping helps avoid a single point of failure and ensures VMs are not all impacted during a host OS upgrade.

### Key Concepts

- **VM Grouping**: VMs in an availability set should:
  - Perform the same functions.
  - Have the same software installed.
  
- **Distribution Across Resources**: Azure ensures VMs in an availability set are distributed across multiple physical servers, compute racks, storage units, and network switches.

- **Failure Impact**: In the event of hardware or Azure software failure, only a subset of VMs in the set will be affected. This ensures application uptime and availability.

- **VM Creation**: 
  - You can create VMs and availability sets simultaneously.
  - A VM can only be added to an availability set during its creation; changes require deletion and recreation of the VM.

- **Creation Tools**: Availability sets can be created using the Azure portal, ARM templates, scripting, or APIs.

- **SLAs**: Microsoft provides **SLAs** for VMs and availability sets. For more details, see the [SLA for Azure Virtual Machines](https://azure.microsoft.com/en-us/support/legal/sla/).

### Important Notes
- **OS/Application Failures**: Adding VMs to an availability set does not protect against **OS** or **application-level** failures. Additional disaster recovery or backup strategies are needed for this level of protection.
  
### Considerations

- **Redundancy**: Place multiple VMs in an availability set to achieve redundancy.
  
- **Separation of Tiers**: Separate application tiers into different availability sets to avoid a single point of failure across all machines.

- **Load Balancing**: Use **Azure Load Balancer** to distribute traffic across VMs for high availability and optimal network performance.

- **Managed Disks**: Use **Azure Managed Disks** for block-level storage with VMs in availability sets.

## 4. Review Update Domains and Fault Domains

### Overview
Azure Virtual Machine Availability Sets use **update domains** and **fault domains** to enhance high availability and fault tolerance when deploying and upgrading applications. Each VM in an availability set is placed in one update domain and one fault domain.

### Update Domains

- **Definition**: A group of VMs and associated physical hardware that are upgraded together during a service upgrade or rollout.
  
- **Characteristics**:
  - VMs in an update domain are updated and rebooted together during planned maintenance.
  - Only one update domain is rebooted at a time during maintenance.
  - By default, there are **5 update domains** (non-configurable).
  - You can configure up to **20 update domains**.

### Fault Domains

- **Definition**: A group of nodes that represent a physical unit of failure, typically a server rack.
  
- **Characteristics**:
  - A fault domain defines VMs sharing the same hardware (or switches) with a single point of failure (e.g., a server rack with shared power or network).
  - Two fault domains work together to mitigate hardware failures, network outages, power interruptions, or software updates.

### Example Scenario

- **Two Fault Domains**: Each containing **two VMs** in different availability sets.
  - **Web Availability Set**: Contains one VM from each fault domain.
  - **SQL Availability Set**: Contains one VM from each fault domain.

This setup ensures that failures in one fault domain won't affect the availability of all VMs.

## 5. Review Availability Zones

### Overview
**Availability Zones** are a high-availability solution that protects applications and data from datacenter failures by distributing resources across multiple zones within an Azure region.

### Key Characteristics

- **Physical Locations**: Availability zones are unique physical locations within an Azure region.
- **Datacenter Independence**: Each zone contains one or more datacenters with independent power, cooling, and networking.
- **Minimum Zones**: All enabled regions have a minimum of three separate availability zones for resiliency.
- **Datacenter Failure Protection**: Physical separation of zones ensures protection from datacenter failures.
- **Zone-Redundant Services**: Replicate applications and data across zones to protect against single points of failure.

### Things to Consider

Azure services supporting availability zones are divided into two categories:

#### Zonal Services
Resources are pinned to a specific zone.
- Examples:
  - **Azure Virtual Machines**
  - **Azure Managed Disks**
  - **Standard IP Addresses**

#### Zone-Redundant Services
The platform automatically replicates resources across zones for high availability.
- Examples:
  - **Zone-Redundant Azure Storage**
  - **Azure SQL Database**

**Tip**: For comprehensive business continuity, combine availability zones with **Azure regional pairs** for enhanced protection.

## 6. Compare Vertical and Horizontal Scaling

### Vertical Scaling
- **Definition**: Vertical scaling (scale up/scale down) adjusts the size of a single virtual machine to meet workload demands. This involves increasing or decreasing the virtual machine's resources (e.g., CPU, memory).
  
#### Scenarios for Vertical Scaling:
- **Under-utilization**: Scale down during off-peak times (e.g., weekends) to reduce costs.
- **Higher Demand**: Scale up to handle increased demand without creating additional VMs.

### Horizontal Scaling
- **Definition**: Horizontal scaling (scale out/scale in) adjusts the number of virtual machine instances based on workload changes. This involves adding (scale out) or removing (scale in) VMs.

#### Scenarios for Horizontal Scaling:
- **Increased Load**: Scale out to add more virtual machines to handle larger loads.
- **Decreased Load**: Scale in by removing unnecessary VMs to optimize cost.

### Considerations
#### Vertical Scaling:
- **Limitations**: Dependent on available larger hardware, can hit upper limits. Requires VM restart, causing temporary downtime.
- **Flexibility**: Less flexible compared to horizontal scaling.

#### Horizontal Scaling:
- **Limitations**: Fewer limitations, more scalable for cloud environments.
- **Flexibility**: Highly flexible, supporting potentially thousands of VMs to adjust to workloads.

#### Reprovisioning:
- Some scaling processes may require **reprovisioning** (replacing a VM with a new one), which can impact service continuity. Plan for potential downtime and data migration if needed.

## 7. Implement Azure Virtual Machine Scale Sets

### What are Azure Virtual Machine Scale Sets?
- **Definition**: Azure Virtual Machine Scale Sets allow you to deploy and manage a set of identical virtual machines (VMs) that automatically scale based on demand. They ensure high availability, scalability, and ease of management.

### Key Characteristics:
- **Uniform Configuration**: All VMs are created from the same base OS image and configuration, making it easier to manage a large number of machines.
- **Autoscaling**: Scale sets automatically adjust the number of VM instances based on application demand. This can be done manually or automatically.
- **Load Balancing**: Support for Azure Load Balancer (Layer 4) and Azure Application Gateway (Layer 7) for distributing traffic and ensuring availability.
- **High Availability**: If a VM instance fails, others take over with minimal disruption to users.
- **Scaling Limits**: Supports up to 1,000 VM instances. Custom images have a limit of 600 instances.

### Benefits:
- **App Availability & Scalability**: Automatically scales to handle changing workloads.
- **Reduced Manual Configuration**: Simplifies management of large-scale services and workloads.

## 8. Create Virtual Machine Scale Sets

### Steps to Create Scale Sets in Azure Portal:
1. **Orchestration Mode**:
   - **Flexible Mode**: Manually create and add VMs with custom configurations.
   - **Uniform Mode**: Define a base VM model, and Azure automatically creates identical instances.

2. **Image**: Choose the OS or application base for the VMs.

3. **VM Architecture**: 
   - **x64**: Most compatible for software.
   - **Arm64**: Offers better price-performance (up to 50% more cost-effective).

4. **Size**: Select the appropriate VM size based on the workload requirements (CPU, memory, storage). Azure charges hourly based on the size and OS.

### Advanced Settings:
- **Spreading Algorithm**:
  - **Max Spreading**: VMs spread across as many fault domains as possible in each zone.
  - **Fixed Spreading**: VMs are spread across exactly five fault domains (fails if fewer than five are available).
  
  **Recommendation**: Use **Max Spreading** for better fault tolerance.

## 9. Implement Autoscale

### Autoscaling in Azure Virtual Machine Scale Sets:
Autoscaling automatically adjusts the number of virtual machine instances based on changing workload demands. This ensures that the correct number of instances are running to meet performance requirements without overprovisioning resources.

### Key Benefits:
1. **Automatic Adjusted Capacity**: Create autoscaling rules to maintain optimal performance based on defined thresholds. When thresholds are met, the system automatically adjusts the VM capacity.
  
2. **Scale Out**: When demand increases, autoscaling adds more virtual machines to meet the load. This helps maintain performance during high-demand periods.

3. **Scale In**: When demand decreases (e.g., during off-peak times), autoscaling reduces the number of virtual machines, saving costs by only running the required instances.

4. **Scheduled Events**: Autoscaling can be scheduled to increase or decrease capacity at specific times, allowing for predictable scaling.

5. **Reduced Overhead**: Autoscaling reduces manual management tasks and optimizes application performance without requiring constant monitoring.

## 10. Configure Autoscale

### Scaling Modes in Azure Virtual Machine Scale Sets:
1. **Manual Scaling**: 
   - Set a fixed number of virtual machines (VMs) for the scale set.
   - Configure the **Scale-in policy** to determine the order in which VMs are deleted when scaling down.

2. **Autoscaling**:
   - **Automatic Scaling**: Adjust the number of VMs based on performance metrics or schedules.
   - Define a **maximum** and **minimum** number of VMs to be used during autoscaling.

### Autoscale Configuration:

1. **Default Instance Count**: Initial number of VMs deployed in the scale set (0 - 1000).

2. **Instance Limits**:
   - **Minimum Instance Count**: The least number of VMs the scale set should scale down to.
   - **Maximum Instance Count**: The greatest number of VMs the scale set should scale up to.

3. **Scaling Conditions**:
   - **Scale Out**: Trigger autoscaling when CPU usage exceeds a defined threshold. This increases the number of VMs.
   - **Scale In**: Trigger autoscaling when CPU usage drops below a defined threshold. This reduces the number of VMs.

4. **Query Duration**: The time period that the Autoscale engine looks back to calculate the average metric usage.

5. **Schedule**: Define the start and end dates for scaling actions, or repeat the schedule on specific days.

## 12. Summary and Key Takeaways

In this module, you learned how to manage and configure **virtual machine availability** on Azure using various tools and services:

### 1. **Azure Virtual Machine Scale Sets**:
   - Deploy and manage identical VMs, making it easier to scale services.
   - Supports **autoscaling**, adjusting the number of instances based on workload demand.

### 2. **Autoscaling**:
   - Optimizes performance by automatically scaling the number of VMs in or out based on predefined metrics (e.g., CPU usage).

### 3. **Availability Sets and Zones**:
   - **Availability Sets**: Group VMs to ensure they are distributed across multiple physical servers for high availability.
   - **Availability Zones**: Physically separate data centers within a region for redundancy and fault tolerance.

### 4. **Vertical and Horizontal Scaling**:
   - **Vertical Scaling**: Adjusts the size of a VM to handle larger workloads.
   - **Horizontal Scaling**: Adjusts the number of VMs based on application demands.

## Resources for Further Learning

### Learn More with Copilot:
- Copilot helps with configuring Azure infrastructure and answering questions. Access it via Microsoft Edge or at [copilot.microsoft.com](https://copilot.microsoft.com).

### Documentation:
- [Availability options for Azure Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/availability)
- [Autoscale with Azure Virtual Machine Scale Sets](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)
- [Create virtual machines in a scale set using Azure portal](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/quick-create-portal)

### Self-Paced Training:
- [Introduction to Azure Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/)
- [Implement scale and high availability with Windows Server VM](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/availability)
- [Introduction to Azure Virtual Machine Scale Sets](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)
