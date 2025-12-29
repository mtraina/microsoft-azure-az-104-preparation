## 1. Host Your Domain on Azure DNS — Introduction Summary

### Overview
- Azure DNS hosts DNS records using Azure infrastructure.
- Uses the same Azure credentials, APIs, tools, and billing as other Azure services.
- Provides a managed DNS hosting solution.

### Use Case Scenario
- Company purchases a custom domain from a third-party registrar.
- Needs DNS hosting to resolve the domain name to a web server IP.
- Uses Azure DNS to manage domain records alongside existing Azure resources.

### Module Focus
- Configure Azure DNS to host a custom domain.
- Add DNS records (including aliases) to resolve domain names to websites.

### Learning Objective
- Set up and manage domain hosting using Azure DNS.

### Prerequisites
- Basic understanding of DNS concepts and IP addressing.

## 2. What Is Azure DNS? — Summary

### DNS Basics
- DNS translates human-readable domain names into IP addresses.
- Uses a global network of DNS servers.
- DNS servers:
  - Cache recent lookups for faster resolution
  - Store authoritative records for domains

### How DNS Resolution Works
- Check local cache first.
- If not found, query other DNS servers.
- Cache results when found or return an error if unresolved.

### IP Address Standards
- **IPv4**: Four decimal numbers (most common today).
- **IPv6**: Eight hexadecimal groups, designed to replace IPv4.
- DNS can resolve names to both IPv4 and IPv6 addresses.

### DNS Records and Zones
- DNS records are stored in **zones**.
- Common record types:
  - **A / AAAA**: Map domain to IPv4 / IPv6 address
  - **CNAME**: Alias to another domain
  - **MX**: Mail routing
  - **TXT**: Verification and metadata
- **NS** and **SOA** records are created automatically in Azure DNS.
- Record sets allow multiple values in a single record (except CNAME and SOA).

### Azure DNS Overview
- Managed DNS hosting using Azure infrastructure.
- Acts as the **Start of Authority (SOA)** for your domain.
- Uses Azure credentials, APIs, tools, and billing.
- Domains must be registered with a third-party registrar.

### Why Use Azure DNS
- Integrated with Azure Resource Manager.
- Benefits include:
  - Improved security
  - Ease of management
  - Private DNS zones
  - Alias record sets
- Does not support DNSSEC.

### Security Features
- Role-Based Access Control (RBAC)
- Activity logs
- Resource locking

### Private DNS Zones
- Provide name resolution within and across virtual networks.
- Support all common DNS record types.
- Automatically manage VM hostnames.
- Support split-horizon DNS (public and private zones).

### Alias Record Sets
- Point DNS records directly to Azure resources.
- Supported for A, AAAA, and CNAME records.

## 3. Configure Azure DNS to Host Your Domain — Summary

### Overview
- Azure DNS hosts DNS records for a domain using DNS zones.
- Supports both **public** and **private** DNS zones.
- Domains must be registered with a third-party registrar.

### Public DNS Zone Configuration

#### Step 1: Create a DNS Zone
- Create a DNS zone for the domain (e.g., `wideworldimports.com`).
- Specify subscription, resource group, domain name, and location.

#### Step 2: Get Azure DNS Name Servers
- Retrieve name server (NS) records from the DNS zone.

#### Step 3: Update Domain Registrar
- Update the registrar’s NS records to match Azure DNS name servers.
- Use all four Azure-provided name servers (domain delegation).

#### Step 4: Verify Delegation
- Verify delegation by querying the **SOA** record.
- Example:
  - `nslookup -type=SOA wideworldimports.com`

#### Step 5: Configure DNS Records
- **A record**: Maps a hostname to an IP address.
- **CNAME record**: Creates an alias to another domain or Azure resource.
- Used to resolve services like web servers, load balancers, or functions.

### Private DNS Zone Configuration

#### Purpose
- Provides name resolution within Azure virtual networks.
- Not publicly accessible and doesn’t require a domain registrar.

