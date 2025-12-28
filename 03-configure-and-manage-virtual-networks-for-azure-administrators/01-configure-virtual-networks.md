## 1. Azure Virtual Networks — Introduction Summary

### Overview
- Azure Virtual Networks (VNets) provide private, secure networking in Azure.
- Enable communication between Azure resources, the internet, and on-premises networks.
- Designed to support security, scalability, and availability.

### Use Case
- Supports migration from on-premises to Azure.
- Common in regulated industries (e.g., healthcare) requiring secure connectivity.
- Can extend on-premises data centers or create cloud-only private networks.

### Key Topics
- Virtual network creation and configuration
- Subnetting and IP address planning
- Private vs public IP address usage
- Common VNet deployment scenarios

### Learning Objectives
- Understand Azure VNet components and features
- Use subnets effectively
- Choose appropriate IP address types
- Create VNets and assign IP addresses

## 2. Plan Virtual Networks — Summary

### Why Use Azure Virtual Networks
- Enable migration of on-premises resources to the cloud.
- Reduce costs and operational complexity.
- Eliminate the need for physical datacenters and infrastructure.
- Especially beneficial for small and medium-sized organizations.

### Azure Virtual Network Basics
- Provides logical isolation of Azure resources.
- Represents a virtual version of an on-premises network.
- Supports VPN creation and management in Azure.
- Uses CIDR blocks for IP address ranges.
- Can connect to:
  - Other Azure virtual networks
  - On-premises networks (non-overlapping CIDR required)
- Allows control over:
  - DNS settings
  - Network segmentation using subnets

### Common Virtual Network Scenarios

#### Dedicated Cloud-Only Network
- Resources communicate securely within Azure.
- Internet access configured only when required.

#### Secure Datacenter Extension
- Uses site-to-site VPNs.
- IPsec provides secure connectivity between on-premises and Azure.

#### Hybrid Cloud Scenarios
- Connects cloud applications with on-premises systems.
- Supports diverse platforms such as mainframes and Unix systems.

### Key Planning Considerations
- Careful IP addressing to avoid overlaps.
- Subnet design for security and organization.
- Planning for future connectivity and scalability.

## 3. Create Subnets — Summary

### Purpose
- Subnets divide a virtual network into smaller segments.
- Improve security, performance, and management.

### IP Address Rules
- Subnet ranges must:
  - Be within the VNet address space
  - Be unique and non-overlapping
  - Use CIDR notation
- Azure reserves **five IP addresses per subnet**.

### Reserved IP Addresses (Example: 192.168.1.0/24)

| Reserved IP Address | Purpose |
|---------------------|---------|
| 192.168.1.0 | Network address |
| 192.168.1.1 | Default gateway |
| 192.168.1.2–192.168.1.3 | Azure DNS |
| 192.168.1.255 | Broadcast address |

### Key Planning Considerations
- Some services require dedicated subnets (e.g., VPN Gateway).
- Subnet traffic routing can be customized using network virtual appliances.
- Each subnet can have one Network Security Group (NSG).
- Private Link provides private access to Azure services.

## 4. Create Virtual Networks — Summary

### Overview
- Virtual networks (VNets) can be created anytime, including during VM creation.
- They provide a private, isolated network in Azure.

### Key Requirements
- Define an **IP address space** not already in use.
- Address space can be cloud-only or on-premises, but not both initially.
- Once created, the IP address space **cannot be changed**.
- At least **one subnet** is required:
  - Each subnet must have a unique, non-overlapping IP range within the VNet.
- VNets are created in the Azure portal by specifying:
  - Subscription
  - Resource group
  - VNet name
  - Region

### Notes
- Default Azure networking limits may change; check current documentation.

## 5. Plan IP Addressing — Summary

### Overview
- Azure resources need IP addresses to communicate with:
  - Other Azure resources
  - On-premises networks
  - The internet
- Two types of IP addresses:
  - **Private IP**: For internal communication within VNets and on-premises networks.
  - **Public IP**: For internet-facing communication.

