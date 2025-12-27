## 1. Azure Policy Initiatives – Introduction

Azure Policy helps organizations **define, assign, and manage governance rules** to ensure Azure resources remain compliant with IT standards and service-level agreements. Policies are written in **JSON** and enforce rules and effects across resources at scale.

### Azure Policy Initiatives
- **Initiatives** group multiple policy definitions toward a single goal.
- Enable **centralized governance** and simplified compliance tracking.
- Commonly used to meet **regulatory and organizational requirements**.

### Industry and Compliance Use
- Widely used in regulated sectors (government, finance, public sector).
- Supports **sovereignty-focused and regulatory compliance frameworks**.
- Helps reduce audit time and enforce consistent cloud guardrails.

### What You Learn in This Module
- Assign policies to control future resource creation
- Create and manage initiatives for compliance tracking
- Resolve noncompliant or denied resources
- Implement policies consistently across an organization

> Organizations remain responsible for meeting all legal and regulatory obligations.

## 2. Cloud Adoption Framework for Azure (Overview)

The **Microsoft Cloud Adoption Framework for Azure** provides end-to-end guidance to help organizations plan, govern, and manage cloud adoption effectively. It supports cloud architects, IT teams, and business leaders with best practices, tools, and documentation.

### Cloud Governance
Cloud governance ensures cloud usage aligns with business goals while minimizing risks such as security, compliance, cost, and operational issues. Governance is continuous and adapts over time.

### Five Steps of Cloud Governance
1. **Build a governance team** – Define ownership and accountability.
2. **Assess cloud risks** – Identify compliance, security, cost, data, and AI risks.
3. **Document governance policies** – Establish clear rules and boundaries.
4. **Enforce policies** – Use automation and oversight to ensure compliance.
5. **Monitor governance** – Continuously review and improve policies.

### Core Governance Disciplines
- **Cost management** – Control and optimize cloud spending.
- **Security baseline** – Apply consistent security requirements.
- **Resource consistency** – Standardize configuration and management.
- **Identity baseline** – Enforce identity and access controls.
- **Deployment acceleration** – Speed deployments through standardization.

### Cloud Governance with Azure Policy
**Azure Policy** is Azure’s main governance tool. It:
- Enforces organizational standards at scale  
- Monitors and reports compliance centrally  
- Prevents or remediates noncompliant resources  
- Supports automation and DevOps integration  

The goal is to balance **control and stability** with **speed and productivity**, ensuring governance without slowing innovation.

## 3. Azure Policy Design Principles (Summary)

Azure governance uses **Azure Policy** to control, secure, and manage cloud resources by enforcing rules and tracking compliance.

### Governance Hierarchy
Azure governance is applied through a hierarchy:
- **Management groups** – Organize multiple subscriptions for large-scale governance.
- **Subscriptions** – Units of billing, access control, and limits.
- **Resource groups** – Logical containers for related resources.
- **Resources** – Individual services like VMs, storage, or databases.

Policies assigned at higher levels are **inherited** by lower levels.

### Azure Resource Manager (ARM)
Azure Resource Manager is the **management layer** for Azure.
- Handles creation, update, and deletion of resources.
- Provides RBAC, templates, tagging, auditing, and monitoring.
- Azure Policy is integrated directly into ARM.

### Control Plane vs Data Plane
- **Control plane**: Manages resources (create, update, delete).  
  - Azure Policy evaluates compliance here.
- **Data plane**: Interacts with data inside resources (files, databases, secrets).  
  - Managed by service-specific permissions.
  - Azure Policy supports limited data-plane scenarios (e.g., Kubernetes, Key Vault).

### Policy Evaluation Scenarios
- **Greenfield (policy-first)**: Policies are evaluated during resource creation or updates.
- **Brownfield (resource-first)**: Existing resources are scanned for compliance after policy assignment.

### Key Takeaway
Azure Policy works within Azure Resource Manager to enforce governance consistently across the Azure hierarchy, balancing **control, security, and compliance** without blocking productivity.

