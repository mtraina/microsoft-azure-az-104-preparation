# 📘 Understand Microsoft Entra ID (formerly Azure Active Directory)

**Source:** https://learn.microsoft.com/en-us/training/modules/understand-azure-active-directory/  
**Platform:** Microsoft Learn  
**Level:** Beginner  
**Focus:** Cloud identity, access management, and comparison with traditional Active Directory

This module explains what **Microsoft Entra ID** is, how it works, how it differs from on-premises Active Directory, and how it supports cloud and hybrid identity scenarios.

---

## 1️⃣ Introduction

Introduces the module and outlines what you will learn:
- The purpose of Microsoft Entra ID
- How identity works in Azure and cloud services
- Key concepts needed to understand modern identity management

---

## 2️⃣ Examine Microsoft Entra ID

Explains the fundamentals of Microsoft Entra ID:

- A **cloud-based identity and access management (IAM)** service
- Manages users, groups, and applications
- Provides authentication and authorization for:
  - Azure
  - Microsoft 365
  - SaaS and custom applications
- Fully managed by Microsoft (Platform as a Service)

**Key idea:** Entra ID replaces the need to manage identity infrastructure for cloud apps.

---

## 3️⃣ Compare Microsoft Entra ID and Active Directory Domain Services (AD DS)

Highlights the differences between cloud and traditional directory services:

### Active Directory Domain Services (AD DS)
- On-premises directory service
- Uses LDAP, Kerberos, NTLM
- Hierarchical structure (domains, OUs)
- Supports Group Policy Objects (GPOs)
- Requires domain controllers

### Microsoft Entra ID
- Cloud-native, multi-tenant service
- Flat directory structure
- Uses modern authentication protocols:
  - OAuth 2.0
  - OpenID Connect
  - SAML
- Designed for cloud and SaaS apps
- No domain controllers to manage

**Key takeaway:** AD DS and Entra ID solve different problems but can be used together in hybrid environments.

---

## 4️⃣ Examine Microsoft Entra ID as a Directory Service for Cloud Apps

Describes how Entra ID acts as a central identity provider:

- Single directory for multiple cloud applications
- Enables **Single Sign-On (SSO)**
- Centralized user and access management
- Supports:
  - Microsoft apps (Azure, Microsoft 365)
  - Third-party SaaS apps
  - Custom-built applications
- Reduces need for separate app-specific user accounts

**Key benefit:** One identity, many applications.

---

## 5️⃣ Compare Microsoft Entra ID P1 and P2 Plans

Explains licensing tiers and advanced features:

### Entra ID Free
- User and group management
- Basic security features
- SSO for cloud apps

### Entra ID P1
Adds:
- Conditional Access
- Self-service group management
- Advanced security and usage reports
- Hybrid identity features

### Entra ID P2
Adds:
- Identity Protection (risk-based sign-ins)
- Privileged Identity Management (PIM)
- Advanced governance and security controls

**Key takeaway:** Higher tiers provide stronger security and identity governance.

---

## 6️⃣ Examine Microsoft Entra Domain Services

Covers Microsoft Entra Domain Services (formerly Azure AD DS):

- Provides **managed domain services in Azure**
- Supports legacy protocols:
  - LDAP
  - Kerberos
  - NTLM
- Enables domain join for VMs
- No need to manage domain controllers

**Use case:** Legacy applications that require traditional AD features but run in Azure.

## 8️⃣ Summary

Recap of key concepts:

- Microsoft Entra ID is Azure’s cloud identity service
- It differs fundamentally from on-premises AD DS
- It acts as a directory for cloud applications
- Licensing tiers add security and governance features
- Entra Domain Services support legacy workloads

---

## 🧠 Key Concepts at a Glance

- **Microsoft Entra ID:** Cloud-based IAM for Azure and SaaS apps
- **AD DS:** Traditional on-premises directory service
- **SSO:** One identity across many applications
- **P1 vs P2:** Increasing levels of security and identity go
