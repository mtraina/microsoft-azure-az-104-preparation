## 2. Create, Configure, and Manage Users

This unit explains how to work with **user accounts in Microsoft Entra ID (Azure AD)**, which are required for anyone who needs access to Azure resources.

### Key Points

- **What a user is:**  
  Each person who needs access to Azure must have a user account. The account includes information used during sign-in and helps Microsoft Entra ID issue access tokens for authorization. :contentReference[oaicite:0]{index=0}

- **Where you do this:**  
  User management is done through the **Microsoft Entra ID dashboard** in the Azure portal. You can switch between directories if needed. :contentReference[oaicite:1]{index=1}

- **Types of users:**
  - **Cloud identities:** Exist only in Microsoft Entra ID (e.g., administrators you create directly). :contentReference[oaicite:2]{index=2}  
  - **Directory-synchronized identities:** Synchronized from on-premises Active Directory using Microsoft Entra Connect. :contentReference[oaicite:3]{index=3}  
  - **Guest users:** External identities (e.g., users from other companies or personal Microsoft accounts invited for collaboration). :contentReference[oaicite:4]{index=4}

- **Viewing users:**  
  In the Azure portal, under **Identity > Users > All Users**, you can see a list of user accounts and their types. :contentReference[oaicite:5]{index=5}

### Why It Matters

User accounts are fundamental for authenticating individuals and controlling what resources they can access within Azure. Proper management ensures the right users have access to the right resources at the right time.

## 3. Exercise – Assign Licenses to Users

This exercise teaches you how to **assign licenses to users and groups** in **Microsoft Entra ID (Azure Active Directory)** by using the **Microsoft Entra admin center** and **Microsoft 365 admin center**.

### 🧠 What You Do in the Exercise

1. **Create a New User**
   - Go to https://entra.microsoft.com/
   - Use the Microsoft Entra admin center to create a user account (e.g., “Chris Green”).  
   - Verify the new user appears in **All Users**.

2. **Create a Security Group**
   - In Microsoft Entra admin center, create a **security group** (e.g., “Marketing”).  
   - Add the new user to the group.

