## 1. Manage and Control Traffic Flow with Azure Routes

### Overview
- Azure virtual networks provide a security perimeter to control inbound and outbound traffic.
- Routing can be customized to restrict access and allow traffic only from trusted sources.

### Scenario
- A retail organization experienced a security breach exposing customer data.
- The remediation plan includes adding **network virtual appliances (NVAs)** to inspect traffic.
- Traffic must be routed through NVAs to detect and block malicious activity.

### Module Focus
- Understand Azure routing capabilities.
- Create and manage custom routes within a virtual network.
- Deploy a basic network virtual appliance.
- Redirect traffic through an NVA for inspection and security enforcement.

### Learning Objectives
- Identify Azure virtual network routing features.
- Configure routing inside a virtual network.
- Deploy and use NVAs to control traffic flow.

### Prerequisites
- Basic networking knowledge (subnets, IP addressing).
- Familiarity with Azure virtual networking.

## 2. Identify Routing Capabilities of an Azure Virtual Network

### Azure Routing Basics
- Azure uses **system routes** by default to manage traffic between subnets, VNets, on-premises networks, and the internet.
- System routes are automatically assigned to each subnet and cannot be created or deleted.
- **Custom routes** can override system routes to control traffic flow.

### Default System Routes
- Enable communication within the virtual network and to the internet.
- Common next hop types:
  - **Virtual network**: Routes traffic within the VNet address space.
  - **Internet**: Sends traffic to the internet (0.0.0.0/0).
  - **None**: Drops traffic for private or non-routable address spaces.

### Additional System Routes
- Created automatically when enabling:
  - Virtual network peering
  - Service chaining
  - Virtual network gateways
  - Service endpoints

### Virtual Network Peering & Service Chaining
- Peering enables communication between VNets in the same or different regions.
- Service chaining allows traffic to be redirected through NVAs or gateways using user-defined routes.

### Virtual Network Gateway
- Provides encrypted connectivity between Azure and on-premises networks or between Azure VNets.
- Maintains routing tables and gateway services.

### Service Endpoints
- Extend private VNet connectivity to Azure services.
- Restrict access to Azure resources from within the private network.
- Automatically add routes to the route table.

### Custom Routes
- Used to control traffic flow more precisely (e.g., route through firewalls or NVAs).
- Implemented using:
  - **User-defined routes (UDRs)**
  - **Border Gateway Protocol (BGP)**

### User-Defined Routes (UDRs)
- Override system routes.
- Supported next hop types:
  - Virtual appliance
  - Virtual network gateway
  - Virtual network
  - Internet
  - None
- Service tags can be used instead of IP ranges to simplify management.

### Border Gateway Protocol (BGP)
- Exchanges routes between Azure and on-premises networks.
- Commonly used with ExpressRoute or site-to-site VPNs.
- Improves resilience and dynamic route updates.

### Route Selection Priority
- Azure selects routes based on:
  1. Longest prefix match
  2. Route type priority:
     - User-defined routes
     - BGP routes
     - System routes
- Duplicate address prefixes are not allowed in UDRs.

### Key Takeaways
- Custom routes are used to **control traffic flow** within a virtual network.
- Virtual network peering connects VNets across regions for private communication.

## 3. Exercise – Create Custom Routes

### Purpose
- Control network traffic flow as part of a security strategy.
- Ensure traffic between public and private subnets is routed through a **network virtual appliance (NVA)** for inspection and monitoring.

### Scenario
- Traffic from a **public subnet** to a **private subnet** must pass through a **perimeter (DMZ) subnet**.
- A custom route is used to redirect traffic to the NVA, which will later be deployed in the DMZ subnet.

### Key Components
- **Route table** with a custom route
- **Virtual network** with three subnets:
  - Public subnet
  - Private subnet
  - DMZ (perimeter) subnet
- **Route table association** with the public subnet

### High-Level Steps

#### Create a Route Table and Custom Route
- Create a route table for the public subnet.
- Add a custom route that:
  - Targets the private subnet address range.
  - Uses a **virtual appliance** as the next hop.
  - Points to the future NVA’s private IP address.

```bash
az network route-table create --name publictable --resource-group "myResourceGroupName" --disable-bgp-route-propagation false

az network route-table route create --route-table-name publictable --resource-group "myResourceGroupName" --name productionsubnet --address-prefix 10.0.1.0/24 --next-hop-type VirtualAppliance --next-hop-ip-address 10.0.2.4
```

#### Create Virtual Network and Subnets
- Create a virtual network with a single address space.
- Add three subnets:
  - Public subnet (front-end servers)
  - Private subnet (internal servers)
  - DMZ subnet (for the NVA)

```bash
az network vnet create --name vnet --resource-group "myResourceGroupName" --address-prefixes 10.0.0.0/16 --subnet-name publicsubnet --subnet-prefixes 10.0.0.0/24

az network vnet subnet create --name privatesubnet --vnet-name vnet --resource-group "myResourceGroupName" --address-prefixes 10.0.1.0/24

az network vnet subnet create --name dmzsubnet --vnet-name vnet --resource-group "myResourceGroupName" --address-prefixes 10.0.2.0/24
```

#### Associate Route Table
- Associate the route table with the **public subnet**.
- This forces all outbound traffic destined for the private subnet to flow through the NVA.

```bash
az network vnet subnet update --name publicsubnet --vnet-name vnet --resource-group "myResourceGroupName" --route-table publictable
```

### Outcome
- Traffic from public to private resources is no longer direct.
- All such traffic is routed through the perimeter subnet, enabling inspection, filtering, and enhanced security.

## 4. Network Virtual Appliance (NVA)

### Overview
- An **NVA** is a virtual machine used to control, inspect, and secure network traffic in Azure.
- Common functions include firewalls, routers, load balancers, proxies, and IDS/IPS.

