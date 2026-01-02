## 1. Azure Virtual Network Peering

### Overview
- Provides secure, private connectivity between Azure virtual networks across the same or different regions.

### Scenario & Goals
- Connect multiple VNets used by separate business units.
- Enable private communication without exposing services to the internet.
- Maintain a simple and effective network design with transit support.

### Key Concepts
- Azure VNet peering for direct connectivity.
- Azure VPN Gateway for transit routing.
- Hub-and-spoke architecture using user-defined routes and service chaining.

### Learning Outcomes
- Understand VNet peering use cases and features.
- Configure VPN Gateway for transit connectivity.
- Design scalable peering network architectures.

## 2. Azure Virtual Network Peering Uses

### Overview
- Azure Virtual Network peering is a fast and simple way to connect two VNets.
- Peered VNets behave like a single network for connectivity, while remaining separate resources.

### Types of Peering
- **Regional peering**: Connects VNets within the same Azure region.
- **Global peering**: Connects VNets across different Azure regions.

### Scope and Limitations
- Supported within the same cloud environment (Public, China, or Government).
- Global peering is not supported between different Azure Government regions.
- VNets can be peered across subscriptions and tenants.
- Each VNet remains independently managed.

### Key Benefits
- **Private connectivity**: Traffic stays on the Azure backbone with no internet exposure.
- **High performance**: Low latency and high bandwidth.
- **Simplified communication**: Direct connectivity between resources in different VNets.
- **Flexible data transfer**: Works across regions, subscriptions, and deployment models.
- **No downtime**: Peering can be created without disrupting resources.

## 3. Gateway Transit and Connectivity

### Overview
- Azure Virtual Network peering can use an Azure VPN Gateway as a transit point.
- A peered virtual network can access external resources through a remote VPN gateway.

### Hub-and-Spoke Scenario
- Multiple VNets are peered with a central hub VNet.
- The hub VNet contains a gateway subnet and a single Azure VPN Gateway.
- Spoke VNets use the hub’s VPN gateway for connectivity.
- Gateway transit must be enabled on the hub VNet.

### Portal Configuration Notes
- Gateway transit is configured through peering traffic options rather than a specific setting name.
- You control traffic flow by allowing or forwarding network traffic.

### Azure VPN Gateway Key Points
- Each virtual network supports only one VPN gateway.
- Gateway transit works with both regional and global peering.
- Enables access to external resources without deploying multiple gateways.

### Supported Connectivity
- Site-to-site VPN to on-premises networks.
- VNet-to-VNet connections.
- Point-to-site VPN connections for clients.

### Security Considerations
- Network security groups (NSGs) can allow or block traffic between VNets or subnets.
- NSG rules can be opened or restricted during peering configuration.

## 4. Create Virtual Network Peering

### Overview
- Azure Virtual Network peering can be created using the Azure portal, PowerShell, or Azure CLI.
- This module focuses on creating peering through the Azure portal for ARM-based deployments.

### Prerequisites
- Azure account must have the **Network Contributor** role or equivalent permissions.
- Two virtual networks are required.
- One network is designated as the **remote virtual network**.

### Connectivity Behavior
- Virtual machines in separate VNets cannot communicate before peering.
- Once peering is established, communication is allowed based on configuration settings.

### Peering Status Monitoring
- Peering status can be checked in the Azure portal.
- Peering is fully established only when both VNets show a **Connected** status.

### Status States
- **Initiated**: Initial peering created from the first VNet.
- **Connected**: Both VNets have completed peering.
- **Updating**: Used in the classic deployment model.

### Regional and Global Peering
- Azure Global VNet peering enables connectivity across Azure regions.

## 5. Extend Peering with User-Defined Routes and Service Chaining

### Nontransitive Nature of Peering
- Virtual network peering is **nontransitive**.
- Peering between VNet A–B and B–C does not allow communication between A and C.
- Additional configuration is required to enable broader connectivity.

### Extension Mechanisms

#### Hub-and-Spoke Network
- A central hub VNet hosts shared resources such as:
  - Network virtual appliances (NVAs)
  - Azure VPN gateways
