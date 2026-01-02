## 1. Introduction to Azure Load Balancer

Adatum, an expanding online store, has multiple three-tier applications running on **Azure VMs** that were migrated from on-premises. Some applications are public-facing, while others are restricted to the main office in Sydney.

- **Background:**  
  Previously, on-premises hardware devices used **Layer 4 (OSI model) load balancing** to:  
  - Distribute incoming traffic across web-tier and middle-tier VMs.  
  - Allow RDP connections to individual VMs for administration.  
  - Stop sending traffic to failed VMs.  
  - Maintain session affinity so that client traffic goes to a single VM in the back-end pool.

- **Azure Load Balancer:**  
  Azure Load Balancer replicates these functions in the cloud:  
  - Distributes inbound traffic across a back-end pool of **Azure VMs** or **Virtual Machine Scale Set instances**.  
  - Uses **load-balancing rules** to define how traffic is distributed.  
  - Uses **health probes** to detect unresponsive VMs and avoid sending traffic to them.

- **Learning Objectives:**  
  - Understand what Azure Load Balancer is and what it does.  
  - Determine if Azure Load Balancer fits your organization’s requirements.

- **Prerequisites:**  
  - Basic understanding of networking concepts.

## 2. What is Azure Load Balancer?

When a single server is overwhelmed by traffic, **load balancing** distributes incoming requests across multiple servers to improve responsiveness and efficiency. Azure Load Balancer provides this functionality in the cloud.

- **Purpose:**  
  Evenly distribute network traffic across **Azure VMs** or **Virtual Machine Scale Set instances** for high availability and better performance.

- **Key Features:**  
  - **Load-balancing rules:** Define how traffic is sent to back-end instances.  
  - **Health probes:** Detect unhealthy resources to avoid sending traffic to them.

- **Types of Azure Load Balancer:**  
  - **Public Load Balancer:**  
    - Balances **internet traffic** to Azure VMs.  
    - Maps public IP/port to private IP/port of back-end VMs.  
    - Can provide **outbound connections** for VMs inside a virtual network.  
  - **Internal (Private) Load Balancer:**  
    - Balances traffic **within a virtual network** or via VPN.  
    - Front-end IPs are private and not exposed to the internet.  
    - Commonly used for **line-of-business (LOB)** or multi-tier applications.

- **Use Cases for Internal Load Balancer:**  
  - Load balancing within a virtual network.  
  - Cross-premises load balancing from on-premises systems.  
  - Multi-tier applications where back-end tiers are not internet-facing.  
  - LOB applications hosted in Azure without additional hardware/software.

- **Scalability:**  
  Can handle millions of TCP and UDP application flows for both inbound and outbound scenarios.

## 3. How Azure Load Balancer Works

Azure Load Balancer operates at **Layer 4 (transport layer)** of the OSI model. It manages traffic based on **source/destination IP, port, and protocol (TCP/UDP)**. Its components work together to ensure **high availability** and **performance**.

### Key Components

- **Front-end IP**  
  - Public IP: Used by clients to access the application; traffic is distributed to back-end VMs.  
  - Private IP: Used internally within a virtual network; traffic is routed via VPN or ExpressRoute for internal apps.

- **Load Balancer Rules**  
  - Define how traffic is mapped from front-end IP/port to back-end IP/port.  
  - Traffic uses a **five-tuple hash** (source IP, source port, destination IP, destination port, protocol).  
  - Supports multiple front-end IPs and multiple ports.  
  - **Layer 4 only:** For Layer 7 (application-aware) routing, use Azure Application Gateway.

- **Back-end Pool**  
  - Collection of VMs or VM Scale Set instances that respond to traffic.  
  - Automatically scales and redistributes traffic when instances are added or removed.

- **Health Probes**  
  - Check the health of back-end instances.  
  - TCP, HTTP, or HTTPS probes ensure traffic is not sent to unhealthy instances.  
  - Failing probes do not terminate existing connections; new traffic is rerouted.

- **Session Persistence (Affinity)**  
  - Default: No persistence (any healthy VM handles requests).  
  - Options:  
    - **Client IP (2-tuple):** Same VM handles requests from same client IP.  
    - **Client IP + protocol (3-tuple):** Same VM handles requests from same client IP and protocol.

- **High Availability Ports (HA Ports)**  
  - A single rule can load balance **all TCP/UDP flows across all ports**.  
  - Useful for high traffic scenarios and NVAs.

- **Inbound NAT Rules**  
  - Map traffic from a load balancer's public IP to specific back-end VM ports.  
  - Enables external access like RDP or SSH.

- **Outbound Rules**  
  - Configure **SNAT** for back-end instances to communicate with the internet or public endpoints.

### Summary

Azure Load Balancer ensures efficient traffic distribution at Layer 4 using front-end IPs, rules, back-end pools, and health probes. It supports session persistence, HA ports, inbound NAT, and outbound rules to meet both public and internal load-balancing needs.

## 4. When to Use Azure Load Balancer

Azure Load Balancer is ideal for **applications requiring ultra-low latency and high performance**, especially when replacing on-premises hardware load balancers. Key use cases include:

- Replacing Layer 4 hardware devices for traffic distribution across multiple VM tiers.  
- Ensuring high availability using **health probes** to avoid sending traffic to failed VM nodes.  
- Maintaining **session persistence** so clients communicate with the same VM during a session.  
- Configuring **public load balancers** for internet-facing web tiers.  
- Configuring **internal load balancers** to balance traffic between internal tiers, like web and data-processing tiers.  
- Using **inbound NAT rules** to allow remote administrative access to VMs (e.g., RDP/SSH).

### When Not to Use Azure Load Balancer

Azure Load Balancer is **not suitable** for:

- Single VM web applications with minimal traffic where no load balancing is needed.  
- Scenarios requiring **Layer 7 (application-aware) load balancing**, TLS/SSL offloading, caching, or global routing.

### Alternatives for Layer 7 or Global Load Balancing

- **Azure Front Door**: Global load balancing with TLS/SSL offload, path-based routing, WAF, and caching.  
- **Azure Traffic Manager**: DNS-based load balancing across global regions; slower failover due to DNS limitations.  
- **Azure Application Gateway**: Regional Layer 7 load balancer with TLS/SSL offload and web farm optimization.  

**Comparison**:  
- Azure Load Balancer is **Layer 4**, ultra-low-latency, handles millions of requests per second, and is **zone-redundant**.  
- Not suitable if web application firewall (WAF) or Layer 7 functionality is required.