#### Steps
- Create a private DNS zone (e.g., `private.wideworldimports.com`).
- Identify virtual networks that need name resolution.
- Link virtual networks to the private DNS zone using virtual network links.

## 4. Exercise: Create a DNS Zone and an A Record Using Azure DNS — Summary

### Overview
- Objective: Set up an Azure DNS zone for a domain and create an A record.
- DNS zones host configuration records for your domain.
- NS and SOA records are created automatically.

---

### Step 1: Create a DNS Zone
1. Sign in to the Azure portal.
2. Select **Create a resource** > Search for **DNS zone** > **Create**.
3. Enter the following:
   - **Subscription:** Your Azure subscription
   - **Resource group:** Choose an existing or create a new one
   - **Name:** `wideworldimportsXXXX.com` (unique in the sandbox)
4. Click **Review + create**, then **Create**.
5. After deployment, go to the resource **Overview** pane and select **Record Sets**.
6. Note the NS records — you’ll need them to delegate the domain.

---

### Step 2: Create an A Record
1. In the DNS zone, select **Record sets** > **+ Add**.
2. Enter the following details:
   - **Name:** `www` (host name)
   - **Type:** `A`
   - **Alias record set:** `No`
   - **TTL:** `1` hour
   - **IP Address:** `10.10.10.10` (example)
3. Click **Add** to create the record.
4. Optionally, add multiple IP addresses if needed.

---

### Step 3: Verify DNS Zone
1. Use **nslookup** to verify the A record:
   ```bash
   nslookup www.wideworldimportsXXXX.com <name server address>
   
   nslookup www.wideworldimportsXXXX.com ns1-04.azure-dns.com
   ```
The result should resolve to 10.10.10.10

Notes
- A records map hostnames to IP addresses.
- NS records are required for domain delegation.
- SOA records act as the authoritative start-of-authority for your domain.
- This setup demonstrates Azure DNS functionality even without a registered domain.

## 5. Dynamically Resolve Resource Names Using Azure DNS Alias Records — Summary

### Overview
- Goal: Link the apex domain (root domain) to Azure resources like load balancers.
- Problem: Standard A and CNAME records cannot directly reference Azure resources at the zone apex.

---

### Apex Domain
- The apex domain is the highest-level domain in a DNS zone, e.g., `wideworldimports.com`.
- Represented by the `@` symbol in DNS records.
- NS and SOA records are automatically created for the apex domain.
- CNAME records **cannot** be used at the apex level.

---

### Alias Records
- Alias records in Azure DNS allow zone apex domains to point to Azure resources.
- Supported targets:
  - Azure Traffic Manager profile
  - Azure Content Delivery Network endpoints
  - Public IP resources
  - Front Door profile
- Supported DNS types for alias records:
  - **A** (IPv4)
  - **AAAA** (IPv6)
  - **CNAME**

---

### Advantages of Alias Records
1. **Prevents dangling DNS records:** Keeps DNS records up to date automatically.
2. **Automatic updates:** Changes to the target resource's IP are automatically reflected in DNS.
3. **Load-balanced applications:** Supports routing to Azure Traffic Manager at the apex.
4. **Integration with Azure CDN:** Zone apex can directly reference CDN endpoints.

---

### Key Use Case
- Use an alias record to point `wideworldimports.com` (apex) to a load balancer.
- Ensures continuity even if the IP of the load balancer changes.
- Simplifies DNS management by linking the lifecycle of DNS records directly with Azure resources.

## 6. Exercise: Create alias records for Azure DNS

Your website deployment was successful, but high traffic is putting strain on a single web server. To distribute the load, you can use a load balancer and an Azure alias record to dynamically link the zone apex to the load balancer.

### Objectives

- Set up a virtual network with two VMs and a load balancer.
- Configure an Azure alias record at the zone apex to point to the load balancer.
- Verify that the domain name resolves to one or either of the VMs.

---

### Step 1: Set up the virtual network, load balancer, and VMs

To speed up the setup, use the Bash script from GitHub.

