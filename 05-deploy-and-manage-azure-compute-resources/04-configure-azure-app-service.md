## 1. Azure App Service – Introduction

Azure App Service is a fully managed platform for hosting **web, mobile, and API applications**, with AI-ready capabilities. It allows organizations to move from on-premises infrastructure to a scalable cloud solution without needing to upgrade aging hardware.

### Key Learning Objectives
- Identify features and use cases for Azure App Service.
- Create and manage apps using App Service.
- Configure deployment settings, including **deployment slots**.
- Secure applications and configure **custom domain names**.
- Back up and restore App Service apps.
- Monitor applications with **Azure Application Insights**.

## 2. Implement Azure App Service

Azure App Service is a fully managed platform for **websites, mobile backends, and APIs** on Windows and Linux. It supports multiple programming languages and scales easily.

### Supported Languages & Frameworks
- ASP.NET, Java, Node.js, Python, PHP
- PowerShell and other scripts/executables as background services

### Key Benefits

| Benefit | Description |
|--------|-------------|
| **Multiple languages & frameworks** | First-class support for popular languages and ability to run background services. |
| **DevOps optimization** | Supports CI/CD with Azure DevOps, GitHub, BitBucket, Docker Hub, and Azure Container Registry. Manage apps via Azure PowerShell or CLI. |
| **Global scale & high availability** | Manual or automatic scaling across Microsoft datacenters with high SLA. |
| **Security & compliance** | ISO, SOC, and PCI compliant. Supports Microsoft Entra ID and social logins. Configure IP restrictions and manage service identities. |
| **Application templates** | Ready-to-use templates from Azure Marketplace, e.g., WordPress, Joomla, Drupal. |
| **Visual Studio integration** | Streamlined tools for creating, deploying, and debugging apps. |
| **API & mobile features** | Turn-key CORS support, authentication, offline sync, push notifications, and other mobile enhancements. |

## 3. Create an App with Azure App Service

Azure App Service allows you to create **Web Apps, Mobile Apps, or API Apps** directly in the Azure portal.

### Key Configuration Settings

- **Name**: Must be unique (e.g., `webappces1.azurewebsites.net`). Custom domains can also be used.
- **Publish**: Host your app as **code** or **Docker container**.
- **Runtime stack**: Choose language and SDK versions (.NET Core, .NET Framework, Node.js, PHP, Python, Ruby). Linux/custom containers can include a startup command/file.
- **Operating system**: Windows or Linux.
- **Region**: Determines available App Service plans.
- **Pricing plan**: Associates your app with an **App Service plan** to define resources, features, and capacity.

### Post-Creation Settings

- **Always On**: Keeps the app loaded even with no traffic. Required for continuous or CRON-triggered WebJobs.
- **Session affinity**: Ensures a client connects to the same instance in multi-instance deployments.
- **HTTPS Only**: Redirects all HTTP traffic to HTTPS.

**Tip:** Practice creating a web app in the Azure portal using a sandbox exercise to explore these settings.

## 4. Continuous Integration and Deployment with Azure App Service

Azure App Service supports **continuous and manual deployment** from multiple sources, automatically syncing code changes to your web app.

### Deployment Options

- **Continuous Deployment (CI/CD)**: Automatically push updates with minimal impact on users.
  - **GitHub**: Deploy directly from your repository; changes to the production branch auto-deploy.
  - **Bitbucket**: Similar to GitHub, supports automated deployment.
  - **Azure Repos**: Use version control to manage code and enable automated deployment.
  - **Local Git**: Deploy by adding a local Git repository URL provided by App Service.

- **Manual Deployment**: Push code updates manually.
  - **Remote Git**: Use the Git URL from App Service to push code.
  - **OneDrive**: Upload files for deployment via your Microsoft account.
  - **Dropbox**: Use file hosting to deploy your app.

**Tip:** Configure deployment in the **Deployment Center** in the Azure portal to choose between continuous and manual deployment.

## 5. Deployment Slots in Azure App Service

Deployment slots allow you to run multiple live versions of your app, separate from the default production slot. They are available in **Standard, Premium, and Isolated** pricing tiers.

### Key Points

- Each slot has its **own hostname**.
- **App content and configuration** can be swapped between slots, including production.
- The number of slots depends on the App Service plan tier.

### Benefits of Using Deployment Slots

- **Validation:** Test changes in a staging slot before deploying to production.
- **Reduced downtime:** Swapping slots ensures all instances are ready; traffic redirection is seamless.
- **Rollback:** If a swap doesn’t work as expected, you can swap back to the previous version.
- **Auto swap:** Automatically swap a slot into production after warming up, enabling continuous deployment with zero downtime. *(Not supported for Web Apps on Linux)*

**Tip:** Use deployment slots to improve release reliability and minimize impact on users.

## 6. Adding Deployment Slots in Azure App Service

Deployment slots can be **added and configured in the Azure portal**. You can swap app content and configuration between slots, including production.

### Key Points About Deployment Slots

- **New slots** can be **empty** or **cloned** from an existing slot.
- **Slot settings categories**:
  1. Slot-specific app settings and connection strings
  2. Continuous deployment settings
  3. Azure App Service authentication settings

- **Cloned configurations** are editable after creation.
- Some configuration elements **follow the content** during a swap, while others remain **slot-specific**.

### Swapped vs Slot-Specific Settings

| Swapped Settings | Slot-Specific Settings |
|-----------------|---------------------|
| Language stack & version | Custom domain names |
| App settings* | Nonpublic certificates & TLS/SSL |
| Connection strings* | Scale settings |
| Mounted storage accounts* | Always On |
| Public certificates | IP restrictions |
| WebJobs content | WebJobs schedulers |
| Hybrid connections** | Diagnostic settings |
| Service endpoints** | CORS settings |
| Azure CDN** | Virtual network integration |
| Path mapping | Managed identities |

