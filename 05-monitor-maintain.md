# 5. Monitor and Maintain (10-15%)

> Azure Monitor, Log Analytics, alerts, Backup, Site Recovery, and Network Watcher.

---

## Domain mind map

```mermaid
mindmap
  root((Monitor + Maintain))
    Azure Monitor
      Metrics
        platform metrics
        custom metrics
      Logs
        Log Analytics workspace
        diagnostic settings
      Alerts
        metric alert
        log search alert
        activity log alert
      Action Groups
        email / SMS
        webhook / Azure Function / Logic App / runbook
      Workbooks
      Dashboards
      Application Insights
    Log Analytics
      Workspaces
      Tables
      KQL basics
        where / project / summarize / count
      Solutions
    Backup
      Recovery Services Vault
      Backup Center
      Backup policies
        schedule + retention
      Workloads
        VM
        SQL on VM
        Azure Files
        Workload in VM - SQL, SAP HANA
      Soft delete
    Site Recovery - ASR
      Replication
      Recovery plans
      Failover and failback
      Test failover
    Network Watcher
      Connection Monitor
      IP flow verify
      NSG flow logs v2
      Packet Capture
      Effective routes / security rules
    Updates
      Azure Update Manager
      Maintenance Configurations
```

---

## Diagnostic data path

```mermaid
flowchart LR
    R[Azure resource] --> D[Diagnostic Setting]
    D --> LA[Log Analytics workspace]
    D --> SA[Storage account<br/>archive]
    D --> EH[Event Hub<br/>SIEM / 3rd party]
    LA --> Query[KQL queries]
    Query --> Alert[Log search alerts]
    Query --> WB[Workbooks]
    Alert --> AG[Action Group]
```

A diagnostic setting can route to **up to 5 destinations** - one Log Analytics workspace, one or more storage accounts, and one or more Event Hubs / partner integrations.

---

## Alert types

```mermaid
flowchart TD
    A[Need an alert] --> M{What signal?}
    M -- "Numeric metric (CPU, IOPS)" --> Metric[Metric alert<br/>real-time, near-zero latency]
    M -- "Pattern in logs / KQL" --> Log[Log search alert<br/>runs query on schedule]
    M -- "Subscription-level event" --> Activity[Activity log alert<br/>e.g. resource deleted]
    Metric --> AG[Action Group]
    Log --> AG
    Activity --> AG
```

| Alert type | Signal | Latency | Cost driver |
|---|---|---|---|
| Metric alert | Platform / custom metrics | Near real-time | Per metric monitored |
| Log search alert | KQL over Log Analytics | Min frequency 1-5 min | Per query execution |
| Activity log alert | Subscription events | Seconds | Free |

---

## Action Group destinations

```mermaid
flowchart LR
    AG[Action Group] --> Email[Email]
    AG --> SMS[SMS]
    AG --> Push["Mobile push (Azure mobile app)"]
    AG --> Voice[Voice call]
    AG --> WH[Webhook]
    AG --> AF[Azure Function]
    AG --> LA[Logic App]
    AG --> AR[Automation runbook]
    AG --> ITSM[ITSM connector]
```

---

## KQL quick reference

```kql
// Errors in last hour by application
AppExceptions
| where TimeGenerated > ago(1h)
| summarize count() by AppRoleName, ProblemId
| order by count_ desc

// VMs with average CPU > 80% over 5 minutes
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5m)
| where avg_CounterValue > 80
```

Top operators:

- `where` filter rows
- `project` keep specific columns
- `extend` add a calculated column
- `summarize ... by` aggregate
- `join` combine tables
- `bin(timestamp, span)` bucket time

---

## Backup vs Site Recovery

```mermaid
flowchart TD
    Goal[What is the goal?] --> Q1{RTO target?}
    Q1 -- Hours / days --> Backup[Azure Backup<br/>RPO hours, retention years]
    Q1 -- Minutes --> ASR[Azure Site Recovery<br/>RPO seconds-minutes]
    Backup --> RSV[Recovery Services Vault]
    ASR --> RSV
    RSV --> SD[Soft delete enabled by default<br/>14 days minimum]
```

