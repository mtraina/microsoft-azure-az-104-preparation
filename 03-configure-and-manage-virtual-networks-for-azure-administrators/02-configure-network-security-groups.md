## 1. Network Security Groups — Introduction Summary

### Overview
- Network Security Groups (NSGs) control network traffic to Azure resources.
- Use security rules to **allow or deny inbound and outbound traffic**.
- Applied to virtual machine networking and Azure services.

### Use Case Scenario
- Company migrating key systems to the cloud with strict security needs.
- Requires tight control over which computers can access application servers.
- Goal is to block unwanted or unsecured network traffic.

### Purpose of the Module
- Learn how to control and secure network traffic using NSGs.
- Focus on building secure, AI-ready network environments.

### Learning Objectives
- Know when to use network security groups.
- Create and configure NSGs.
- Implement and evaluate inbound and outbound rules.
- Understand the role of application security groups.

### Prerequisites
- Familiarity with Azure virtual networks and virtual machines.
- Basic experience using the Azure portal.
- Understanding of traffic routing and traffic control concepts.

## 2. Implement Network Security Groups — Summary

### Overview
- Network Security Groups (NSGs) control network traffic to Azure resources.
- NSGs can be assigned to:
  - Subnets
  - Network interfaces (NICs)
- Security rules define allowed or denied inbound and outbound traffic.

### Key Characteristics
- NSGs contain a list of security rules.
- A single NSG can be associated multiple times.
- NSGs and rules are created and managed in the Azure portal.
- VM overview shows associated NSGs, subnets, NICs, and rules.

### NSGs and Subnets
- Assigning an NSG to a subnet secures all resources in that subnet.
- Can be used to create a **DMZ (screened subnet)**.
- Each subnet can have **one** associated NSG.

### NSGs and Network Interfaces
- NSGs can be applied directly to NICs.
- Control traffic for a specific virtual machine.
- Each NIC can have **zero or one** associated NSG.

## 3. Determine Network Security Group Rules — Summary

### Overview
- NSG security rules filter network traffic to and from:
  - Subnets
  - Network interfaces
- Rules control **inbound** and **outbound** traffic flow.

### Default Security Rules
- Azure automatically creates default rules in every NSG.
- Default rules include:
  - **DenyAllInbound**
  - **AllowInternetOutbound**
- Default rules cannot be removed.
- You can override them by creating a rule with a **higher priority**.

### Security Rule Components

| Setting | Description |
|--------|-------------|
| Source | Any, IP addresses, My IP, Service Tag, or Application Security Group |
| Source Port Ranges | Ports to allow or deny |
| Destination | Any, IP addresses, Service Tag, or Application Security Group |
| Protocol | TCP, UDP, ICMP, or Any |
| Action | Allow or Deny |
| Priority | Value from 100–4096 (lower number = higher priority) |

### Rule Processing
- Rules are processed in **priority order**.
- The first matching rule is applied.

### Inbound Rules
- Default inbound rules:
  - Allow traffic from the virtual network
  - Allow traffic from Azure load balancers
  - Deny all other inbound traffic

### Outbound Rules
- Default outbound rules:
  - Allow traffic to the virtual network
  - Allow traffic to the internet
  - Deny all other outbound traffic

## 4. Determine Network Security Group Effective Rules — Summary

### Rule Evaluation Order
- NSGs are evaluated independently at **subnet** and **NIC** levels.
- **Inbound traffic**:
  - Subnet NSG rules evaluated first
  - Then NIC NSG rules
- **Outbound traffic**:
  - NIC NSG rules evaluated first
  - Then subnet NSG rules
- Intra-subnet traffic (VMs in the same subnet) is also evaluated.

### Effective Rule Processing
- Both inbound and outbound rules are processed by **priority**.
- The first matching rule determines the outcome.
- Applying NSGs at both subnet and NIC levels affects final traffic behavior.

### Design Considerations
- **Allow all traffic**: Don’t associate an NSG if traffic control isn’t needed.
- **Allow rules required**: Traffic must be allowed at both subnet and NIC levels.
- **Intra-subnet traffic**: Can be blocked using deny rules in subnet NSGs.
- **Rule priority**:
  - Lower number = higher priority
  - Leave gaps (e.g., 100, 200, 300) for future rules

### Viewing Effective Rules
- Use **Effective security rules** in the Azure portal to see applied rules.
- **Network Watcher** provides a consolidated view of network security rules.

## 5. Create Network Security Group Rules — Summary

### Overview
- NSG rules control inbound and outbound traffic.
- Rules are created and managed in the Azure portal.
- Supports many services (e.g., HTTPS, RDP, FTP, DNS).

### Key Rule Properties

- **Source**
  - Controls inbound traffic.
  - Can be an IP range, resource, application security group, or service tag.

- **Destination**
  - Controls outbound traffic.
  - Same filtering options as Source.

- **Service**
  - Defines protocol and port range.
  - Can use predefined services (RDP, SSH) or custom ports.

- **Priority**
  - Determines rule processing order.
  - Lower number = higher priority.

### Tips
- Plan rules based on required traffic and services.
- Use priority gaps to allow future rule additions.

## 6. Implement Application Security Groups — Summary

### Overview
- Application Security Groups (ASGs) group virtual machines by workload.
- Used as **source or destination** in Network Security Group (NSG) rules.
- Provide an application-centric approach to network security.

### How ASGs Work
- Virtual machine NICs are assigned to ASGs.
- NSG rules reference ASGs instead of IP addresses.
- Simplifies managing traffic between application tiers.

### Example Scenario
- Two-tier application:
  - **Web servers**: Handle HTTP/HTTPS traffic.
  - **Application servers**: Process SQL requests.
- Security rules:
  - Allow internet access to web servers on ports 80 and 443.
  - Allow SQL traffic (port 1433) from web servers to application servers.
  - Deny HTTP/HTTPS access to application servers from all other sources.

### Benefits of ASGs
- Reduce IP address management complexity.
- No requirement to separate workloads into subnets.
- Simplify and reduce the number of NSG rules.
- Automatically apply rules to all VMs in an ASG.
- Improve clarity by organizing security around workloads.

### ASGs vs Service Tags
- **ASGs**: Group virtual machines by application or role.
- **Service Tags**: Represent IP ranges for Azure services.

## 7. Exercise: Implement Virtual Networking — Summary

### Scenario
- Organization needs restricted access to virtual machines.
- Goal is to control and secure VM network access.

### Tasks
- Create and configure Network Security Groups (NSGs).
- Associate NSGs with virtual machines.
- Allow and deny VM access using NSG rules.

### Skills Practiced
- Create virtual networks and subnets via:
  - Azure portal
  - Templates
- Configure communication between:
  - Application Security Groups (ASGs)
  - Network Security Groups (NSGs)
- Configure public and private Azure DNS zones (optional).

### Notes
- Estimated time: 50 minutes.
- Requires an Azure subscription.
