# 1. Identity and Governance (20-25%)

> Manage Microsoft Entra ID objects, RBAC, subscriptions and governance, and Azure Policy.

---

## Domain mind map

```mermaid
mindmap
  root((Identity + Governance))
    Microsoft Entra ID
      Users
        Cloud / synced / guest
        Bulk create / invite
        SSPR
      Groups
        Security vs Microsoft 365
        Assigned vs Dynamic
        Group-based licensing
      Tenant
        Custom domains
        Self-service settings
        External collaboration
      External ID
        B2B guest invitations
        Cross-tenant access settings
    RBAC
      Built-in roles
        Owner / Contributor / Reader
        User Access Administrator
        Resource-specific - VM Contributor, Storage Blob Data Reader
      Custom roles
        Actions / NotActions
        DataActions / NotDataActions
        AssignableScopes
      Scope
        Mgmt Group then Sub then RG then Resource
        Inheritance + deny assignments
    Subscriptions and Mgmt Groups
      Tenant root
      Mgmt Groups
        Up to 6 levels deep
      Subscriptions
        Move between MGs
      Resource Groups
        Same lifecycle resources
      Tags
        Inherited from RG via Policy
      Resource Locks
        CanNotDelete
        ReadOnly
      Cost Mgmt
        Budgets
        Cost analysis
        Action groups on threshold
    Azure Policy
      Definition
      Initiative (set)
      Assignment
      Effects
        Audit
        Deny
        Append
        Modify
        DeployIfNotExists
        AuditIfNotExists
      Compliance
      Exemption
```

---

## Entra ID objects at a glance

| Object | Created where | Synced from on-prem? | Notes |
|---|---|---|---|
| Cloud user | Azure portal / Graph | No | Default for new tenant accounts |
| Synced user | On-prem AD via Entra Connect | Yes | Most attributes managed on-prem |
| Guest user (B2B) | Invited by email | No | External; consumes guest object in your tenant |
| Security group | Azure portal / Graph | Optionally synced | Used for RBAC / app assignment |
| Microsoft 365 group | Outlook / Teams / Graph | No | Adds shared mailbox + SharePoint site |
| Dynamic group | Membership rule | n/a | Requires Entra ID P1 license |

---

## Group membership decision tree

```mermaid
flowchart TD
    Q[Need to assign access<br/>to many users?] --> A{Same lifecycle<br/>same attributes?}
    A -- Yes, attributes change --> D[Dynamic group<br/>P1 required]
    A -- Manual list --> S[Assigned security group]
    A -- Need shared mailbox / Teams --> M[Microsoft 365 group]
    D --> R[Use group object ID<br/>in RBAC role assignment]
    S --> R
    M --> R
```

---

## RBAC vs Entra ID roles

```mermaid
flowchart LR
    subgraph EntraRoles[Microsoft Entra ID roles]
      E1[Global Administrator]
      E2[User Administrator]
      E3[Authentication Administrator]
    end
    subgraph AzureRBAC[Azure RBAC roles]
      A1[Owner]
      A2[Contributor]
      A3[Reader]
      A4[Storage Blob Data Reader]
    end
    EntraRoles -- "manages tenant identities" --> Tenant[(Microsoft Entra Tenant)]
    AzureRBAC -- "manages Azure resources" --> Mgmt[(Mgmt Group / Sub / RG)]
```

| Question | Use |
|---|---|
| Reset a user password | Entra role (User Administrator) |
| Create a VM | Azure RBAC (Contributor) |
| Manage Conditional Access policies | Entra role (Conditional Access Admin) |
| Read blobs in a container | Azure RBAC (Storage Blob Data Reader) |
| Assign roles to others | User Access Administrator (Azure RBAC) **or** Owner |

---

## RBAC scope hierarchy

```mermaid
flowchart TD
    TR[Tenant Root MG] --> MG1[Management Group prod]
    TR --> MG2[Management Group nonprod]
    MG1 --> S1[Subscription A]
    MG1 --> S2[Subscription B]
    S1 --> RG1[Resource Group rg-app]
    RG1 --> R1[Storage Account]
    RG1 --> R2[VM]
    classDef inherit fill:#eef2f7,stroke:#0f6cbd
    class TR,MG1,MG2,S1,S2,RG1,R1,R2 inherit
```