| Capability | Azure Backup | Azure Site Recovery |
|---|---|---|
| Primary purpose | Long-term data retention | Disaster recovery / region failover |
| RPO | Hours (or app-consistent) | Seconds-minutes |
| RTO | Hours | Minutes |
| Stores | Backup data in vault | Replica disks in target region |
| Test recovery | File / disk restore | Test failover (non-disruptive) |

Azure Backup workloads:

- Azure VM backup (agentless snapshot).
- Azure Files backup (share snapshot).
- SQL Server / SAP HANA in Azure VM (workload backup extension).
- On-prem servers via MARS agent or MABS / DPM.

---

## Backup policy components

```mermaid
flowchart LR
    Policy[Backup policy] --> Sched[Schedule<br/>daily / weekly / hourly]
    Policy --> Ret[Retention<br/>daily / weekly / monthly / yearly]
    Policy --> Inst[Instant Restore tier<br/>1-5 days snapshots]
    Policy --> Vault[Vault retention<br/>up to 9999 days]
```

Soft delete on Recovery Services Vault is **enabled by default** and cannot be disabled below 14 days; Enhanced soft delete adds immutability.

---

## ASR failover lifecycle

```mermaid
sequenceDiagram
    participant Admin
    participant ASR
    participant Source as Source region
    participant Target as Target region
    Admin->>ASR: Enable replication on VM
    ASR->>Source: Read changes
    ASR->>Target: Continuous replication
    Admin->>ASR: Test failover
    ASR->>Target: Spin up VM in isolated network
    Admin->>ASR: Cleanup test
    Note over Admin,ASR: Real disaster
    Admin->>ASR: Failover (commit)
    ASR->>Target: Promote replicas to primary
    Admin->>ASR: Reprotect (reverse replication)
    Admin->>ASR: Failback to source
```

Always run a **test failover** quarterly. Document recovery plans (groups + scripts + manual actions).

---

## Network Watcher tools

| Tool | Use |
|---|---|
| Connection Monitor | End-to-end reachability + latency over time |
| IP flow verify | "Is this packet allowed by NSG rules right now?" |
| Effective security rules | Combined NSG result on a NIC |
| Effective routes | Final routing table on a NIC |
| NSG flow logs (v2) | Persisted flow records to storage; analyze in Traffic Analytics |
| Packet Capture | tcpdump-like capture from VM extension |
| Connection Troubleshoot | One-time test from VM A to endpoint X |

```mermaid
flowchart TD
    P[VM cannot reach DB] --> IP[IP flow verify<br/>NSG allowing?]
    P --> ER[Effective routes<br/>going via expected next hop?]
    P --> CM[Connection Monitor<br/>persistent test]
    P --> PC[Packet Capture<br/>see actual packets]
```

---

## Updates and maintenance

```mermaid
flowchart LR
    UM[Azure Update Manager] --> Eval[Periodic assessment]
    UM --> Sched[Maintenance Configuration<br/>schedule + reboot setting]
    UM --> Win[Windows OS]
    UM --> Lin[Linux OS]
    UM --> Arc[Azure Arc-enabled servers]
```

Use **Maintenance Configurations** to apply patches in waves (e.g. ring 1 dev -> ring 2 test -> ring 3 prod).

---

## Monitor exam clue patterns

| Phrase | Likely answer |
|---|---|
| "alert on disk space < 10%" | Metric alert on `OSDiskFreePercentage` (or KQL log search if guest metric) |
| "send notification + create ticket" | Action Group with email + ITSM connector |
| "diagnostic logs to SIEM" | Diagnostic setting -> Event Hub |
| "store NSG flows" | NSG flow logs v2 -> storage account; Traffic Analytics for visualization |
| "RPO of seconds, cross-region" | Azure Site Recovery |
| "30-day backup retention" | Azure Backup policy with 30-day daily retention |
| "test failover without impact" | ASR Test failover into isolated VNet |
| "patch all VMs Tuesday 2 am" | Azure Update Manager + Maintenance Configuration |

---

**Next:** [06-exam-cheatsheet.md](06-exam-cheatsheet.md)