3. **Assign a License to the Group**
   - Open the **Microsoft 365 admin center** (https://admin.microsoft.com).  
   - Go to **Billing > Licenses**.  
   - Choose a license, switch to the **Groups** view, and **assign** it to your “Marketing” group.  
   - Members such as your user will then inherit the license.

### 🧠 Why This Matters

- Users need appropriate **licenses** to access Microsoft 365 services (e.g., Exchange, Teams).  
- Assigning licenses to a **group** simplifies management when multiple users require the same subscriptions.  
- You must have sufficient permissions (**User Administrator** or similar) and appropriate Azure/Microsoft 365 admin access to perform these tasks.

Note: adding a 365 livense is not possible with the same private email used so far as a non-private email is needed for logging it.

---

## 4. Exercise: Restore or Remove Deleted Users in Microsoft Entra ID

### 1. **Remove a User:**
- Navigate to the **Microsoft Entra admin center**.
- Under **Identity**, select **Users**.
- Choose a user to delete (e.g., Chris Green).
- Click on **Delete user** from the menu and confirm the deletion.

### 2. **Restore a Deleted User:**
- Go to the **Deleted users** section in the left navigation.
- Find the user you want to restore (users deleted within the last 30 days).
- Select the user and click **Restore user**.
- Confirm by clicking **OK**.
- Go to **All users** to verify that the user has been restored.

### **Important Notes:**
- Users deleted for over 30 days are permanently removed.
- You need basic **Microsoft Entra** admin rights to perform these actions.

---

## 5. Create, Configure, and Manage Groups in Microsoft Entra ID

### **Purpose of Groups:**
Groups in **Microsoft Entra ID** help organize users and manage permissions more easily. Instead of assigning permissions to users one-by-one, groups allow you to assign permissions to all members at once. This simplifies managing access to resources based on rules like department or job title.

### **Types of Groups:**
1. **Security Groups**: 
   - Used to manage member and computer access to shared resources.
   - Commonly used for applying security policies and permissions.

2. **Microsoft 365 Groups**: 
   - Provides collaboration tools like shared mailboxes, calendars, and files.
   - Allows external users to access resources and is available for both users and administrators.

### **Viewing and Managing Groups:**
- You can view and manage all groups under the **Groups** section in the **Microsoft Entra admin center**. 
- A new deployment will not have any groups defined initially.

### **Membership Types:**
1. **Assigned**: 
   - Members are manually added to the group.
   
2. **Dynamic**: 
   - Membership is automatically determined by rules based on user attributes (e.g., department, job title).
   - Dynamic groups are still categorized as security or Microsoft 365 groups, but their membership is controlled by rules.

### **Dynamic Groups:**
- Dynamic groups automatically adjust their membership based on specific attributes defined in a rule.
- If a user’s attributes match the rule, they are added to the group.
- It’s important to manage these groups carefully to avoid accidental membership changes.

---

## 6. Exercise - Add Groups in Microsoft Entra ID

### **Exercise Requirements:**
- This lab assumes you have a basic **Microsoft Entra tenant** with at least **User Administrator** rights.
- You can get a free trial subscription at **Try Azure for Free**.

### **Steps to Create a Microsoft 365 Group:**
1. Go to the **Microsoft Entra admin center** and navigate to the **Identity** page.
2. In the left navigation, select **Groups**.
3. On the **Groups** blade, click **New group**.

4. **Fill in the following details:**
   - **Group type**: Microsoft 365
   - **Group name**: Northwest Sales
   - **Membership type**: Assigned
   - **Owners**: Assign your own administrator account as the group owner
   - **Members**: Assign a member of this group

5. **Verify the new group:**
   - After creating the group, check the **All groups** list to ensure **Northwest Sales** is listed.
   - You may need to refresh the **All groups** list a few times for the new group to appear.

---

## 7. Configure and Manage Device Registration

### **Overview:**
With the increase of devices and Bring Your Own Device (BYOD) policies, IT professionals face the challenge of enabling user productivity while protecting organizational assets. **Microsoft Entra ID** helps achieve both by managing device identities and offering tools like **Microsoft Intune** to ensure security standards are met. This enables **Single Sign-On (SSO)** across devices, apps, and services.

### **Microsoft Entra Registered Devices:**
Microsoft Entra Registered devices allow users to access organizational resources on personal devices, such as BYOD or mobile devices.

- **Description**: Devices are registered with Microsoft Entra ID, but users sign in using local credentials (e.g., Microsoft account) rather than an organizational account.
- **Key Features**:
  - **Target Audience**: BYOD and mobile devices
  - **Device Ownership**: User or Organization
  - **Operating Systems Supported**: Windows 10/11, iOS, Android, macOS
  - **Sign-in Options**: Local credentials, Windows Hello, PIN, biometrics
  - **Management Tools**: Mobile Device Management (MDM) like Microsoft Intune
  - **Capabilities**: SSO, Conditional Access

Microsoft Entra Registered devices enable access to organization resources based on Conditional Access policies. These devices can be managed and secured through **MDM** tools like Microsoft Intune.

**Example Scenario**: A user adds their work account to their home PC, registers it with Microsoft Entra ID, and is granted access to resources under Intune compliance policies.

### **Microsoft Entra Joined Devices:**
Microsoft Entra Joined devices are designed for cloud-first or hybrid organizations. These devices are directly joined to Microsoft Entra ID and require organizational accounts for sign-in.

- **Description**: Devices are joined to Microsoft Entra ID and require organizational credentials for access.
- **Key Features**:
  - **Target Audience**: Cloud-only and hybrid organizations
  - **Device Ownership**: Organization
  - **Operating Systems Supported**: Windows 10/11 (excluding Home editions)
  - **Management Tools**: Mobile Device Management (MDM) like Microsoft Intune
  - **Capabilities**: SSO to both cloud and on-premises resources, Conditional Access, Self-service Password Reset

Microsoft Entra Joined devices authenticate to both cloud and on-premises resources, and are managed via MDM or Configuration Manager.

**Example Scenario**: An organization with no on-premises infrastructure can use Microsoft Entra Joined devices to manage all their devices using Intune.

### **Hybrid Microsoft Entra Joined Devices:**
Hybrid Microsoft Entra Joined devices are for organizations with an on-premises Active Directory (AD) and Microsoft Entra ID. These devices are joined to both on-premises AD and Microsoft Entra ID.

- **Description**: Devices are joined to both on-premises AD and Microsoft Entra ID.
- **Key Features**:
  - **Target Audience**: Hybrid organizations with existing AD infrastructure
  - **Device Ownership**: Organization
  - **Operating Systems Supported**: Windows 10/11, Windows Server 2008/R2 and newer
  - **Sign-in Options**: Password or Windows Hello for Business
  - **Management Tools**: Group Policy, Configuration Manager, Intune
  - **Capabilities**: SSO to cloud and on-premises resources, Conditional Access, Self-service Password Reset

**Example Scenario**: An organization with a mix of on-premises and cloud resources can use hybrid joined devices for a seamless experience across both environments.

### **Device Writeback:**
In a hybrid setup, device writeback helps track devices registered in Microsoft Entra ID within on-premises Active Directory. This allows conditional access based on device status.

- **Scenario**: For cloud-based apps, conditional access policies can verify if a device is joined to Microsoft Entra ID. For on-premises apps, **device writeback** enables this verification by syncing device objects to the on-premises AD, making it possible to apply conditional access.


---

## 7. Manage Licenses

### **Overview:**
Microsoft's paid cloud services (like **Microsoft 365**, **Enterprise Mobility + Security**, **Dynamics 365**) require licenses for each user accessing them. Administrators traditionally managed these licenses at the individual user level, which could be complex and time-consuming. Now, **Microsoft Entra ID** introduces **group-based licensing**, simplifying license assignment by associating licenses with groups instead of individual users.

### **Group-Based Licensing:**
- With **group-based licensing**, administrators assign product licenses to a **security group** in Microsoft Entra ID. All members of that group automatically inherit the assigned licenses.
- When a user joins or leaves the group, their licenses are automatically updated based on group membership. This eliminates the need for complex PowerShell scripts to manage licenses.
- This method simplifies license management, especially for large organizations undergoing frequent changes (e.g., user departures, department shifts).

### **License Requirements:**
To use **group-based licensing**, you must have:
- **Microsoft Entra ID Premium P1** or higher (Paid or trial).
- **Office 365 Enterprise E3** or higher (Paid or trial).

Additionally, you must ensure that you have enough licenses for all unique members of the group. For example, if a group has 1,000 unique members, you must have at least 1,000 licenses.

### **Key Features of Group-Based Licensing:**
1. **Assignable to Any Security Group**: 
   - Groups can be synced from on-premises via **Microsoft Entra Connect**, or created directly in Microsoft Entra ID (cloud-only groups).
   - Dynamic groups (created automatically based on rules) can also be used.
   
2. **Service Plan Control**:
   - Administrators can disable specific service plans within a product, such as temporarily disabling **Yammer** while enabling **Microsoft 365** for a department.
   
3. **Supported Services**:
   - Supports all Microsoft cloud services requiring user-level licensing, including **Microsoft 365**, **Enterprise Mobility + Security**, and **Dynamics 365**.
   
4. **License Modifications**:
   - Modifications from group membership changes are typically processed within minutes.
   
5. **Multiple License Sources**:
   - A user can be part of multiple groups with license policies, and they can have licenses directly assigned outside of groups. 
   - If a license is assigned from multiple sources, it is consumed only once.

6. **License Assignment Conflicts**:
   - Sometimes licenses can't be assigned due to insufficient licenses or conflicting services. Administrators are notified when issues occur, allowing them to take corrective action.

7. **Usage Location**:
   - Some Microsoft services aren't available in all locations. Administrators should set the **usage location** in the **User Profile** before assigning licenses. If no usage location is set, users inherit the directory's location by default, which may cause issues with license assignment.

---

## 8. Exercise - Change Group License Assignments

### Overview

This exercise walks you through how to change license assignments for a group in Microsoft Entra ID, troubleshoot common issues, and resolve license assignment errors. You will also learn how to ensure seamless transitions when using group-based licensing.

### Prerequisites

- A basic Microsoft Entra tenant with at least **User Administrator** rights.
- Access to **Microsoft Entra Admin Center** and **Microsoft 365 Admin Center**.
- A **trial or paid subscription** for Microsoft Entra ID Premium P1 or Office 365 Enterprise E3 or greater.

### Steps to Change Group License Assignment

1. **Access Microsoft Entra Admin Center**
   - Open the [Microsoft Entra Admin Center](https://entra.microsoft.com).
   - In the left navigation pane, go to **Groups**.

2. **Select Group**
   - Under **All groups**, select the group you want to modify.

3. **Manage Licenses**
   - In the left navigation pane, under **Manage**, select **Licenses**.

4. **Review Current Assignments**
   - Check the current license assignments and note any existing configurations.

5. **Assign New License**
   - Click **+ Assignments** and open the [Microsoft 365 Admin Center](https://admin.microsoft.com).
   - Under **Billing**, select **Licenses**, and choose an available license.
   - Go to **Groups**, select **+ Assign licenses**, and choose the group you selected earlier.
   - Click **Assign** at the bottom of the page.

6. **Verify Changes**
   - Review the changes in both the **Microsoft Entra admin center** and the **Microsoft 365 admin center** to ensure the licenses have been properly applied.

### Common License Assignment Problems and Solutions

1. **Not Enough Licenses**
   - **Problem**: There aren’t enough licenses available for the group.
   - **Solution**: Either purchase more licenses or free up unused ones.
   - **Check Available Licenses**: Go to **Microsoft Entra - Identity - Billing - Licenses - All Products**.

2. **Conflicting Service Plans**
   - **Problem**: A conflict occurs between service plans that cannot be assigned together.
   - **Example**: Assigning both Office 365 E3 and E1 with overlapping plans (e.g., SharePoint Online Plan 2 conflicts with Plan 1).
   - **Solution**: Disable the conflicting plans or adjust license assignments to resolve the issue.

3. **Dependent Services**
   - **Problem**: One service plan requires another to be enabled for it to function properly.
   - **Solution**: Ensure the necessary prerequisite service is enabled before removing licenses from users.

4. **Usage Location Not Allowed**
   - **Problem**: License assignment fails if the user’s usage location is not supported.
   - **Solution**: Set the correct **usage location** for users in the **User Profile**.

5. **Duplicate Proxy Addresses**
   - **Problem**: Duplicate proxy addresses in Exchange Online may cause assignment failures.
   - **Solution**: Resolve any proxy address conflicts before applying the license.

6. **License Assignment Errors in Logs**
   - **Problem**: Errors such as **LicenseAssignmentAttributeConcurrencyException** are logged.
   - **Solution**: Reprocess the licenses or resolve errors manually in the Microsoft Entra Admin Center.

### Additional Considerations

- **Multiple Licenses in a Group**: You can assign multiple licenses to a group, but if there’s an issue with one license, none of the licenses will be applied to group members.
- **Deleting Licensed Groups**: Remove all licenses from a group before deleting it. License removals may fail if users have dependent licenses.
- **Managing Add-On Products**: Some products, like Microsoft Workplace Analytics, require both the add-on and prerequisite service to be assigned to the group. Ensure both are included before assigning.

### Reprocessing Licenses

- **Group Reprocessing**: If necessary, manually trigger the reprocessing of licenses by clicking the **Reprocess** button in the **Licenses** tab for the group.
- **User Reprocessing**: Similarly, trigger user reprocessing if there are license assignment errors.

### Migration to Group-Based Licensing

When migrating from individual license assignments (e.g., via PowerShell), follow this process:
1. Create a new group or use an existing group for license assignment.
2. Assign the required licenses to the group to match the current configuration.
3. Confirm that licenses have been successfully applied to users.
4. Gradually remove direct license assignments, monitoring for issues.

### Example Migration Process

- **Scenario**: An organization has 1,000 users requiring Office 365 Enterprise E3 licenses and is using PowerShell scripts to manage license assignments.
- **Migration Steps**:
  1. Assign the **Office 365 E3** license to the **All users** group in Microsoft Entra ID.
  2. Confirm that licenses are applied to all users by checking the processing status in the **Licenses** tab of the group.
  3. Verify that users have both **direct** and **group** licenses assigned.
  4. Gradually remove direct assignments, monitoring the outcomes.
  
### Change License Assignments for a User or Group

1. Before updating license assignments, ensure users have the **current license plan** assigned via the group and not directly.
2. Verify there are enough **available licenses** for the new plan.
3. Ensure no **conflicting service licenses** are assigned to the user.
4. If managing on-premises groups, allow time for synchronization to reflect changes in Microsoft Entra ID.
5. If using **dynamic group memberships**, license assignment will be based on user attributes.

---

## 10. Exercise - Change User License Assignments

### Overview

In this exercise, you will create a new user in **Microsoft Entra ID** and update the user's license assignments. This task is essential for managing user access to Microsoft services and resources in an organization.

### Prerequisites

- A basic **Microsoft Entra tenant** with at least **User Administrator** rights.
- A **trial or paid subscription** for Microsoft Entra ID.

### Steps to Complete the Exercise

#### 1. **Create a New User in Microsoft Entra ID**
   1. Go to the **Microsoft Entra admin center** and navigate to the **Identity - Users** page.
   2. In the left navigation pane, select **Users**.
   3. In the **Users** blade, click on **New user**.
   4. Fill in the user details with the following information:
      - **User name**: DominiqueK
      - **Name**: Dominique Koch
      - **First name**: Dominique
      - **Last name**: Koch
      - **Password**: Create a unique password for the user
      - **Usage location**: Select your preferred usage location
   5. Once the details are complete, verify that the new user "Dominique Koch" appears in the list of all users in Microsoft Entra ID.

#### 2. **Update User License Assignments**
   1. Go back to the **Microsoft Entra admin center**.
   2. In the left navigation pane, select **Users** under **Identity**.
   3. Select **Dominique Koch** from the user list.
   4. In the left navigation pane, select **Licenses**.
   5. In the **Update license assignments** blade, check the boxes for one or more licenses you wish to assign to the user.
   6. Once the desired licenses are selected, click **Save** to apply the changes.

---

## 11. Create Custom Security Attributes

### Overview

Custom security attributes in **Microsoft Entra ID** allow you to define business-specific attributes (key-value pairs) that can be assigned to various Microsoft Entra objects. These attributes help store information, categorize objects, and enforce granular access control for Azure resources.

### Why Use Custom Security Attributes?

- **Extend user profiles**: Add custom attributes like "Employee Hire Date" or "Hourly Salary" for employees.
- **Fine-grained access control**: Ensure only authorized users (e.g., administrators) can view sensitive data like salary information.
- **Categorize resources**: Create filterable inventories for auditing purposes (e.g., categorizing applications).
- **Control resource access**: Grant users access to Azure resources like **Azure Storage blobs** based on custom attributes.

### What Can You Do With Custom Security Attributes?

- Define business-specific information for your tenant.
- Add custom security attributes to **users**, **applications**, **Microsoft Entra resources**, or **Azure resources**.
- Manage Microsoft Entra objects using queries and filters based on these attributes.
- Govern access by using attributes to determine who can access specific resources.

### Features of Custom Security Attributes

- Available across the entire tenant.
- Support for different data types: **Boolean**, **Integer**, **String**.
- Attributes can have **single values** or **multiple values**.
- Can contain **user-defined free-form values** or **predefined values**.
- Can be assigned to **directory synced users** from on-premises Active Directory.

---

## 12. Explore Automatic User Creation

### Overview

**Automatic user creation** utilizes **SCIM (System for Cross-Domain Identity Management)** to automate the provisioning and de-provisioning of users and groups between various systems. This process ensures that identity systems are always up to date by synchronizing user attributes and profiles.

### Components of SCIM

1. **HCM System**: Applications and technologies supporting HR processes throughout the employee lifecycle.
2. **Microsoft Entra Provisioning Service**: Uses the SCIM 2.0 protocol to automate user provisioning. It connects to the SCIM endpoint for applications and uses REST APIs to manage user and group life cycles.
3. **Microsoft Entra ID**: User repository that manages the lifecycle of identities and their entitlements.
4. **Target System**: The system or application with a SCIM endpoint that works with Microsoft Entra to enable automatic user and group provisioning.

### Why Use SCIM?

**SCIM** is an open standard protocol for automating the exchange of user identity information between identity domains and IT systems. Its primary benefits include:

- **Automation of user account creation**: Users added to the **Human Capital Management (HCM) system** are automatically provisioned in **Microsoft Entra ID** or **Windows Server Active Directory**.
- **Synchronization of user attributes**: Ensures that user profiles remain consistent across systems.
- **Automatic deprovisioning**: If a user is removed from the HR system, their access in Entra ID is also automatically revoked, reducing security risks.

### Key Benefits

- **Up-to-date identity systems**: By automating the process, you minimize the risk of breaches due to outdated user information.
- **Improved security**: Automatic deprovisioning helps ensure that only active users have access to organizational resources.

---