\* Can be configured as slot-specific  
\** Not currently available  

**Tip:** Use deployment slots to safely stage changes, test, and roll out updates with minimal downtime.

## 7. Securing Your Azure App Service App

Azure App Service provides **built-in authentication and authorization** for web apps, APIs, mobile backends, and Azure Functions with minimal or no code.

### Key Features

- Runs **in the same environment** as your app but separately.
- Configured through **app settings**; no SDKs or code changes required.
- Handles:
  - User authentication with specified providers
  - Token validation, storage, and refresh
  - Authenticated session management
  - Injection of identity info into request headers

### Security Options

1. **Allow Anonymous Requests**
   - App receives unauthenticated traffic.
   - Authenticated requests include identity info in HTTP headers.
   - Flexible for apps with public pages or multiple sign-in options.

2. **Allow Only Authenticated Requests**
   - Redirects anonymous requests to `/.auth/login/<provider>`.
   - Returns HTTP 401 for native app requests.
   - No authentication code needed in your app.
   - Restricts access to all app calls, may not suit apps with public pages.

### Logging and Tracing

- View authentication/authorization traces in logs.
- Failed request tracing shows security module activity.
- Look for `EasyAuthModule_32/64` in trace logs.

**Tip:** App Service security simplifies authentication management while letting you focus on business logic.

## 8. Creating Custom Domain Names for Azure App Service

When you create a web app, Azure assigns a default URL like: contoso.azurewebsites.net

For production apps, you can use a **custom domain** like: www.contoso.com

### Benefits of a Custom Domain

- Branded, user-friendly URL
- Builds trust and credibility
- Easier traffic management and security

### Steps to Configure a Custom Domain

1. **Reserve Your Domain**
   - Buy a domain via Azure or a third-party registrar.
   - Azure portal allows direct management of the domain.

2. **Create DNS Records**
   - Use **A record** or **CNAME record** to map the domain to your app:
     - **A record:** Maps domain to IP address (must update if IP changes).
     - **CNAME record:** Maps domain to another domain (auto-updates with Azure IP changes).
   - Note: Some registrars don’t allow CNAME for root/wildcard domains, requiring an A record.

3. **Enable the Custom Domain**
   - Validate and add your domain in the Azure portal.
   - Test your domain before going live.

**Important:** Custom domains require a **paid App Service plan**.

## 9. Back Up and Restore Azure App Service Apps

Azure App Service lets you **create manual or scheduled backups** and restore your app to a previous state.

### Requirements
- **App Service plan:** Standard or Premium tier
- **Azure Storage account and container** in the same subscription

### What Can Be Backed Up
- App configuration settings
- File content
- Connected databases (SQL Database, MySQL, PostgreSQL, MySQL in-app)

### Backup Details
- Each backup consists of:
  - **Zip file:** Contains app and database content
  - **XML file:** Manifest of the Zip contents
- **Full backups** save everything (default)
- **Partial backups** allow excluding specific files/folders
- Maximum backup size: **10 GB**
- Backups are visible in the **Containers page** of the storage account

### Restore Considerations
- **Full restore:** Replaces all content on the site with backup data; missing files are deleted
- **Partial restore:** Restores only selected files/folders; excluded content remains
- You can **browse backup files** without restoring by unzipping the Zip and XML

### Important Notes
- Storage account firewalls can block backup destinations
- Manual and scheduled backups are supported

## 10. Azure Application Insights for App Service

Azure Application Insights (part of Azure Monitor) lets you **monitor and analyze live applications** to detect performance issues and understand user behavior.

![Azure Application Insights](../assets/01-app-insights-16629887.png)

### Key Features
- Works on multiple platforms: **.NET, Node.js, Java EE**
- Supports apps hosted **on-premises, in hybrid environments, or public cloud**
- Integrates with **Azure Pipelines**, Visual Studio, and Visual Studio App Center
- Detects performance anomalies automatically

### What to Monitor
- **Requests:** Rates, response times, failure rates; identify popular pages and peak usage
- **Dependencies:** Monitor external services impacting app performance
- **Exceptions:** Track server/browser errors, drill into stack traces
- **Page views & load performance:** User browser metrics
- **Users & sessions:** Active users and session counts
- **Performance counters:** CPU, memory, network usage from Windows/Linux servers
- **Host diagnostics:** Docker and Azure host metrics
- **Trace logs:** Correlate app events with requests
- **Custom events & metrics:** Track business or app-specific events (e.g., items sold)

### Benefits
- Helps **developers improve app performance and usability**
- Provides **insight into user behavior and system performance**
- Supports **diagnostics, analytics, and alerts**

## 11. Exercise: Implement Web Apps

https://microsoftlearning.github.io/AZ-104-MicrosoftAzureAdministrator/Instructions/Labs/LAB_09a-Implement_Web_Apps.html

## 12. Azure App Service Summary

- **Service:** HTTP-based hosting for web, mobile, and API apps  
- **Platforms:** Windows & Linux, supports multiple languages  
- **Configuration:** Runtime stack, OS, region, App Service plan  
- **Deployment Slots:** Manage dev/test/stage/production with seamless swaps  
- **Custom Domains:** Replace default `*.azurewebsites.net` with branded domain  
- **Backup & Restore:** Full or partial backups for app content and DB (Standard/Premium plans)  
- **Monitoring:** Azure Application Insights detects performance anomalies, tracks usage, and supports custom metrics  
- **Key Takeaway:** Build, secure, scale, and monitor apps efficiently in Azure
