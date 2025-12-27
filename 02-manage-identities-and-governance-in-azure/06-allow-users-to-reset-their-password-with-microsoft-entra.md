## 1. Introduction to Self-Service Password Reset in Microsoft Entra ID

  If you're an IT administrator for a large organization, manual password resets can quickly become overwhelming. Microsoft Entra ID offers **self-service password reset (SSPR)** to help employees securely reset their passwords without IT intervention. This improves productivity and reduces administrative workload.

  ---
  
  ### Module Overview

  In this module, you will learn how to:
  - Evaluate whether to implement SSPR.
  - Implement SSPR to meet organizational requirements.
  - Configure SSPR to customize the user experience.

  ---
  
  ### Prerequisites

  - Basic understanding of Microsoft Entra ID.  
  - A Microsoft Entra tenant with a subscription that includes **Microsoft Entra ID P1**.  
  - An account with at least the **Authentication Policy Administrator** role.

## 2. What is Self-Service Password Reset (SSPR) in Microsoft Entra ID?

  Self-service password reset (SSPR) allows users to reset their own passwords without contacting the help desk, reducing IT workload and improving productivity. This is particularly useful when users forget their passwords, passwords expire, or they are unable to sign in.

  ---
  
  ### Why Use SSPR?

  - Users can reset passwords via a web browser or Windows sign-in screen.
  - Reduces help-desk calls and administrative effort.
  - Minimizes productivity loss due to forgotten or expired passwords.
  - Supports access to Azure, Microsoft 365, and other applications using Microsoft Entra ID.

  ---
  
  ### How SSPR Works

  1. **User Initiates Reset:** Via the password-reset portal or the "Can't access your account" link on a sign-in page.  
  2. **Localization:** Portal displays the correct language based on browser settings.  
  3. **Verification:** User enters username and completes a CAPTCHA.  
  4. **Authentication:** User authenticates using chosen methods (e.g., mobile app, email, phone, security questions).  
  5. **Password Reset:** User sets and confirms a new password.  
  6. **Notification:** User receives a confirmation of the reset.

  > You can customize the experience, e.g., adding your company logo to the portal.

  ---
  
  ### Authentication Methods for SSPR

  Administrators can choose authentication methods for users. At least two methods are recommended:

  | Method | Registration | Authentication |
  |--------|-------------|----------------|
  | Mobile app notification | Install Microsoft Authenticator app and register | Receive and approve/deny a notification |
  | Mobile app code | Same as above | Enter code from app |
  | Email | Provide an external email | Enter code sent to email |
  | Mobile phone | Provide a mobile number | Receive code via SMS or automated call |
  | Office phone | Provide a non-mobile number | Receive automated call and press # |
  | Security questions | Select and answer questions | Answer questions correctly |

  > Trial Microsoft Entra organizations do not support phone call options.

  ---
  
  ### Minimum Number of Authentication Methods

  - Admins can require **1 or 2 methods** for registration.  
  - Users choose which methods to use to reset their password.  
  - For security questions, admins can require a minimum number of questions to register and answer correctly.

  ---
  
  ### Recommendations

  - Enable **two or more authentication methods**.  
  - Prefer **mobile app notification/code** as primary methods.  
  - Enable **email or office phone** for users without mobile devices.  
  - Avoid using SMS as a primary method due to potential fraud.  
  - Use **security questions only in combination** with another method.  

  **Administrator accounts** always require strong two-method authentication, and security questions are not available for these accounts.

  ---
  
  ### Notifications

  - **Notify users on password resets:** Alerts users if a reset occurs.  
  - **Notify all admins when other admins reset passwords:** Ensures visibility for security.

  ---
  
  ### License Requirements

  - Any signed-in user can change their password.  
  - **SSPR** for forgotten/expired passwords requires Microsoft Entra ID P1 or P2 or Microsoft 365 Apps for Business.  
  - **Password writeback** for hybrid environments is supported with P1/P2, allowing changes to sync back to on-premises Active Directory.

  ---
  
  ### SSPR Deployment Options

  - **Microsoft Entra Connect** or **Cloud Sync** can be used for password writeback.  
  - Both options can coexist for different domains.  
  - Cloud Sync offers higher availability for distributed environments.

  ---
  
  ### Check Your Knowledge

  1. **When is a user considered registered for SSPR?**  
     - When they register at least the minimum number of required authentication methods.

  2. **When you enable SSPR for your organization?**  
     - Users can reset their passwords when they cannot sign in.

