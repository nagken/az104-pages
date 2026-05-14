# 2. Storage (15-20%)

> Storage accounts, Blob, Files, Queues, Tables, lifecycle, AzCopy, and access control.

---

## Domain mind map

```mermaid
mindmap
  root((Storage))
    Storage Account
      Kinds
        StorageV2 - general purpose v2
        BlockBlobStorage - premium
        FileStorage - premium files
      Performance
        Standard
        Premium
      Replication
        LRS
        ZRS
        GRS
        RA-GRS
        GZRS
        RA-GZRS
      Networking
        Public + selected networks
        Private endpoint
        Service endpoint
        Allowed IP firewall
      Auth
        Access keys
        SAS - account / service / user delegation
        Microsoft Entra ID - RBAC
    Blob
      Containers
      Block blob
      Append blob
      Page blob - VHDs
      Tiers
        Hot
        Cool
        Cold
        Archive
      Lifecycle Mgmt
      Soft delete
      Versioning
      Object Replication
      Immutable / WORM
    Azure Files
      Protocols
        SMB 3.1.1
        NFS 4.1
      Auth
        Storage key
        Entra Domain Services
        AD DS
      Snapshots and backup
      Azure File Sync
        Cloud tiering
        Sync groups
    Queues and Tables
      Queue - simple FIFO
      Table - NoSQL key-value
    Tools
      AzCopy
      Storage Explorer
      Azure CLI / PowerShell
      Import / Export service
```

---

## Pick a replication option

```mermaid
flowchart TD
    Start[Need to choose replication] --> Q1{Region failure must be survivable?}
    Q1 -- No --> Z{Zone failure?}
    Z -- Yes --> ZRS[ZRS<br/>3 copies across AZs]
    Z -- No --> LRS[LRS<br/>3 copies in 1 datacenter]
    Q1 -- Yes --> R{Need read access<br/>to secondary?}
    R -- No --> GRS[GRS<br/>async copy to paired region]
    R -- Yes --> RAGRS[RA-GRS<br/>read-only secondary endpoint]
    Q1 -- Yes + zone HA --> GZRS[GZRS or RA-GZRS]
```

| Option | Copies | Survives DC | Survives Zone | Survives Region |
|---|---|---|---|---|
| LRS | 3 in one DC | n | n | n |
| ZRS | 3 across AZs | y | y | n |
| GRS | 6 (3+3 paired region) | y | n in primary | y (after failover) |
| RA-GRS | Same as GRS + read endpoint | y | n | y + read anytime |
| GZRS | 3 across AZs + 3 in paired region | y | y | y |
| RA-GZRS | GZRS + read endpoint | y | y | y + read anytime |

---

## Pick a blob tier

```mermaid
flowchart TD
    A[How often is the blob accessed?] --> Hot[Hot tier<br/>frequent access]
    A --> Cool[Cool tier<br/>>=30 days, infrequent]
    A --> Cold[Cold tier<br/>>=90 days, rare]
    A --> Archive[Archive tier<br/>>=180 days, offline]
    Archive --> Rehy[Rehydrate to Hot/Cool<br/>standard hours / high priority]
    classDef warn fill:#fff4ce,stroke:#a76b00
    class Archive,Rehy warn
```

Trade-off: **higher access cost** for cool/cold/archive, but **lower storage cost**. Archive is offline; data is unavailable until rehydrated.

---

## Lifecycle management example

```mermaid
flowchart LR
    New[New blob in container 'logs'] -->|0 days| Hot
    Hot -->|after 30 days no read| Cool
    Cool -->|after 90 days no read| Archive
    Archive -->|after 365 days| Delete[Delete blob]
```

Lifecycle rules can target by **prefix**, **blob index tag**, **last modified**, **last access** (when access tracking enabled).

---

## Authorization options

```mermaid
flowchart TD
    Req[Request to storage] --> M{How is it auth'd?}
    M --> Key[Account key<br/>full access<br/>rotate carefully]
    M --> SAS[SAS token]
    M --> AAD[Microsoft Entra ID + RBAC<br/>recommended]
    SAS --> AccountSAS[Account SAS<br/>signed by key]
    SAS --> ServiceSAS[Service SAS<br/>signed by key]
    SAS --> UDSAS[User Delegation SAS<br/>signed by Entra ID key]
    AAD --> Roles[Storage Blob Data Owner / Contributor / Reader]
```