## 4. Azure Policy Resources (Short Summary)

Azure Policy enforces standards and tracks compliance across Azure using several core policy resources.

### Policy Definitions
- Define **rules and effects** to evaluate resource compliance.
- Saved at **management group** or **subscription** level.
- Scope determines which resources are evaluated.

### Initiatives (Policy Sets)
- Group multiple policy definitions into one unit.
- Simplify large-scale governance and regulatory compliance.
- Can be **built-in** (Microsoft-provided) or **custom**.

### Assignments
- Apply a policy or initiative to a specific **scope**.
- Support parameters, exclusions, overrides, and enforcement modes.
- Determine which resources count toward compliance.

### Exemptions
- Temporarily or permanently exclude resources from evaluation.
- Types: **Mitigated** (handled another way) and **Waiver** (temporarily accepted).

### Attestations
- Manually confirm compliance for resources under manual policies.

### Remediations
- Automatically or manually fix noncompliant resources.
- Used with **modify** or **deployIfNotExists** policies.

**Key idea:** Azure Policy resources work together to define rules, assign scope, handle exceptions, and maintain compliance at scale.

## 5. Azure Policy Definitions (Short Summary)

Azure Policy definitions define **compliance rules** and **actions** for Azure resources using JSON.

### Core Structure
- **Condition (if)**: Checks resource properties using fields, values, counts, aliases, and logical operators.
- **Effect (then)**: Determines what happens when conditions are met.

### Key Definition Elements
- **displayName & description**: Identify and explain the policy.
- **policyType**: Built-in, Custom, or Static.
- **mode**: Determines evaluation scope (Resource Manager or Resource Provider).
- **parameters**: Make policies reusable with configurable values.
- **policyRule**: Contains the if/then logic.

### Conditions & Logic
- Logical operators: `allOf`, `anyOf`, `not` (can be nested).
- Conditions evaluate **fields**, **values**, or **counts**.
- Supports comparisons like equals, notIn, containsKey, greater, exists.

### Policy Functions
- Add logic such as date calculations, IP range checks, or referencing current values.
- Examples: `addDays()`, `ipRangeContains()`, `current()`.

### Effect Types
- **Preventive**: `deny`, `denyAction`
- **Corrective**: `modify`, `deployIfNotExists`
- **Monitoring**: `audit`, `auditIfNotExists`
- **Other**: `disabled`, `manual`, `append`

**Key idea:** Azure Policy definitions combine conditions and effects to enforce, audit, or remediate resource compliance at scale.

## 6. Evaluation of Resources Through Azure Policy

Azure Policy provides visibility and control by evaluating resources for compliance across subscriptions and management groups.

### Evaluation Triggers
Policy evaluations occur when:
- Policies or initiatives are assigned or updated
- Resources are created or modified
- Subscriptions move within management groups
- Exemptions change
- Scheduled or on-demand compliance scans run

### Evaluation Timing
- **Automatic scans** run every 24 hours.
- **Manual scans** can be triggered for existing resources (Brownfield).
- New assignments may take up to **30 minutes** to propagate.
- Scan duration depends on policy complexity, scope size, and system load.

### Resource Compliance States
Resources are marked as:
- **Compliant**
- **Non-compliant**
- **Error**
- **Conflicting**
- **Protected**
- **Exempted / Unknown**

Compliance percentage is calculated based on compliant, exempt, and unknown resources versus total resources.

### Enforcement Mode
- **Enabled**: Policy effects are enforced.
- **Disabled (DoNotEnforce)**: Policies evaluate but don’t enforce actions (“what-if” mode).
- Useful for testing policies safely before full enforcement.

### Safe Deployment Best Practices
- Treat policy as code and test changes.
- Start with enforcement disabled.
- Roll out policies gradually using **deployment rings** (test → non-prod → prod).
- Validate compliance and application health at each stage.

### Reacting to Policy Changes
- Azure Policy emits events via **Azure Event Grid**.
- Events can trigger Azure Functions, Logic Apps,