## 3. Implement Microsoft Entra Self-Service Password Reset (SSPR)

  Implementing SSPR in Microsoft Entra ID allows users to reset their own passwords, reducing IT workload and improving productivity. You can start with a small group and expand to the whole organization once tested.

  ---
  
  ### Prerequisites

  - **Microsoft Entra organization** with at least a P1 or P2 trial license.  
  - **Microsoft Entra account** with Authentication Policy Administrator role.  
  - **Non-administrative user account** for testing SSPR (must have a valid license).  
  - **Security group** to limit rollout for testing.

  ---
  
  ### Scope of SSPR Rollout

  - **None**: No users can use SSPR (default).  
  - **Selected**: Only specified security group members can use SSPR (for testing).  
  - **All**: Every user in the organization can use SSPR.

  ---
  
  ### Configure SSPR

  1. Navigate to **Azure portal → Microsoft Entra ID → Manage → Password reset**.  

  2. **Properties**:  
     - Enable SSPR.  
     - Select either **All users** or **Selected users** (specify security group if using selected).  

  3. **Authentication Methods**:  
     - Require one or two authentication methods.  
     - Choose which methods users can use.  

  4. **Registration**:  
     - Require users to register for SSPR at next sign-in.  
     - Specify frequency for reconfirming authentication info.  

  5. **Notifications**:  
     - Notify users on password resets.  
     - Notify administrators when an admin resets a password.  

  6. **Customization**:  
     - Provide an email or web page URL for user help and support.

## 4. Exercise - Set Up Self-Service Password Reset (SSPR)

  In this exercise, you'll configure and test SSPR for a limited group using email authentication.

  ---
  
  ### Create a Security Group

  1. In Microsoft Entra, go to **Manage → Groups**.  
  2. Select **New Group**.  
  3. Enter the following values:
     - **Group type:** Security  
     - **Group name:** SSPRTesters  
     - **Group description:** Members are testing the rollout of SSPR  
     - **Membership type:** Assigned  
  4. Select **Create**.

  ---
  
  ### Create a User Account

  1. In Microsoft Entra, go to **Manage → Users**.  
  2. Select **+ New user → Create new user**.  
  3. Enter the following values:
     - **User principal name:** balas  
     - **Display name:** Bala Sandhu  
     - **Password:** Copy the autogenerated password to a secure location.  
  4. Assign the user to the **SSPRTesters** group.  
  5. Select **Review + create → Create**.

  ---
  
  ### Enable SSPR for the Group

  1. Navigate to **Microsoft Entra ID → Manage → Password reset → Properties**.  
  2. Set **Self-service password reset enabled** to **Selected**.  
  3. Choose the **SSPRTesters** group.  
  4. Save changes.  
  5. Review **Authentication methods, Registration, and Notifications** pages. Ensure **Email** is selected as an authentication method.  
  6. In **Customization**, enable it and enter a helpdesk email, e.g., `admin@organization-domain-name.onmicrosoft.com`.  
  7. Select **Save**.

  ---
  
  ### Register for SSPR

  1. Open a new browser window in private/incognito mode and go to [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).  
  2. Sign in as `balas@organization-domain-name.onmicrosoft.com` with the initial password.  
  3. If prompted, update the password.  
  4. Go to the **Security info** tab → **+ Add sign-in method** → select **Email**.  
  5. Enter your email details and verify using the code sent.  

  ---
  
  ### Test SSPR

  1. Open a new browser window and go to [https://aka.ms/sspr](https://aka.ms/sspr).  
  2. Enter `balas@organization-domain-name.onmicrosoft.com` as the User ID.  
  3. Complete the CAPTCHA and select **Next**.  
  4. Select **Email** to receive a verification code.  
  5. Enter the code and set a new password.  
  6. Confirm the new password and finish the process.  

## 5. Exercise - Customize Directory Branding

  In this exercise, you'll configure your organization's custom branding for the Azure sign-in page.

  ---
  
  ### Prepare Images for Branding

  Ensure you have the following image files ready:
  - **Page background image:** PNG or JPG, 1920 x 1080 pixels, < 300 KB  
  - **Company logo image:** PNG or JPG, 32 x 32 pixels, < 5 KB

  ---
  
  ### Configure Custom Branding

  1. Sign in to the **Azure portal**.  
  2. Navigate to your **Microsoft Entra organization**. If needed, use **Switch directory** in your Azure profile to find it.  
  3. Under **Manage**, select **Company branding → Customize**.  
  4. Next to **Favicon**, select **Browse** and upload your logo image.  
  5. Next to **Background image**, select **Browse** and upload your page background image.  
  6. Choose a **Page background color** or keep the default.  
  7. Select **Review + Create → Create**.

  ---
  
  ### Test the Branding

  1. Open a new browser window and go to [https://login.microsoft.com](https://login.microsoft.com).  
  2. Sign in with the **Bala Sandhu** account. The custom branding should be visible.  
  3. Select **Forgot my password** to verify that branding also appears on the password reset page.