- Prefer **User Delegation SAS** over account-key SAS - it can be revoked by rotating the user delegation key, not the account key.
- Use **RBAC + Managed Identity** for app code wherever possible.
- Disable shared key access where compliance requires it (`allowSharedKeyAccess = false`).

---

## SAS quick reference

| Type | Signed by | Scope | Notes |
|---|---|---|---|
| Account SAS | Account key | All services in account | Can grant control-plane operations |
| Service SAS | Account key | One service (blob/file/queue/table) | Can be tied to a stored access policy |
| User Delegation SAS | Entra ID OAuth token | Blob only | Most secure; revocable |

---

## Network access decision

```mermaid
flowchart TD
    Q[How do clients reach the storage account?] --> P{Public internet OK?}
    P -- Yes anywhere --> AllNet[Networking: Enabled from all networks]
    P -- Yes from specific IPs --> Fw[Firewall: selected networks + IP rules]
    P -- No --> PE[Private Endpoint<br/>account reachable via private IP only]
    Fw --> SE[Service Endpoint<br/>VNet-aware but still public IP]
    PE --> DNS[Update Private DNS Zone<br/>privatelink.blob.core.windows.net]
```

Private endpoint vs service endpoint:

- **Service endpoint** = traffic from the VNet uses Microsoft backbone but the storage account still has a public IP.
- **Private endpoint** = the storage account gets a **private IP** in your VNet via Private Link.

---

## Azure Files: when to use what

```mermaid
flowchart LR
    Q{What protocol?} --> SMB[SMB 3.1.1<br/>Windows / Mac / Linux]
    Q --> NFS[NFS 4.1<br/>Linux only<br/>Premium tier]
    SMB --> Auth{Auth method?}
    Auth --> Key[Storage account key]
    Auth --> AADDS[Microsoft Entra Domain Services]
    Auth --> ADDS[On-prem AD DS]
    NFS --> NoAuth[No identity-based auth<br/>use NSGs / private endpoint]
```

**Azure File Sync** centralizes on-prem file servers around Azure Files:

- Cloud endpoint (file share) + server endpoints (Windows servers).
- **Cloud tiering** keeps frequently-used files local; cold files become reparse points.
- Backups happen on the cloud endpoint via Azure Backup.

---

## Tooling cheat sheet

| Need to... | Use |
|---|---|
| Bulk copy local -> blob | `azcopy copy "C:\data\*" "https://acct.blob.core.windows.net/c?<SAS>"` |
| Sync (one-way) | `azcopy sync` |
| Move data offline (TBs) | Azure Import/Export (BYOD) or Data Box |
| Browse and edit interactively | Azure Storage Explorer |
| Mount Azure Files on Linux | `mount -t cifs` (SMB) or `mount -t nfs` (NFS) |
| Mount on Windows | `net use` with storage key or AD identity |

---

## Soft delete vs versioning vs snapshots

```mermaid
flowchart LR
    SD[Soft delete] --> SD1[Recovers deleted blobs / containers / shares<br/>retention 1-365 days]
    V[Versioning] --> V1[Every overwrite creates new version<br/>previous version recoverable]
    SS[Snapshots] --> SS1[Read-only point-in-time copy<br/>manual or scheduled]
    Imm[Immutable storage] --> Imm1[WORM / time-based / legal hold<br/>compliance scenarios]
```

---

## Storage exam clue patterns

| Phrase | Likely answer |
|---|---|
| "rarely accessed but kept for years" | Cool / Archive tier with lifecycle rule |
| "must survive a region failure" | GRS / RA-GRS / GZRS |
| "Linux file share" | Azure Files NFS (Premium) |
| "centralize branch file servers" | Azure File Sync |
| "no public IP on storage" | Private Endpoint + Private DNS zone |
| "give a contractor temporary access" | User Delegation SAS |
| "regulatory immutable retention" | Immutable Blob Storage (time-based / legal hold) |
| "bulk upload from on-prem" | AzCopy or Data Box |

---

**Next:** [03-compute.md](03-compute.md)
