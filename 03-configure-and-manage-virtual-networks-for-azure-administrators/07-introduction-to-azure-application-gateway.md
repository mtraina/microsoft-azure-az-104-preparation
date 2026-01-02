## 1. Introduction to Azure Application Gateway

Azure Application Gateway is a **Layer 7 load-balancing service** for web applications that provides advanced traffic management and security. Key capabilities include:

- **HTTP load balancing** across a pool of web servers.  
- **Web Application Firewall (WAF)** to inspect and filter malicious traffic (e.g., SQL injection, XSS).  
- **TLS termination** to offload encryption and reduce CPU usage on application servers.  
- **Session affinity** to ensure clients consistently connect to the same back-end server.  
- **High availability** by detecting unavailable servers and stopping traffic routing to them.  

Adatum, an online commerce store, wants to replace aging on-premises hardware that currently performs these functions. Azure Application Gateway replicates this functionality in the cloud, providing traffic management, security, and encryption.

### Learning Objectives

- Understand what Azure Application Gateway is and the functionality it provides.  
- Determine if Azure Application Gateway meets your organization’s requirements.  

### Prerequisites

- Basic networking concepts  
- Familiarity with Azure VMs and Azure App Service  
- Familiarity with Azure virtual networking

## 2. What is Azure Application Gateway?

Azure Application Gateway manages client requests to web apps hosted on a **pool of servers**, which can include:

- Azure Virtual Machines (VMs)  
- Azure Virtual Machine Scale Sets  
- Azure App Service  
- On-premises servers  

Key features of Application Gateway:

- **HTTP load balancing** with round-robin distribution across back-end servers.  
- **Session stickiness** ensures requests in the same session go to the same back-end server, which is critical for scenarios like e-commerce transactions.  
- **Web Application Firewall (WAF)** to protect against common web vulnerabilities.  
- **Support for multiple protocols**: HTTP, HTTPS, HTTP/2, and WebSocket.  
- **End-to-end encryption** for secure communication between clients and back-end servers.  
- **Autoscaling** to dynamically adjust capacity based on traffic.  
- **Connection draining** to gracefully remove servers during maintenance or updates.  

These features ensure high availability, security, and performance for web applications.

## 3. How Azure Application Gateway Works

Azure Application Gateway routes and load balances requests to web servers using a set of integrated components:

**Front-end IP address**  
Receives client requests. Can be public, private, or both. Only one public and one private IP per gateway.

**Listeners**  
Accept traffic on a specific combination of protocol, port, host, and IP address.  
- **Basic listener:** Routes requests based on URL path.  
- **Multi-site listener:** Routes requests using both hostname and path.  
Handles TLS/SSL certificates for secure connections.

**Routing rules**  
Bind listeners to back-end pools and define how to route requests based on hostname and path.  
Include HTTP settings: protocol, session stickiness, connection draining, request timeout, and health probes.

**Load balancing**  
Uses **Layer 7 round-robin** distribution, based on hostnames and URL paths. Session stickiness ensures all requests from the same client session go to the same back-end server.

**Web Application Firewall (WAF)**  
Optional component that inspects incoming requests before reaching listeners. Protects against threats like SQL injection, cross-site scripting, command injection, and HTTP protocol anomalies.  
Uses OWASP Core Rule Sets (CRS) 2.2.9 – 3.2 for attack detection, customizable per requirement.

**Back-end pools**  
Collection of servers (VMs, VM scale sets, Azure App Service, or on-premises servers).  
Load balancing distributes requests among servers. TLS/SSL certificates may be required for secure communication.

**Routing methods**  
- **Path-based routing:** Directs requests to different back-end pools based on URL paths.  
- **Multiple-site routing:** Supports multiple web applications on the same gateway using different hostnames and back-end pools.  
Additional features: redirection, HTTP header rewriting, and custom error pages.

**TLS/SSL termination**  
Offloads CPU-intensive encryption tasks from back-end servers. Can optionally re-encrypt traffic for end-to-end encryption. Ensures back-end servers are not directly exposed to the internet.

**Health probes**  
Determine which servers in a back-end pool are healthy. Servers returning HTTP status 200–399 are considered healthy. Prevents routing to nonresponsive servers.

**Autoscaling**  
Automatically adjusts instance count based on traffic load, removing the need to predefine size during provisioning.

**WebSocket and HTTP/2 support**  
Enable efficient, bidirectional communication over long-lived TCP connections with lower overhead and better resource utilization.

Application Gateway ensures secure, scalable, and efficient traffic management for web applications while reducing the attack surface of your infrastructure.

## Azure Load Balancing Solutions Summary

## 4. When to Use Azure Application Gateway
Azure Application Gateway is ideal when you need:

1. **Traffic routing to on-premises or Azure back-end pools**  
   - Can route traffic from an Azure endpoint to servers in Adatum’s on-premises datacenter.

2. **High availability with health probes**  
   - Ensures traffic is only directed to healthy servers.

3. **TLS/SSL termination**  
   - Offloads CPU-intensive encryption/decryption from back-end servers.

4. **Web Application Firewall (WAF)**  
   - Blocks malicious traffic like SQL injection and cross-site scripting before it reaches back-end servers.

5. **Session affinity (sticky sessions)**  
   - Ensures users’ session state is consistently handled by the same back-end server.

---

### When Not to Use Azure Application Gateway
- Not needed if your web application:  
  - Receives low traffic  
  - Already handles its load efficiently on a single server  
  - Doesn’t require Layer 7 routing, WAF, or session stickiness

---

### Comparison to Other Azure Services

| Service | Layer | Scope | Key Use Case |
|---------|-------|-------|--------------|
| **Application Gateway** | 7 (HTTP/HTTPS) | Regional | Load balancing web apps with WAF, TLS termination, session affinity, path/host-based routing |
| **Azure Front Door** | 7 | Global | Load balancing web apps across multiple regions, fast failover, caching, TLS offload |
| **Azure Traffic Manager** | DNS-based | Global | Distribute traffic across regions using DNS, slower failover, domain-level routing |
| **Azure Load Balancer** | 4 (TCP/UDP) | Regional | Ultra-low latency, high-performance load balancing within a region, millions of requests per second |

---

### Key Takeaways
- **Application Gateway** → Layer 7, advanced routing, WAF, TLS termination, session stickiness  
- **Load Balancer** → Layer 4, regional, ultra-low latency traffic  
- **Front Door** → Global, Layer 7, caching, fast failover  
- **Traffic Manager** → Global, DNS-based routing