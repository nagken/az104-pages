# 4. Virtual Networking (15-20%)

> VNets, subnets, NSGs, peering, VPN/ExpressRoute, name resolution, load balancing, and network security.

---

## Domain mind map

```mermaid
mindmap
  root((Virtual Networking))
    VNet and Subnets
      Address space CIDR
      Subnets
      Reserved IPs (5 per subnet)
      Service endpoints
      Subnet delegation
    NSG and ASG
      Inbound / outbound rules
      Priority 100-4096
      Default rules
      Application Security Groups
      Effective rules
    Connectivity
      VNet Peering
        same / cross region
        no transitive
      VPN Gateway
        Site-to-Site
        Point-to-Site
        SKU + active-active
      ExpressRoute
        Private peering
        Microsoft peering
        FastPath
      Virtual WAN
    Name Resolution
      Azure DNS public
      Azure Private DNS Zones
      Default Azure-provided DNS
    Load Balancing
      Azure Load Balancer
        Basic vs Standard
        Public vs Internal
      Application Gateway
        WAF SKU
        Path-based routing
      Traffic Manager
        DNS-based, global
      Front Door
        global L7 + WAF + CDN
    Network Security
      Azure Firewall
      Azure Bastion
      Route Tables (UDR)
      DDoS Protection
      Private Link / Endpoint
```

---

## VNet and subnet basics

```mermaid
flowchart LR
    VNet["VNet 10.0.0.0/16"] --> S1["subnet-web 10.0.1.0/24"]
    VNet --> S2["subnet-app 10.0.2.0/24"]
    VNet --> S3["subnet-data 10.0.3.0/24"]
    VNet --> Bastion["AzureBastionSubnet 10.0.250.0/26"]
    VNet --> GW["GatewaySubnet 10.0.255.0/27"]
```

Reserved IPs in **every** subnet (first 4 + last):

| Offset | Use |
|---|---|
| `.0` | Network address |
| `.1` | Default gateway |
| `.2`, `.3` | Azure DNS |
| last (`.255` in /24) | Broadcast |

So a `/24` subnet has 256 - 5 = **251 usable IPs**.

Special required subnet names:

- `GatewaySubnet` for VPN / ExpressRoute gateway (recommended `/27` or larger).
- `AzureBastionSubnet` for Bastion (`/26` or larger).
- `AzureFirewallSubnet` for Azure Firewall (`/26` or larger).

---

## NSG decision flow

```mermaid
flowchart TD
    P[Packet hits NIC] --> Sub[Subnet NSG inbound rules]
    Sub -- allow --> NIC[NIC NSG inbound rules]
    NIC -- allow --> VM[VM]
    Sub -- deny --> Drop1[Drop]
    NIC -- deny --> Drop2[Drop]
    VM -- outbound --> NICo[NIC NSG outbound]
    NICo -- allow --> Subo[Subnet NSG outbound]
    Subo -- allow --> Net[Internet / VNet / VirtualNetwork tag]
```

NSG rule mechanics:

- Rules are processed by **priority** (100 = highest, 4096 = lowest).
- First match wins; remaining rules are ignored.
- Default rules (priority 65000+) cannot be deleted but can be overridden.
- Use **service tags** (`Internet`, `VirtualNetwork`, `AzureLoadBalancer`, `Storage`) instead of IP ranges where possible.
- **Application Security Groups (ASGs)** group VMs by role (e.g. `asg-web`, `asg-db`) so rules use names instead of IPs.

---

## VNet peering vs VPN vs ExpressRoute

```mermaid
flowchart TD
    Goal[Connect networks] --> P{Both Azure VNets?}
    P -- Yes --> Peer[VNet Peering<br/>private, low latency<br/>not transitive]
    P -- No on-prem --> Speed{Bandwidth + SLA need?}
    Speed -- Modest, encrypted internet --> S2S[Site-to-Site VPN<br/>over public internet<br/>IPsec/IKE]
    Speed -- Predictable bandwidth + SLA --> ER[ExpressRoute<br/>private circuit via partner<br/>50 Mbps - 100 Gbps]
    Speed -- Individual remote users --> P2S[Point-to-Site VPN<br/>per-user cert / Entra ID]
```

| Option | Encryption | Bandwidth | SLA | Use |
|---|---|---|---|---|
| VNet Peering | Microsoft backbone | High | Yes | Azure-to-Azure |
| Site-to-Site VPN | IPsec | Up to ~1.25 Gbps | 99.95% | On-prem branch / DC |
| Point-to-Site VPN | IPsec / OpenVPN | Per user | n/a | Remote users |
| ExpressRoute | Private (no public internet) | Up to 100 Gbps | 99.95% | Large enterprise |
| ExpressRoute Direct | Private | 10/100 Gbps | 99.95% | Massive throughput |

**Peering is non-transitive.** A -> B and B -> C does **not** give A -> C. Use a hub VNet with **Azure Firewall / NVA + UDR**, or **Virtual WAN**, to enable transitive routing.

---

## Hub-and-spoke pattern