> Role assignments **inherit downward**. A `Reader` at Management Group applies to every subscription, RG and resource below it.

---

## Custom RBAC role recipe

```mermaid
flowchart LR
    A[Pick smallest built-in role] -->|covers it?| Y[Use built-in]
    A -->|missing actions?| C[Create custom role]
    C --> J[Define JSON]
    J --> Actions["Actions: Microsoft.Compute/* /read"]
    J --> NotActions["NotActions: Microsoft.Compute/virtualMachines/delete"]
    J --> Scope[AssignableScopes: subscription IDs]
    J --> Assign[Assign at smallest scope]
```

Common gotchas:

- A user needs `Microsoft.Authorization/roleAssignments/write` to grant roles. That comes from **Owner** or **User Access Administrator**.
- Data plane vs management plane: listing blobs in the portal needs both `Reader` (mgmt) and `Storage Blob Data Reader` (data) on the storage account.
- `Contributor` cannot assign roles. Frequently tested.

---

## Subscriptions, governance, and Policy

```mermaid
flowchart TD
    P[Need consistent setting<br/>across many subs/RGs] --> Q{Just block bad config?}
    Q -- Yes --> Deny[Policy effect: Deny]
    Q -- No, audit only --> Audit[Policy effect: Audit]
    Q -- Want to fix at scale --> DINE[Policy effect: DeployIfNotExists]
    Q -- Want tag inherited --> Modify[Policy effect: Modify or Append]
    DINE --> MI[Policy uses Managed Identity to remediate]
    Modify --> Tag[Tag inheritance from RG]
```

| Effect | Use case |
|---|---|
| `Audit` | Report on non-compliance without blocking |
| `Deny` | Hard block at create/update time |
| `Append` | Add fields to a request (e.g. tag) |
| `Modify` | Add or update tags / properties on existing resources |
| `DeployIfNotExists` | Auto-remediate (e.g. enable diagnostic settings) |
| `AuditIfNotExists` | Flag resources missing a related resource |

---

## Resource locks

```mermaid
flowchart LR
    L[Resource Lock] --> CD[CanNotDelete<br/>read+update OK]
    L --> RO[ReadOnly<br/>read only]
    note1[Locks inherit downward<br/>most restrictive wins<br/>can be applied at sub / RG / resource]
    L --- note1
```

- A `ReadOnly` lock on a resource group blocks deleting an RG and **also** modifying any resource inside.
- Locks affect **management plane** only - data plane (e.g. blob writes) is unaffected.
- Removing a lock requires `Microsoft.Authorization/locks/*` (Owner or User Access Administrator).

---

## Tags and cost management

```mermaid
flowchart LR
    Tag[Tag<br/>key=value] --> RG[Resource Group]
    Tag --> R[Resource]
    RG -.inherits via Policy.-> R
    Cost[Cost Analysis] --> ByTag[Group by tag<br/>e.g. costcenter]
    Budget[Budget] --> AG[Action Group<br/>email + webhook]
```

- Tags are not automatically inherited from RG to resources. Use the **Inherit a tag from the resource group** built-in policy (`Modify` effect).
- Maximum 50 tags per resource. Key length 512, value length 256.

---

## Identity exam clue patterns

| Phrase in question | Likely answer |
|---|---|
| "least privilege" | Built-in RBAC role at the lowest scope that works |
| "self-service password reset" | Enable SSPR; group-targeted in Entra ID |
| "external partner" | B2B guest invitation + cross-tenant access settings |
| "prevent users from deleting" | Resource Lock (CanNotDelete) |
| "audit all storage accounts for HTTPS" | Azure Policy (Audit effect) |
| "automatically enable diagnostic settings" | Azure Policy (DeployIfNotExists) with managed identity |
| "see costs by team" | Tags + Cost analysis grouped by tag |

---

**Next:** [02-storage.md](02-storage.md)
