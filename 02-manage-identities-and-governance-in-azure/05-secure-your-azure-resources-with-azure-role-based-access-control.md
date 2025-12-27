## 1. Secure Your Azure Resources with Azure RBAC – Introduction

Securing Azure resources while enabling appropriate access is essential for cloud-based organizations. **Azure role-based access control (Azure RBAC)** is Azure’s authorization system that helps manage **who can access resources, what actions they can perform, and where that access applies**.

### Purpose of Azure RBAC
- Protects resources such as **virtual machines, storage, networks, and web apps**
- Balances **security** with **operational access** for employees and partners
- Provides fine-grained access control across Azure

### Example Scenario
- An engineering firm moves workloads to Azure for better collaboration
- IT is responsible for securing assets while allowing necessary access
- Azure RBAC is used to manage and control permissions effectively

### Learning Objectives
By the end of this module, you will be able to:
- Verify access to Azure resources
- Grant access using Azure RBAC
- Review activity logs for RBAC changes

## 2. What Is Azure RBAC?

**Azure role-based access control (RBAC)** is Azure’s authorization system that provides **fine-grained access management** to Azure resources by defining **who** can do **what** at **which scope**.

### Purpose of Azure RBAC
- Ensures users lose access when they leave an organization
- Balances team autonomy with centralized governance
- Works with **Microsoft Entra ID** for identity, SSO, and access control

### How Azure RBAC Works
Access is granted through **role assignments**, which combine:
- **Security principal (who)**: user, group, or application
- **Role definition (what)**: set of permissions (read, write, delete)
- **Scope (where)**: management group, subscription, resource group, or resource

### Built-in Roles (Examples)
- **Owner**: Full access, including assigning roles
- **Contributor**: Manage resources, but not access
- **Reader**: View resources only
- **User Access Administrator**: Manage user access

### Scope and Inheritance
- Roles assigned at a higher scope automatically apply to child scopes
- Enables precise access (for example, contributor to one resource group only)

### Key Characteristics
- Uses an **allow model**: permissions accumulate from multiple role assignments
- Supports **NotActions** to explicitly exclude permissions
- Managed through the **Access control (IAM)** pane in the Azure portal

## Exercise: Grant Access Using Azure RBAC

This exercise shows how to **grant least-privilege access** to a user using Azure RBAC.

---

## Scenario

A user named **Alain** needs permission to create and manage virtual machines for a project.  
**Best practice:** Assign only the necessary role at the smallest scope possible (resource group).

---

## Exercise: List Access Using Azure RBAC

This exercise demonstrates how to **view role assignments** for yourself and for a resource group in the Azure portal.

---

## Scenario

You have access to a resource group for the marketing team at First Up Consultants and want to see **who has access and what roles are assigned**.

---

## 4. List Your Role Assignments

1. **Sign in** to the Azure portal.  
2. Click the **Profile menu** (top-right corner) and select the ellipsis `(...)`.  
3. Choose **My permissions** to open the pane showing:
   - Roles assigned to you
   - Scope of each role  

**Outcome:** You can see all your permissions in Azure.

---

## List Role Assignments for a Resource Group

1. In the Azure portal, search for **Resource groups**.  
2. Select the resource group you want to check.  
3. On the left menu, click **Access control (IAM)**.  
4. Go to the **Role assignments** tab to view:
   - Users, groups, and applications with access
   - Scope of each role (This resource or Inherited from parent)

---

## List All Roles

1. In the **Access control (IAM)** pane, select the **Roles** tab.  
2. View **built-in and custom roles**.  
3. Click **View** under the Details column for a role, then select the **Assignments** tab to see **who is assigned** that role.

**Key takeaway:** Azure RBAC lets you **inspect access** at both personal and resource group levels to ensure proper permissions.


## 5. Steps to Grant Access

1. **Sign in** to the Azure portal as a user with **Owner** or **User Access Administrator** permissions.  
2. Search for **Resource groups** and select the desired resource group.  
3. Open **Access control (IAM)** from the left-hand menu.  
4. Click **Add > Add role assignment**.  
5. On the **Role** tab, search for and select **Virtual Machine Contributor**.  
6. On the **Members** tab, click **Select members** and choose the user (**Alain**).  
7. Click **Review + assign** to finalize the role assignment.

**Outcome:** Alain can now create and manage **virtual machines only within this resource group**.

---

## Steps to Remove Access

1. In **Access control (IAM)**, go to the **Role assignments** tab.  
2. Locate the user and check the box for the assigned role.  
3. Click **Delete** and confirm by selecting **Yes**.

**Key takeaway:** Azure RBAC allows you to **grant and revoke access** at a precise scope, following the principle of least privilege.

## Exercise: View Activity Logs for Azure RBAC Changes

This exercise shows how to **track Azure RBAC changes** using the Azure Activity Log for auditing and troubleshooting.

---

## Scenario

First Up Consultants audits Azure RBAC changes quarterly. You need to **generate a report of role assignment and custom role changes** for the last month.

---

## 6. View Activity Logs

1. In the Azure portal, go to **All services** and search for **Activity log**.  
2. Select **Activity log** to open it.  
3. Set the **Timespan** filter to **Last month**.  
4. Add an **Operation** filter and type `role`.  
5. Select the following Azure RBAC operations:  
   - `Create role assignment (roleAssignments)`  
   - `Delete role assignment (roleAssignments)`  
   - `Create or update custom role definition (roleDefinitions)`  
   - `Delete custom role definition (roleDefinitions)`  

**Outcome:** A list of all role assignment and role definition operations for the last month appears.  

---

## Generate a Report

- Use the **Download CSV** button at the top to export the activity log.  
- Click on any operation to view **detailed activity log information** for auditing.

**Key takeaway:** Azure Activity Log provides a way to **track and report RBAC changes** for compliance and troubleshooting purposes.