### IP Address Characteristics
- Can be **static** or **dynamic**:
  - **Static IPs**: Do not change; ideal for:
    - DNS name resolution
    - IP-based security
    - TLS/SSL certificates
    - Firewall rules
    - Role-based servers (e.g., Domain Controllers, DNS)
  - **Dynamic IPs**: Assigned automatically and can change.

### Best Practices
- Separate static and dynamic IPs into different subnets.
- Plan IP addressing carefully to support connectivity and security.

## 6. Create Public IP Addressing — Summary

### Overview
- Public IP addresses allow Azure resources to communicate with the internet.
- Can be created in the Azure portal, e.g., for virtual machines.

### Key Configuration Settings
- **IP Version**: IPv4, IPv6, or both.
- **SKU**: Basic or Standard; must match associated load balancer.
- **Name**: Unique within the resource group.
- **IP Address Assignment**:
  - **Dynamic**: Assigned when the resource starts; may change if deallocated. Remains same on reboot.
  - **Static**: Assigned at creation; remains until deleted. Changing assignment may not be possible if associated with a resource.

### Notes
- IPv6 addresses with Basic SKU must use Dynamic assignment.
- Standard SKU addresses are Static for both IPv4 and IPv6.

## 7. Associate Public IP Addresses — Summary

### Overview
- Public IP addresses can be associated with:
  - Virtual machine network interfaces
  - Internet-facing load balancers
  - VPN gateways
  - Application gateways
- Supports both **dynamic** and **static** public IPs.
- Note: Basic SKU public IPs retired on September 30, 2025.

### Resource Association Table

| Resource Type | IP Address Configuration |
|---------------|-------------------------|
| Virtual Machine | Network interface |
| Virtual Network Gateway (VPN/ER), NAT Gateway | Gateway IP configuration |
| Public Load Balancer, Application Gateway, Azure Firewall, Route Server, API Management | Front-end configuration |
| Bastion Host | Public IP configuration |

### Standard SKU Features

| Feature | Standard SKU |
|---------|--------------|
| Allocation Method | Static |
| Security | Secure by default |
| Availability Zones | Supported (nonzonal, zonal, or zone-redundant) |

## 8. Allocate or Assign Private IP Addresses — Summary

### Overview
- Private IP addresses can be associated with:
  - Virtual machine network interfaces (NICs)
  - Internal load balancers
  - Application gateways
- Supports **dynamic** and **static** IP assignment.

### Private IP Association Table

| Resource | Private IP Association | Dynamic | Static |
|----------|----------------------|---------|--------|
| Virtual Machine | NIC | Yes | Yes |
| Internal Load Balancer | Front-end configuration | Yes | Yes |
| Application Gateway | Front-end configuration | Yes | Yes |

### Private IP Assignment

- **Dynamic**: Azure assigns the next available unassigned IP in the subnet.
  - Example: If 10.0.0.4–10.0.0.9 are taken, Azure assigns 10.0.0.10.
- **Static**: You select an unassigned IP in the subnet range.
  - Example: For subnet 10.0.0.0/16, any IP from 10.0.0.10–10.0.255.254 can be assigned if others are in use.

## 9. Exercise: Create and Configure Virtual Networks — Summary

### Scenario
- Organization migrating a web-based application to Azure.
- Task: Create virtual networks and subnets, and configure secure peering.

### Requirements
- **Two virtual networks**: `app-vnet` and `hub-vnet` (hub-and-spoke architecture).
- **app-vnet**:
  - Two subnets:
    - Frontend subnet for web servers
    - Backend subnet for database servers
- **hub-vnet**:
  - One subnet for the firewall
- Virtual networks must communicate securely via **VNet peering**.
- Both VNets located in the same region.

### Skills Practiced
- Create virtual networks
- Create subnets
- Configure virtual network peering (optional)

### Key Takeaways — Summary

- **Azure Virtual Networks (VNets)** provide secure, isolated networking for cloud resources.
- Multiple VNets can be created per region per subscription.
- Ensure **VNet CIDR blocks** do not overlap with existing network ranges.
- **Subnets** are IP address ranges within a VNet:
  - Can be sized and segmented for organization and security.
  - Each subnet must have a unique address range.
- Some services, like **Azure Firewall**, require dedicated subnets.
- **Virtual network peering** allows seamless connectivity between VNets, making them appear as a single network.