### Sources
- Available from the **Azure Marketplace** (e.g., Cisco, Check Point, Barracuda, Sophos).

### Purpose
- Filter and block unauthorized or malicious traffic.
- Enforce security between perimeter and internal networks.
- Control traffic flow within and between virtual networks.

### Deployment Models
- **Perimeter (DMZ) subnet**: All traffic is routed through the NVA.
- **Microsegmentation**: Dedicated firewall subnets inspect all traffic (Layer 4/7).

### Networking Considerations
- NVAs act as routers; IP forwarding must be enabled.
- Some NVAs require multiple NICs (management + traffic).
- **User-defined routes (UDRs)** direct traffic through the NVA.

### Availability
- NVAs are critical components.
- High availability is recommended to prevent network outages.

## 5. Exercise - Create an NVA and virtual machines

In this stage, you'll deploy a **Network Virtual Appliance (NVA)** to monitor and secure traffic between front-end public servers and internal private servers.

- **IP Forwarding:** Before the NVA can route traffic correctly, IP forwarding must be enabled. Without it, traffic sent through the appliance will not reach its destination.

- **Deployment Overview:**
  - Deploy the NVA to the `dmzsubnet`.
  - Enable IP forwarding to ensure traffic from the public subnet and traffic using custom routes is sent to the `privatesubnet`.

- **Deployment Steps:**
  1. Deploy an Ubuntu LTS instance as the NVA.
     ```bash 
     az vm create --resource-group "myResourceGroupName" --name nva --vnet-name vnet --subnet dmzsubnet --image Ubuntu2204 --admin-username azureuser --admin-password <password>
     ```
  2. Enable IP forwarding on the Azure network interface to allow the NVA to route traffic properly.
     ```bash
     NICID=$(az vm nic list --resource-group "myResourceGroupName" --vm-name nva --query "[].{id:id}" --output tsv)
     
     echo $NICID

     NICNAME=$(az vm nic show --resource-group "myResourceGroupName" --vm-name nva --nic $NICID --query "{name:name}" --output tsv)
     
     echo $NICNAME

     az network nic update --name $NICNAME --resource-group "myResourceGroupName" --ip-forwarding true
     ```
  3. Enable IP forwarding inside the NVA itself to forward packets at the OS level.
    ```bash
    NVAIP="$(az vm list-ip-addresses --resource-group "myResourceGroupName" --name nva --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"
    
    echo $NVAIP

    ssh -t -o StrictHostKeyChecking=no azureuser@$NVAIP 'sudo sysctl -w net.ipv4.ip_forward=1; exit;'
    ```

- **Notes:**
  - Proper configuration ensures the NVA inspects and forwards traffic securely between subnets.

- **Next Steps:** After deploying and configuring the NVA, the next exercise will guide you on routing traffic through it.

## 6. Exercise - Route traffic through the NVA

In this exercise, you'll route traffic through the **Network Virtual Appliance (NVA)** to control communication between public and private VMs.

- **Prerequisites:** An Azure subscription is required. This exercise is optional.

- **Create Public and Private VMs:**
  - Deploy a VM in the `publicsubnet`.
  - Deploy a VM in the `privatesubnet`.
  - Configure each VM to have the `traceroute` utility installed to test traffic routing.
  ```bash
  code cloud-init.txt

  #cloud-config
  package_upgrade: true
  packages:
    - inetutils-traceroute

  az vm create --resource-group "myResourceGroupName" --name public --vnet-name vnet --subnet publicsubnet --image Ubuntu2204 --admin-username azureuser --no-wait --custom-data cloud-init.txt --admin-password <password>

  az vm create --resource-group "myResourceGroupName" --name private --vnet-name vnet --subnet privatesubnet --image Ubuntu2204 --admin-username azureuser --no-wait --custom-data cloud-init.txt --admin-password <password>

  watch -d -n 5 "az vm list \
    --resource-group "myResourceGroupName" \
    --show-details \
    --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
    --output table"

  PUBLICIP="$(az vm list-ip-addresses --resource-group "myResourceGroupName" --name public --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"
  
  echo $PUBLICIP

  PRIVATEIP="$(az vm list-ip-addresses --resource-group "myResourceGroupName" --name private --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"
  
  echo $PRIVATEIP
  ```

- **Test Traffic Routing:**
  - From the public VM, use `traceroute` to verify traffic flows through the NVA to reach the private VM.
    ```bash
    ssh -t -o StrictHostKeyChecking=no azureuser@$PUBLICIP 'traceroute private --type=icmp; exit'

    traceroute to private.kzffavtrkpeulburui2lgywxwg.gx.internal.cloudapp.net (10.0.1.4), 64 hops max
    1   10.0.2.4  0.710ms  0.410ms  0.536ms
    2   10.0.1.4  0.966ms  0.981ms  1.268ms
    Connection to 52.165.151.216 closed.
    ```
    - Observation: First hop is the NVA; second hop is the private VM.
  - From the private VM, use `traceroute` to verify traffic goes directly to the public VM, bypassing the NVA.
    ```bash
    ssh -t -o StrictHostKeyChecking=no azureuser@$PRIVATEIP 'traceroute public --type=icmp; exit'

    traceroute to public.kzffavtrkpeulburui2lgywxwg.gx.internal.cloudapp.net (10.0.0.4), 64 hops max
    1   10.0.0.4  1.095ms  1.610ms  0.812ms
    Connection to 52.173.21.188 closed.
    ```
    - Observation: Private VM uses default routes; traffic does not go through the NVA.

- **Summary:** Traffic from the public subnet is routed through the NVA in the `dmzsubnet` before reaching the private subnet. The NVA can inspect and block malicious traffic, while traffic from private to public bypasses the NVA.