- Spoke VNets peer with the hub.
- Traffic between spokes flows through the hub infrastructure.

#### User-Defined Routes (UDRs)
- UDRs can specify the next hop as:
  - A virtual machine in a peered VNet
  - A VPN gateway
- Enables control over traffic routing beyond basic peering.

#### Service Chaining
- Directs traffic through virtual appliances or gateways.
- Implemented using UDRs that point to:
  - NVAs in peered VNets
  - Virtual network gateways
- Commonly used for inspection, security, or centralized routing.

### Outcome
- Hub-and-spoke with UDRs and service chaining enables controlled, transitive-like connectivity across multiple VNets.

## 6. Lab 05 – Implement Intersite Connectivity

### Lab Overview
- Explore communication between virtual networks by implementing **VNet peering**.
- Test connectivity between VMs and create **custom routes**.
- Typical scenario: Separate core IT services from manufacturing or other business units.

### Lab Tasks

#### Task 1: Create Core Services VM and VNet
- Create a **CoreServicesVM** in a new VNet **CoreServicesVnet**:
  - Address range: 10.0.0.0/16, Subnet: 10.0.0.0/24
  - VM: Windows Server 2025 Datacenter, Standard_DS2_v3
  - No public inbound ports, Boot diagnostics disabled

#### Task 2: Create Manufacturing VM and VNet
- Create a **ManufacturingVM** in a new VNet **ManufacturingVnet**:
  - Address range: 172.16.0.0/16, Subnet: 172.16.0.0/24
  - VM: Windows Server 2025 Datacenter, Standard_DS2_v3
  - No public inbound ports, Boot diagnostics disabled

#### Task 3: Test Connectivity with Network Watcher
- Use **Connection Troubleshoot** to test VM-to-VM connectivity.
- Initial test shows **Unreachable** because VNets are not peered.

#### Task 4: Configure Virtual Network Peering
- Create peering between **CoreServicesVnet** and **ManufacturingVnet**.
- Enable:
  - Access between VNets
  - Forwarded traffic
- Verify both peering links show **Connected**.

#### Task 5: Test Connectivity with PowerShell
- Use **Test-NetConnection** on ManufacturingVM to test connection to CoreServicesVM private IP.
- Peering enables successful connectivity.

#### Task 6: Create a Custom Route
- Add a **perimeter subnet** in CoreServicesVnet: 10.0.1.0/24
- Create a **route table (rt-CoreServices)** and add a route:
  - Destination: 10.0.0.0/16
  - Next hop: Virtual appliance (future NVA at 10.0.1.7)
- Associate route table with Core subnet to direct traffic through the NVA.

### Key Takeaways
- By default, VNets cannot communicate; **peering enables connectivity**.
- Peered VNets appear as one for routing purposes.
- Traffic uses the **Microsoft backbone network**, not the internet.
- **System-defined routes** exist per subnet; **user-defined routes** override or supplement them.
- **Azure Network Watcher** helps monitor and troubleshoot connectivity.

### Additional Learning
- Use **Azure PowerShell or CLI** to automate VNet peering.
- Explore custom routes to control traffic flow.
- Extend learning with **Copilot** for guidance on Azure networking scenarios.

## 8. Azure Virtual Network Peering – Summary

### Key Takeaways
- **Hub-and-spoke connectivity**: VNets can be connected in a hub-and-spoke topology using peering.
- **Peering types**:
  - **Regional**: Connects VNets within the same region.
  - **Global**: Connects VNets across different regions.
- **Private traffic**: Network traffic stays on the Azure backbone; no internet exposure.
- **Transit connectivity**: Azure VPN Gateway in a peered VNet can provide access to other networks.
- **Security controls**: Network security groups (NSGs) can block or allow traffic between VNets during peering.

### Additional Learning Resources
- **Copilot**: Assists with designing Azure infrastructure, comparing products, and explaining configurations.
- **Documentation**:
  - Overview of Azure Virtual Network peering.
  - Instructions for creating, changing, or deleting a peering.
- **Self-Paced Training**:
  - Introduction to Azure Virtual Networks.
  - Hands-on sandbox for distributing services and enabling communication with VNet peering.
