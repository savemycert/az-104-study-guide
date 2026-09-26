# AZ-104 Commonly Confused Services and Concepts

Side-by-side comparisons of the AZ-104 services and ideas that exam questions most often set against each other. Each table ends with the one thing to remember.

- [Domain 1: Manage Azure identities and governance](#domain-1-manage-azure-identities-and-governance)
- [Domain 2: Implement and manage storage](#domain-2-implement-and-manage-storage)
- [Domain 3: Deploy and manage Azure compute resources](#domain-3-deploy-and-manage-azure-compute-resources)
- [Domain 4: Implement and manage virtual networking](#domain-4-implement-and-manage-virtual-networking)
- [Domain 5: Monitor and maintain Azure resources](#domain-5-monitor-and-maintain-azure-resources)

## Domain 1: Manage Azure identities and governance

### Azure RBAC roles vs Microsoft Entra roles

| | Azure RBAC roles | Microsoft Entra roles |
|---|---|---|
| What it controls | Azure resources such as VMs, storage accounts, and virtual networks | The directory itself: users, groups, applications, domains, settings |
| Example roles | Owner, Contributor, Reader, User Access Administrator | Global Administrator, User Administrator |
| Scope | Management group, subscription, resource group, resource | Tenant, or an administrative unit |
| Where you assign it | Access control (IAM) blade | Microsoft Entra ID > Roles and administrators |
| Exam cue | "manage VMs", "access to the resource group" | "create users", "manage groups", "add a domain" |

**Remember:** the two systems are independent. A Global Administrator has no subscription access until they choose to elevate, and a subscription Owner cannot manage directory users.

### Owner vs Contributor vs User Access Administrator

| | Owner | Contributor | User Access Administrator |
|---|---|---|---|
| Create, change, delete resources | Yes | Yes | No |
| Assign roles to others | Yes | No | Yes |
| Use it when | Someone must run resources and delegate access | Someone builds and operates resources only | Someone delegates access without touching workloads |
| Exam cue | "manage access as well as resources" | "manage resources but not permissions" | "manage role assignments only" |

**Remember:** managing access is the dividing line. Contributor can do everything to a resource except grant someone else access to it.

### Azure Policy vs Azure RBAC vs resource locks

| | Azure Policy | Azure RBAC | Resource locks |
|---|---|---|---|
| Question it answers | What configuration is allowed? | Who can perform which actions? | Can this resource be changed or deleted at all? |
| Evaluates | Resource properties on create, update, and existing state | The caller's role assignments | Every write or delete against the locked scope |
| Starting point | Everything allowed until a policy restricts it | Nothing allowed until a role grants it | No lock until you add one |
| Stops an Owner? | Yes, with a Deny effect | Not applicable (Owner already holds the rights) | Yes, until the lock is removed |
| Exam cue | "only approved regions", "require a tag" | "grant the team access" | "prevent accidental deletion" |

**Remember:** RBAC decides who, Policy decides what, and a lock blocks the change no matter who is making it.

### Audit vs Deny vs Modify / DeployIfNotExists

| | Audit | Deny | Modify / DeployIfNotExists |
|---|---|---|---|
| New non-compliant request | Allowed and flagged | Blocked | Corrected or completed as it is deployed |
| Existing non-compliant resources | Reported only | Reported only | Fixed through a remediation task |
| Needs a managed identity | No | No | Yes, with a role that can make the change |
| Exam cue | "report but don't block" | "block non-compliant creation" | "auto-fix existing resources" |

**Remember:** Audit reports, Deny blocks, and only Modify and DeployIfNotExists can remediate what already exists.

### CanNotDelete lock vs ReadOnly lock

| | CanNotDelete | ReadOnly |
|---|---|---|
| Read | Allowed | Allowed |
| Modify | Allowed | Blocked |
| Delete | Blocked | Blocked |
| Side effects | Few | Can break operations that use a write call to read, such as listing storage keys |
| Exam cue | "prevent accidental deletion" | "freeze the configuration" |

**Remember:** choose CanNotDelete for production safety. ReadOnly also freezes changes, which is more than most scenarios ask for.

## Domain 2: Implement and manage storage

### LRS vs ZRS vs GRS vs GZRS

| | LRS | ZRS | GRS | GZRS |
|---|---|---|---|---|
| Primary region copies | 3, in one datacenter | 3, across 3 availability zones | 3, in one datacenter | 3, across 3 availability zones |
| Second region | None | None | Asynchronous copy to the paired region | Asynchronous copy to the paired region |
| Survives | Disk or server failure | Zone or datacenter outage | Full region outage | Zone outage and full region outage |
| Read from secondary | Not applicable | Not applicable | Only with RA-GRS | Only with RA-GZRS |
| Exam cue | "cheapest", "single datacenter" | "survive a datacenter loss, stay in one region" | "survive a region outage" | "highest durability" |

**Remember:** "region outage" means a G option, and "read from the secondary region" means an RA- option. Plain GRS and GZRS secondaries cannot be read until a failover.

### Account SAS vs service SAS vs user delegation SAS

| | Account SAS | Service SAS | User delegation SAS |
|---|---|---|---|
| Signed with | A storage account key | A storage account key | A key issued to a Microsoft Entra identity |
| Covers | Several services and account-level operations | Specific resources in one service | Blob storage resources |
| Revoke early by | Regenerating the signing key | Removing its stored access policy, or regenerating the key | Revoking the Entra credential or its permissions |
| Exam cue | "access across blob, file, queue, and table" | "one container, revocable, client has no Entra identity" | "most secure SAS", "Entra credentials" |

**Remember:** key-signed SAS tokens die when the key is regenerated. To revoke a single service SAS without touching keys, issue it against a stored access policy.

### Access keys vs SAS vs Microsoft Entra ID with RBAC

| | Access keys | SAS token | Microsoft Entra ID + RBAC |
|---|---|---|---|
| What it grants | Full control of the entire account | Chosen permissions on chosen resources for a time window | Role-based access per identity and scope |
| Can be narrowed | No | Yes (permissions, expiry, IP, protocol) | Yes (role and scope) |
| Shared secret | Yes | Yes (the token itself) | No |
| Use it when | Initial setup or emergencies | A client needs delegated, time-limited access | The caller can have an Entra identity |

**Remember:** most to least secure is Entra RBAC, user delegation SAS, service SAS, then access keys.

### Hot vs cool vs cold vs archive

| | Hot | Cool | Cold | Archive |
|---|---|---|---|---|
| Availability | Online | Online | Online | Offline until rehydrated |
| Storage cost | Highest | Lower | Lower still | Lowest |
| Access cost | Lowest | Higher | Higher still | Highest |
| Minimum retention | None | 30 days | 90 days | 180 days |
| Exam cue | "accessed frequently" | "infrequent, kept 30+ days" | "rarely accessed, kept 90+ days" | "long-term retention, must rehydrate" |

**Remember:** the first three tiers are all instantly readable. Archive is the only tier you must rehydrate, which can take hours at Standard priority.

### Blob soft delete vs blob versioning vs share snapshots

| | Blob soft delete | Blob versioning | Azure Files share snapshot |
|---|---|---|---|
| Service | Blob Storage | Blob Storage | Azure Files |
| Protects against | Deleting a blob (container soft delete covers containers) | Overwriting a blob | File changes or deletions inside a share |
| How it works | Keeps deleted data for a retention window | Saves each prior state as a version with its own ID | Read-only, incremental, point-in-time copy of the share |
| Exam cue | "recover a deleted blob" | "keep the previous version on overwrite" | "restore a file from yesterday", "Previous Versions" |

**Remember:** soft delete undoes deletions, versioning undoes overwrites, and share snapshots are the file-share equivalent of a point-in-time restore.

## Domain 3: Deploy and manage Azure compute resources

### Availability sets vs availability zones vs Virtual Machine Scale Sets

| | Availability set | Availability zones | Virtual Machine Scale Set |
|---|---|---|---|
| What it is | Grouping of VMs across fault and update domains | Placing VMs in physically separate datacenters in a region | A managed group of identical, load-balanced VM instances |
| Protects against | Rack failure and planned maintenance in one datacenter | Loss of an entire datacenter | Demand spikes, plus zone loss when spread across zones |
| Scales automatically | No | No | Yes, on metrics or a schedule |
| VM SLA (2+ VMs) | 99.95% | 99.99% | Depends on how it is placed |
| Exam cue | "fault and update domains" | "survive a datacenter failure", "99.99%" | "identical VMs that grow and shrink with demand" |

**Remember:** sets and zones are about where VMs sit. A scale set is about how many run, and it can also use zones.

### Incremental vs Complete deployment mode

| | Incremental | Complete |
|---|---|---|
| Default | Yes | No (for example, `--mode Complete` on the CLI) |
| Resources in the template | Created or updated | Created or updated |
| Other resources in the resource group | Left untouched | Deleted |
| Use it when | Everyday deployments | The group must contain exactly what the template declares |
| Exam cue | "leave existing resources alone" | "deletes resources not in the template" |

**Remember:** Complete mode treats the template as the full inventory of the resource group, and anything missing from it gets deleted.

### ARM template vs Bicep file

| | ARM template | Bicep file |
|---|---|---|
| Format | JSON (`.json`) | Concise declarative language (`.bicep`) |
| Dependencies | Often explicit `dependsOn` | Inferred from symbolic references |
| Deployment | Sent to Resource Manager as is | Transpiled to ARM JSON first |
| Convert | `az bicep decompile` turns JSON into Bicep | `az bicep build` turns Bicep into JSON |
| Exam cue | "exported template", "existing JSON" | "cleaner authoring", "modules", "automatic dependencies" |

**Remember:** Bicep and ARM JSON have the same capabilities because Bicep compiles to ARM JSON. Decompile always goes from JSON to Bicep.

### Container Instances vs Container Apps vs App Service

| | Azure Container Instances | Azure Container Apps | Azure App Service |
|---|---|---|---|
| What it runs | One container or a container group | Serverless containerized apps and microservices | Web apps and APIs from code or a container |
| Scaling | None, fixed CPU and memory | Scale rules between min and max replicas, including zero | Manual or autoscale instances of the plan (Standard and above) |
| You pay for | vCPU and memory per second while running | Active replicas | The App Service plan, whether busy or idle |
| Release tooling | None | Revisions with traffic splitting | Deployment slots with swap |
| Exam cue | "run a container quickly", "job that exits" | "scale to zero", "queue-driven", "microservices" | "web app", "staging slot", "custom domain and TLS" |

**Remember:** ACI runs something once, Container Apps scales containers down to zero, and App Service hosts web apps on a plan you pay for continuously.

### Scale up vs scale out (App Service)

| | Scale up | Scale out |
|---|---|---|
| What changes | The plan's pricing tier | The number of instances |
| Direction | Vertical | Horizontal |
| Unlocks features | Yes, higher tiers add slots, autoscale, and more | No, only capacity |
| Automatic option | No | Autoscale rules on a metric or schedule (Standard and above) |
| Exam cue | "bigger instances", "more memory per instance" | "more instances", "add instances when CPU is high" |

**Remember:** up makes each instance bigger, out adds more of them, and only out can be automated.

## Domain 4: Implement and manage virtual networking

### Network Security Groups vs Application Security Groups

| | Network security group (NSG) | Application security group (ASG) |
|---|---|---|
| What it is | A set of prioritized allow and deny rules | A named grouping of VM network interfaces by role |
| Filters traffic? | Yes, by 5-tuple, direction, and action | No, it is only referenced inside NSG rules |
| Attached to | A subnet, a NIC, or both | Individual NICs join it |
| Use it when | Deciding what traffic may reach or leave resources | You want rules like "web tier to database tier" that survive IP changes and scaling |
| Exam cue | "priority", "deny inbound RDP", "subnet or NIC level" | "group VMs by workload", "instead of IP addresses" |

**Remember:** the ASG says which machines, and the NSG says what traffic. An ASG does nothing without an NSG rule that names it.

### Service Endpoints vs Private Endpoints

| | Service endpoint | Private endpoint |
|---|---|---|
| What it is | Lets a PaaS service recognize traffic from your subnet, carried on the Microsoft backbone | A private IP from your subnet mapped to the PaaS service through Private Link |
| Service's public endpoint | Still exists; its firewall is limited to your subnet | Can be switched off entirely |
| IP address in your VNet | None | Yes, a NIC with a private IP |
| Extra setup | Enabled per subnet and service type | Usually paired with a private DNS zone |
| Cost | Free | Billed per endpoint and data processed |
| Exam cue | "keep traffic on the backbone", "restrict to a subnet" | "private IP in the VNet", "no public access" |

**Remember:** if the service must have no public exposure, the answer is a private endpoint. A service endpoint only trusts your subnet on a public endpoint.

### Azure Load Balancer vs Application Gateway vs Traffic Manager vs Azure Front Door

| | Azure Load Balancer | Application Gateway | Traffic Manager | Azure Front Door |
|---|---|---|---|---|
| Layer | 4 (TCP/UDP) | 7 (HTTP/S) | DNS | 7 (HTTP/S) |
| Scope | Regional | Regional | Global | Global |
| In the data path? | Yes | Yes | No, it only answers DNS queries | Yes, at the edge |
| Notable features | Health probes, NAT rules, session persistence | Path and host routing, TLS termination, WAF | Priority, weighted, performance, geographic routing | CDN, WAF, edge TLS |
| Exam cue | "any TCP/UDP", "internal app tier" | "URL path routing", "WAF" in one region | "DNS-based routing across regions" | "global web acceleration" |

**Remember:** first decide regional or global, then decide whether the service must read HTTP. Load Balancer never looks at URLs.

### Public Load Balancer vs Internal Load Balancer

| | Public load balancer | Internal load balancer |
|---|---|---|
| Frontend | Public IP address | Private IP from a VNet subnet |
| Reachable from | The internet | Inside the network only |
| Typical placement | In front of internet-facing web servers | In front of a private app or database tier |
| Also provides | Outbound connectivity for the backends | Nothing extra |
| Exam cue | "balance internet requests" | "distribute internal TCP traffic across a private tier" |

**Remember:** the backend pool, probes, and rules are identical in both. Only the frontend IP type decides which one you built.

### Public DNS Zones vs Private DNS Zones

| | Public DNS zone | Private DNS zone |
|---|---|---|
| Answers queries from | Anywhere on the internet | Only VNets linked to the zone |
| How it goes live | Delegation: point the registrar's NS records at Azure's four name servers | A virtual network link to each VNet that needs it |
| Record upkeep | You manage records | Autoregistration can create and remove VM A records |
| Use it when | Hosting records for a domain you own | Resolving VM names with a custom domain, including across peered VNets |
| Exam cue | "update name servers at the registrar" | "resolve VM names privately", "autoregistration" |

**Remember:** a public zone needs delegation at the registrar, and a private zone needs a virtual network link. Neither registers domain names for you.

## Domain 5: Monitor and maintain Azure resources

### Azure Monitor Metrics vs Logs

| | Metrics | Logs |
|---|---|---|
| What they are | Numeric values sampled at regular intervals | Structured event records with many fields |
| Freshness | Near real time | Slight ingestion delay |
| Where you read them | Metrics Explorer, with no query language | Log Analytics, using KQL |
| Stored in | A time-series metrics database | A Log Analytics workspace |
| Use them when | You need live charts or the fastest threshold alerts | You need to investigate, correlate across resources, or keep audit history |
| Exam cue | "CPU percentage", "numeric near-real-time" | "query events", "KQL", "correlate" |

**Remember:** metrics give you the current number, and logs give you the story behind an event.

### Alert Rules vs Action Groups vs Alert Processing Rules

| | Alert rule | Action group | Alert processing rule |
|---|---|---|---|
| What it is | A condition on a metric or log signal | A reusable list of notifications and automated actions | A layer that modifies fired alerts at scale |
| Decides | When an alert fires, and its severity | Who is told and what runs | Whether alerts are suppressed or sent to a different action group |
| Contains | Scope, condition, threshold, frequency, severity 0 to 4 | Email, SMS, push, voice, webhook, Function, Logic App, runbook | Scope, schedule, and a suppress or route action |
| Exam cue | "fire when CPU exceeds a threshold" | "notify the on-call engineer", "run a runbook" | "silence alerts during a maintenance window" |

**Remember:** the rule detects, the action group responds, and the processing rule filters the response without editing each alert rule.

### Diagnostic Setting Destinations: Log Analytics Workspace vs Storage Account vs Event Hub

| | Log Analytics workspace | Storage account | Event hub |
|---|---|---|---|
| Purpose | Analysis | Archive | Streaming |
| What you do with the data | Query with KQL, build log alerts | Retain cheaply for months or years | Feed a third-party SIEM or external tool |
| Exam cue | "route platform logs to a workspace" | "long-term retention for audit" | "send to our SIEM" |

**Remember:** one diagnostic setting can target all three at once, so "archive and analyze" does not mean two settings.

### Recovery Services Vault vs Azure Backup Vault

| | Recovery Services vault | Azure Backup vault |
|---|---|---|
| What it is | The original backup and recovery container | The newer backup platform for modern datasources |
| Protects | Azure VMs, Azure Files, SQL Server and SAP HANA in a VM, on-premises servers via MARS | Azure Database for PostgreSQL, Azure Blobs, Azure managed disks |
| Used by Site Recovery | Yes | No |
| Managed from | Backup center | Backup center |
| Exam cue | "back up a VM", "configure Site Recovery" | "back up blobs", "back up managed disks" |

**Remember:** VMs, file shares, and Site Recovery go in a Recovery Services vault. The newer datasources go in a Backup vault.

### Azure Backup vs Azure Site Recovery

| | Azure Backup | Azure Site Recovery |
|---|---|---|
| Protects against | Deletion, corruption, ransomware | A region or site outage |
| Method | Recovery points taken on a policy schedule | Ongoing replication into a second region |
| Recovery action | Restore a whole VM, disks, or individual files | Fail the workload over and run it in the other region |
| Measured by | Retention (how far back you can go) | RPO and RTO |
| Exam cue | "point-in-time restore", "recover a deleted file" | "keep running during a regional outage", "test failover" |

**Remember:** Backup brings the data back, and Site Recovery keeps the service up. A workload that needs both protections uses both services.

[← Back to the study guide](README.md)