```bash
git clone https://github.com/MicrosoftDocs/mslearn-host-domain-azure-dns.git
cd mslearn-host-domain-azure-dns
chmod +x setup.sh
./setup.sh
```

The setup script takes a few minutes to run. The script:

- Creates a network security group.
- Creates two network interface controllers (NICs) and two VMs.
- Creates a virtual network and assigns the VMs.
- Creates a public IP address and updates the configuration of the VMs.
- Creates a load balancer that references the VMs, including rules for the load balancer.
- Links the NICs to the load balancer.

After the script completes, it shows you the public IP address for the load balancer. Copy the IP address to use it later.

### Create an Alias Record in Your Zone Apex

Now that your test environment is set up, you can create an **Azure alias record** at the zone apex to link your domain directly to the load balancer.

### Steps in the Azure Portal

1. **Open Resource Groups**
   - In the Azure portal, select **Resource groups**.
   - The **Resource groups** pane will appear.

2. **Select Your Resource Group**
   - Click on your resource group.
   - The **Resource group** pane appears.

3. **Open Your DNS Zone**
   - In the list of resources, select the DNS zone you created in the previous exercise, e.g., `wideworldimportsXXXX.com`.
   - The DNS zone pane appears.

4. **Add a Record Set**
   - In the menu bar, select **+ Record set**.
   - The **Add record set** pane appears.

5. **Configure the Alias Record**
   | Setting            | Value                                                                                  |
   |-------------------|----------------------------------------------------------------------------------------|
   | Name              | Leave blank (represents the zone apex for `wideworldimportsXXXX.com`)                  |
   | Type              | A (base record type must be A, AAAA, or CNAME even for alias records)                  |
   | Alias record set  | Yes                                                                                     |
   | Alias type        | Azure resource                                                                          |
   | Azure resource    | Select `myPublicIP` from the list of resources (may take up to 15 minutes to appear)   |

6. **Add the Record**
   - Click **OK** to add the alias record to your DNS zone.
   - Once created, the alias record should appear in the DNS zone listing.

### Verify the Alias Record

1. **Copy the Public IP**
   - In the Azure portal, go to the resource group.
   - Select **myPublicIP**.
   - Click the **Copy** icon next to the IP address.

2. **Test in a Browser**
   - Paste the copied public IP into a web browser.
   - You should see a basic web page displaying the **name of the VM** that the load balancer routed your request to.

> This confirms that the alias record correctly resolves to the load balancer, distributing traffic between your VMs.

## 7. Summary: Hosting Your Domain on Azure DNS

Your company purchased the custom domain **wideworldimporters.com** from a third-party registrar to host a new website. To manage DNS for this domain and point it to your Azure-based web server, you used **Azure DNS** as the hosting service.

### Key Steps You Completed

1. **Created an Azure DNS Zone**
   - Centralized all DNS management within Azure.
   - Enabled easier administration of resources and domain name records.

2. **Updated NS Records at the Domain Registrar**
   - Delegated the domain to Azure DNS by updating Name Server (NS) records.

3. **Configured Record Sets**
   - **A Record**: Maps a domain name to an IPv4 address.
   - **AAAA Record**: Maps a domain name to an IPv6 address.
   - **CNAME Record**: Provides an alias to another domain name.
   - **NS Record**: Specifies authoritative name servers for the zone.
   - **SOA Record**: Provides the start of authority information for the zone.

4. **Used Azure Alias Records**
   - Enabled dynamic references to Azure resources, such as load balancers or public IPs.
   - Avoided static A/AAAA/CNAME records for zone apex domains.
   - Simplified routing and lifecycle management of Azure resources.

### Benefits of Using Azure DNS

- **Centralized Management**: One interface for all DNS and Azure resource management.
- **Better Integration**: Seamlessly links DNS to Azure resources.
- **Improved Security & Monitoring**: Offers built-in security and monitoring tools.
- **Reduced Complexity**: Easier than managing complex redirection via a third-party registrar.