```mermaid
flowchart LR
    OnPrem[On-prem DC] -.S2S VPN or ExpressRoute.-> Hub[(Hub VNet<br/>Azure Firewall + Bastion + DNS)]
    Hub <-->|peering| Spoke1[(Spoke 1<br/>workload A)]
    Hub <-->|peering| Spoke2[(Spoke 2<br/>workload B)]
    Spoke1 -.->|UDR default route to Hub Firewall| Hub
    Spoke2 -.->|UDR| Hub
```

User-Defined Routes (UDR) on spoke subnets force traffic through the hub firewall. This enables **transitive** spoke-to-spoke and spoke-to-on-prem routing.

---

## Name resolution decision

```mermaid
flowchart TD
    Q[Where do clients query DNS?] --> A{Public name?}
    A -- Yes --> AzDNS[Azure DNS public zone<br/>contoso.com]
    A -- Internal-only --> Priv{VNet-internal?}
    Priv -- Yes --> PZ[Azure Private DNS Zone<br/>linked to VNets]
    Priv -- Hybrid w/ on-prem --> Res[DNS Private Resolver<br/>conditional forwarding]
    PZ --> PE[For Private Endpoint<br/>privatelink.<service>.core.windows.net]
```

When you create a **Private Endpoint**, you must integrate with a Private DNS zone (or your own DNS) so the FQDN resolves to the private IP.

---

## Load balancing decision tree

```mermaid
flowchart TD
    LB[Choose a load balancer] --> Layer{Layer 4 or 7?}
    Layer -- "L4 TCP/UDP, regional" --> Az[Azure Load Balancer]
    Az --> AzPub{Public or internal?}
    AzPub --> AzPubLB[Public LB<br/>frontend public IP]
    AzPub --> AzILB[Internal LB<br/>private frontend]
    Layer -- "L7 HTTP/HTTPS, regional" --> AppGW[Application Gateway<br/>+ optional WAF]
    Layer -- "Global L4/DNS routing" --> TM[Traffic Manager<br/>DNS-based]
    Layer -- "Global L7 + CDN + WAF" --> FD[Azure Front Door]
```

| Service | Layer | Scope | Best for |
|---|---|---|---|
| Azure Load Balancer | L4 | Regional | Internal microservice LB, non-HTTP TCP |
| Application Gateway | L7 | Regional | HTTPS, WAF, path/host routing inside region |
| Traffic Manager | DNS | Global | Endpoint failover by DNS, any protocol |
| Azure Front Door | L7 | Global | Global HTTP(S), WAF, CDN, anycast IP |

Standard Load Balancer requires **Standard Public IP** and supports **AZs**. Basic LB is being deprecated.

---

## Application Gateway features

```mermaid
flowchart LR
    Client[Client] --> AppGW[Application Gateway<br/>WAF v2 SKU]
    AppGW -- "/api/*" --> Pool1[Backend pool: APIs]
    AppGW -- "/images/*" --> Pool2[Backend pool: blob storage]
    AppGW -- "/" --> Pool3[Backend pool: web VMs]
    AppGW -.SSL termination + cookie session.-> Client
```

- **Listeners** receive traffic (Basic / Multi-site).
- **Rules** map listeners to backend pools.
- **Health probes** mark backend instances healthy/unhealthy.
- WAF SKU adds OWASP rule sets and bot protection.

---

## Bastion vs jump VM vs public IP

```mermaid
flowchart TD
    Need[Admin access to a VM] --> P{Public IP on VM allowed?}
    P -- No, but I need RDP/SSH --> B[Azure Bastion<br/>browser-based SSH/RDP<br/>over private IP]
    P -- Sometimes --> JIT[Just-In-Time access<br/>Microsoft Defender for Cloud]
    P -- Yes, regulated --> NSGd[NSG: source = office IP only]
```

Bastion is deployed into **AzureBastionSubnet**. The target VM does not need a public IP.

---

## UDR mini reference

| Next hop type | Use case |
|---|---|
| `Internet` | Force traffic to the public internet |
| `Virtual network` | Default within VNet |
| `Virtual network gateway` | Send to on-prem |
| `Virtual appliance` | Send to NVA / Azure Firewall |
| `None` | Drop the traffic |

---

## Networking exam clue patterns

| Phrase | Likely answer |
|---|---|
| "non-transitive routing problem" | Hub-and-spoke + Azure Firewall + UDR, or Virtual WAN |
| "private connectivity to PaaS" | Private Endpoint + Private DNS zone |
| "WAF / OWASP" | Application Gateway WAF v2 or Front Door WAF |
| "lowest-latency global" | Front Door (anycast) |
| "secure RDP/SSH without public IP" | Azure Bastion |
| "must allow exact ports between subnets" | NSG rules with ASGs |
| "encrypted tunnel to one branch office" | S2S VPN |
| "predictable 10 Gbps to data center" | ExpressRoute |
| "remote employees connect from laptops" | Point-to-Site VPN |
| "block VM internet egress" | UDR with `0.0.0.0/0` next hop = `None`, or via Azure Firewall |

---

**Next:** [05-monitor-maintain.md](05-monitor-maintain.md)
