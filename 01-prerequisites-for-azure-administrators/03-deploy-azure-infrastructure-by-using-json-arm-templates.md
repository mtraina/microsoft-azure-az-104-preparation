# 🧱 Deploy Azure Infrastructure by Using JSON ARM Templates — Module Summary

**URL:** https://learn.microsoft.com/en-gb/training/modules/create-azure-resource-manager-template-vs-code/  
## 📘 Overview

Azure Resource Manager (ARM) templates are **JSON files** that define Azure infrastructure declaratively. This module focuses on:
- Understanding ARM template structure
- Writing templates in Visual Studio Code
- Deploying templates using Azure CLI or Azure PowerShell
- Making templates reusable with parameters and outputs

---

## 🎯 Learning Objectives

By completing this module, you will be able to:

- Create an ARM template using Visual Studio Code  
- Define Azure resources in JSON  
- Add **parameters** to customize deployments  
- Add **outputs** to return deployment values  

---

## 📑 Module Structure & Unit Summaries

The module is divided into **7 units**, combining theory with hands-on exercises.

---

### 1. Introduction

Introduces the purpose of the module and explains why ARM templates are useful for:
- Infrastructure as Code (IaC)
- Consistent, repeatable deployments
- Automation of Azure resource provisioning

---

### 2. Explore Azure Resource Manager Template Structure

This unit explains the **core components of an ARM template**, including:

- `$schema`  
  Defines the JSON schema for template validation

- `contentVersion`  
  Specifies the version of the template

- `apiProfile`  
  Defines a collection of API versions for resource type

- `parameters`  
  Inputs provided at deployment time (e.g., resource name, location)

- `variables`  
  Values that are used to simplify template language expressions.

- `functions`  
   U    ser-defined functions that are available within the template - [reference](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/user-defined-functions)

- `resources`  
  The Azure resources to be deployed

- `output`  
  Values returned after deployment (e.g., resource IDs)

This structure allows you to describe **what** to deploy, not **how** to deploy it.


WIP

---

### 3. Exercise – Create and Deploy an ARM Template

Hands-on lab where you:

- Create a basic ARM template in Visual Studio Code
- Define at least one Azure resource
- Deploy the template to Azure using:
  - Azure CLI **or**
  - Azure PowerShell
- View deployment results in the Azure portal

---

### 4. Add Flexibility Using Parameters and Outputs

This unit focuses on making templates reusable and adaptable:

- **Parameters**
  - Allow customization at deployment time
  - Support multiple environments (dev/test/prod)

- **Outputs**
  - Return useful values after deployment
  - Can be consumed by other templates or scripts

Parameterized templates reduce duplication and improve maintainability.

---

### 5. Exercise – Add Parameters and Outputs

A guided lab where you:

- Add parameters to an existing ARM template
- Define outputs to expose deployment results
- Redeploy the template and validate the changes

This reinforces template flexibility and reuse concepts.

---

### 6. Module Assessment

A short knowledge check covering:

- ARM template structure
- Resource declaration
- Parameters and outputs
- Deployment concepts

---

### 7. Summary

Recaps key takeaways:

- ARM templates enable Infrastructure as Code in Azure
- Templates are declarative and predictable
- Parameters and outputs improve reusability
- Visual Studio Code is an effective authoring tool

---

## 🧠 Key Concepts Covered

### What Is an ARM Template?
- JSON-based Infrastructure as Code definition
- Declarative (describe desired state)
- Deployable via Azure CLI, PowerShell, REST API, or Azure portal

### Repeatable Deployments
- Same template can be deployed multiple times
- Produces consistent results across environments

### Parameters and Outputs
- Parameters inject values at runtime
- Outputs return values after deployment
- Together, they support modular and reusable designs

---

## 🛠 Tools Used in This Module

- **Visual Studio Code** — template authoring
- **Azure CLI** or **Azure PowerShell** — deployment tools
- **Azure Resource Manager** — deployment engine
- Azure subscription and resource group

---

## 📌 Notes & Best Practices

- ARM templates use **declarative syntax**
- You define *what* resources you want, not *how* to create them
- Microsoft recommends **Bicep** as a higher-level alternative that compiles to ARM JSON, though ARM remains foundational

---

## ✅ Outcome

After completing this module, you will be able to confidently:
- Write ARM templates
- Deploy Azure resources using Infrastructure as Code
- Customize deployments with parameters
- Extract useful deployment information with outputs
