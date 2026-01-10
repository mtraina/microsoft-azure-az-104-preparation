## 1. Azure Container Instances (ACI) Introduction

- **Purpose:** Run applications in isolated containers with fast startup and easy management  
- **Comparison:** Containers vs Virtual Machines – lightweight, faster, and more flexible  
- **Use Cases:** AI-ready workloads, microservices, short-lived jobs, batch tasks  
- **Key Features:** Quick deployment, scaling, and integration with Azure services  
- **Container Groups:** Deploy multiple containers together with shared resources  
- **Goal:** Understand when to use ACI and implement container groups effectively

## 2. Containers vs Virtual Machines

- **Virtualization Type:**  
  - Containers: OS-level virtualization, multiple apps share the OS but remain isolated  
  - VMs: Hardware-level virtualization, complete OS per VM  

- **Isolation:**  
  - Containers: Lightweight isolation, less strict security boundary  
  - VMs: Strong isolation from host and other VMs  

- **Operating System:**  
  - Containers: Runs user-mode only, minimal services, low resource usage  
  - VMs: Full OS including kernel, higher resource usage  

- **Deployment:**  
  - Containers: Docker CLI or orchestrators (e.g., AKS)  
  - VMs: Windows Admin Center, Hyper-V, PowerShell, or SCVMM  

- **Persistent Storage:**  
  - Containers: Azure Disks (local), Azure Files (shared)  
  - VMs: VHD (local), SMB shares (shared)  

- **Fault Tolerance:**  
  - Containers: Orchestrator recreates containers on another node  
  - VMs: Failover to another server, OS restarts  

- **Benefits of Containers:**  
  - Faster, flexible development and deployment  
  - Simplified testing and higher workload density  
  - Efficient resource utilization

## 3. Azure Container Instances (ACI)

- **Overview:**  
  - Fastest, simplest way to run containers in Azure  
  - No VMs to manage; ideal for isolated container workloads  

- **Container Images:**  
  - Portable, standalone package including:  
    - Code, runtime, system tools, libraries, settings  
  - Containers = running instances of images  

- **Benefits of ACI:**  
  - **Fast startup:** Containers start in seconds  
  - **Public connectivity:** Containers get public IP and FQDN  
  - **Custom sizing:** Dynamic scaling of container resources  
  - **Persistent storage:** Mount Azure Files shares directly  
  - **OS support:** Run Linux or Windows containers  
  - **Co-scheduled groups:** Multiple containers sharing a host  
  - **Virtual network deployment:** Deploy containers in Azure VNets

## 4. Azure Container Groups

- **Overview:**  
  - Top-level ACI resource: a collection of containers on the same host  
  - Containers share lifecycle, resources, network, and storage  
  - Similar to a Kubernetes pod  

- **Resources & Deployment:**  
  - Resource allocation = sum of all containers' CPU, memory, GPUs  
  - Deployment options:  
    - **ARM templates:** Recommended when integrating with other Azure services  
    - **YAML files:** Recommended for container-only deployments  

- **Networking:**  
  - Share one external IP and DNS label (FQDN)  
  - Containers share port namespace → no internal port mapping  
  - Expose ports on both container and IP for external access  
  - Deleting a group releases its IP and FQDN  

- **Use cases & scenarios:**  
  - **Web app updates:** One container serves app, another pulls content  
  - **Log collection:** App container outputs logs; logging container stores them  
  - **App monitoring:** Monitoring container checks app health and raises alerts  
  - **Front-end/back-end support:** Separate containers for front-end UI and back-end services  

- **Storage:**  
  - Containers can mount Azure Files shares as volumes

## 5. Azure Container Apps (ACA)

- **Overview:**  
  - Serverless platform for containerized apps  
  - Less infrastructure management, lower cost  
  - Managed Kubernetes-based environment (Dapr, KEDA, Envoy)  
  - No direct access to Kubernetes APIs  

- **Common Uses:**  
  - API endpoints  
  - Background jobs  
  - Event-driven processing  
  - Microservices  

- **Scaling:**  
  - HTTP traffic  
  - Event-driven triggers (queues, events)  
  - CPU or memory load  
  - Any KEDA-supported scaler  
  - Scale to zero supported  

- **Key Features:**  
  - Optimized for microservices and general-purpose containers  
  - Service discovery, traffic splitting  
  - Supports scheduled, on-demand, and event-driven jobs  

- **ACA vs AKS:**

| Feature       | Azure Container Apps (ACA) | Azure Kubernetes Service (AKS) |
|---------------|----------------------------|--------------------------------|
| Overview      | Serverless container PaaS  | Managed Kubernetes cluster     |
| Deployment    | Quick PaaS-style           | Full Kubernetes control        |
| Management    | Simplified, built on AKS  | Granular Kubernetes management |
| Scalability   | HTTP & event-driven autoscale | Pod & cluster autoscaling    |
| Use Cases     | Microservices, serverless  | Complex, long-running apps    |
| Integration   | Logic Apps, Functions, Event Grid | Azure Policy, Monitor, Defender |

## 6. Exercise: Implement Container Instances

https://microsoftlearning.github.io/AZ-104-MicrosoftAzureAdministrator/Instructions/Labs/LAB_09b-Implement_Azure_Container_Instances.html

## 7. Summary and Resources: Azure Container Instances

- **Purpose:**  
  Learn when to use Azure Container Instances (ACI) vs Virtual Machines (VMs) and how to implement container groups.

- **Key Takeaways:**  
  - Containers provide lightweight isolation and use fewer system resources than VMs.  
  - Containers can be deployed individually (Docker) or via orchestrators like Azure Container Apps.  
  - Storage options: Azure Disks (local) or Azure Files (shared).  
  - Container group: a collection of containers scheduled on the same host, sharing resources and lifecycle.  
  - High availability: containers can be quickly recreated on another cluster node if a node fails.
