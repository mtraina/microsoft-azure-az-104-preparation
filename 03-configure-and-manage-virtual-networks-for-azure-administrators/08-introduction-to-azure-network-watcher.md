## 1. Azure Network Watcher Overview

### What is Azure Network Watcher
Azure Network Watcher is a service that allows organizations to **detect and monitor network performance issues** for Azure infrastructure as a service (IaaS) resources. It provides insights into connectivity, packet flow, and network health across virtual networks.

### Key Use Case for Adatum
- Adatum runs several **three-tier applications** on VMs migrated from on-premises datacenters.  
- Applications are hosted across **multiple Azure virtual networks**.  
- Some applications are **hybrid**, with compute tiers in both on-premises and cloud environments.  
- As a DevOps engineer, you are responsible for monitoring the **network performance and health** of these resources.

## 2. Azure Network Watcher

Azure Network Watcher monitors, diagnoses, and manages the network health of **Azure IaaS resources** like VMs, VNets, application gateways, and load balancers.  
> Not designed for PaaS or web analytics.

### Key Capabilities
- **Monitoring**: Topology & Connection Monitor  
- **Diagnostics**: IP Flow Verify, NSG Diagnostics, Next Hop, Effective Security Rules, Connection Troubleshoot, Packet Capture, VPN Troubleshoot  
- **Traffic**: Flow Logs, Traffic Analytics

### Monitoring
- **Topology**: Visualizes resources and their relationships across subscriptions and regions.  
- **Connection Monitor**: End-to-end connectivity checks for Azure and hybrid endpoints.

### Diagnostics
- **IP Flow Verify**: Checks if traffic is allowed/denied.  
- **NSG Diagnostics**: Troubleshoots NSG rules and can add rules.  
- **Next Hop**: Detects routing issues.  
- **Effective Security Rules**: Shows all rules applied to a NIC and subnet.  
- **Connection Troubleshoot**: Tests connectivity at a specific time.  
- **Packet Capture**: Records network traffic to/from VMs.  
- **VPN Troubleshoot**: Diagnoses virtual network gateways and connections.

### Traffic
- **Flow Logs**: Logs IP traffic through NSGs or VNets.  
- **Traffic Analytics**: Visualizes flow log data.

## 3. How Azure Network Watcher Works

Azure Network Watcher automatically becomes available when you create a virtual network in an Azure region. Access it via the Azure portal search bar.

### Topology Tool
Visualizes all resources and relationships in a virtual network:
- Subnets, NICs, NSGs, Load Balancers & Health Probes
- Public IPs, VNets, Peering, Gateways, VPN connections
- VMs & VM Scale Sets

**Resource properties:**
- Name, ID, Location
- Associations: Type (Contains/Associated), Name, Resource ID

### Connection Monitor
- End-to-end connectivity monitoring for Azure and hybrid deployments
- Measures latency and detects network changes (e.g., NSG modifications)
- Requires lightweight monitoring agents on VMs or hosts

### Network Diagnostic Tools
- **IP Flow Verify:** Checks if packets (5-tuple) are allowed/denied at VM level  
- **Next Hop:** Determines the next hop type and IP for a packet; verifies routing  
- **Effective Security Rules:** Shows combined NSG rules affecting a NIC  
- **Packet Capture:** Records network traffic using filters; stores data locally or in a storage blob  
- **Connection Troubleshoot:** Tests TCP connectivity and reports latency, hops, and faults:
  - Faults: CPU, Memory, GuestFirewall, DNSResolution, NetworkSecurityRule, UserDefinedRoute  
- **VPN Troubleshoot:** Diagnoses gateways/connections, returning:
  - `startTime`, `endTime`, `code`, `results`, `summary`, `detailed`, `recommendedActions`, `actionText`, `actionUri`, `actionUriText`

### Summary
Network Watcher tools enable monitoring, diagnostics, and troubleshooting of Azure IaaS networking, supporting hybrid and cloud scenarios.

## 4. When to Use Azure Network Watcher

Azure Network Watcher is useful for troubleshooting networking issues in Azure IaaS environments.

### Common Scenarios

1. **Resolve connectivity issues for IaaS VMs**
   - Use **IP Flow Verify** to check if packets are allowed or denied.
   - Specify local/remote IP, ports, protocol, and direction (inbound/outbound).
   - Identifies which NSG rule might be blocking traffic (e.g., remote PowerShell over TCP 5986).

2. **Troubleshoot VPN connections**
   - Use **VPN Troubleshoot** for site-to-site VPNs.
   - Diagnoses virtual network gateway connections.
   - Provides health reports and suggested resolution steps.

3. **Determine cross-region network latency**
   - Measure latency between VMs in different regions.
   - Helps decide optimal placement of IaaS resources.
   - Useful for hybrid applications to compare on-premises vs. cloud performance.

### When Not to Use
- Network Watcher is limited to **Azure IaaS resources**.
- Does **not** support PaaS services or web analytics.
- Lacks advanced diagnostics that some third-party tools provide.

