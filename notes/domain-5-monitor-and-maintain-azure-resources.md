# Domain 5: Monitor and maintain Azure resources (14%)

The smallest domain, but a predictable one. Monitoring questions turn on metrics versus logs and on where telemetry is routed. Recovery questions turn on Azure Backup versus Azure Site Recovery.

## Monitor resources in Azure

- **Azure Monitor** collects telemetry from resources, the platform, guest operating systems, and apps. It lets you chart it, alert on it, and automate a response.
- **Two data types.** Classify the question first:

| | Metrics | Logs |
|---|---|---|
| Shape | Numbers sampled over time | Structured records with many columns |
| Latency | Near real time | Short ingestion delay |
| Tool | Metrics Explorer (no query language) | Log Analytics, queried with KQL |
| Store | Time-series metrics database | Log Analytics workspace |
| Good at | Live charts, fastest alerts | Investigation, correlation, audit history |

- **Metrics:**
  - Collected automatically for most resources from creation, with nothing to enable.
  - Platform metrics are kept for 93 days by default.
  - In Metrics Explorer: pick a namespace and a metric, then an **aggregation** (avg, min, max, sum, count) over a time grain. Add **filters** or **split** by a dimension (for example, transactions by response type).
- **Logs:**
  - A **Log Analytics workspace** is the query container. Access control, retention, and pricing are set per workspace.
  - Activity logs, platform logs, guest OS logs, and custom data can share one workspace.
  - **KQL** is read-only. A query starts from a table and pipes through `where` (filter), `summarize` (aggregate), `project` (columns), and `sort`.
  - Query results can be pinned to a dashboard or turned into a **log alert**.
- **Diagnostic settings** decide where platform logs and metrics go. Most platform logs aren't retained anywhere queryable until you create one.
  - Choose the log categories and metrics per resource, then one or more destinations:

| Destination | Purpose |
|---|---|
| Log Analytics workspace | Query with KQL, alert, correlate |
| Storage account | Low-cost long-term archive and audit retention |
| Event hub | Stream to a third-party SIEM or external tool |

- A single diagnostic setting can send to several destinations at once, for example archive plus analysis.
- **Alerting has three layers:**

| Piece | Job |
|---|---|
| Alert rule | Watches a metric or log-query signal and fires on a condition. Sets the scope, threshold, evaluation frequency, and severity 0 (critical) to 4 (verbose). |
| Action group | Reusable "who hears about it and what runs": email, SMS, push, voice, webhook, Azure Function, Logic App, automation runbook |
| Alert processing rule | Suppresses alerts (maintenance windows) or reroutes them to other action groups at scale, without editing each rule |

- One action group can serve many alert rules.
- **Example pattern:** a CPU metric alert, an action group that emails on-call and starts a runbook, and a processing rule that mutes it during patching.
- **Insights** (prebuilt workbooks per resource type, still built on metrics and logs):
  - *VM insights*: guest performance, health, and process dependency maps. Needs the **Azure Monitor Agent** sending to a workspace.
  - *Boot diagnostics*: serial console output and a boot screenshot. It is for a VM that won't start, not for performance.
  - *Storage insights*: capacity, transactions, availability, and latency in one view. Use it to spot throttling and growth.
  - *Network insights*: topology and health across load balancers, gateways, public IPs, and similar resources.
- **Network Watcher** (regional) diagnoses network paths:
  - *IP flow verify* / *Connection troubleshoot*: is the traffic allowed, and which rule blocked it?
  - *Next hop*: where the packet is routed. Use it to catch a UDR diverting traffic.
  - *NSG flow logs*: a record of flows an NSG allowed or denied.
  - *Packet capture*: actual packets on a VM.
- **Connection Monitor** (part of Network Watcher) tests reachability, latency, and packet loss **continuously**. It covers VM to VM, VM to URL, and on-premises to Azure, stores the history, and alerts on degradation.
- **Cues:**
  - "Numeric, near real time, CPU %" → metrics / Metrics Explorer.
  - "Query events with KQL" → logs / Log Analytics workspace.
  - "Send platform logs to a workspace" → diagnostic setting. Storage = archive; event hub = SIEM.
  - "Who is notified when it fires" → action group.
  - "Silence alerts during maintenance" → alert processing rule.
  - "Guest OS performance of a VM" → VM insights + Azure Monitor Agent.
  - "Ongoing latency/reachability between endpoints" → Connection Monitor.

📖 Full lesson: [Azure Monitor: Metrics, Logs, Diagnostic Settings & Alerts (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-monitor-metrics-logs-alerts/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Implement backup and recovery

- **Two vault types:**

| | Recovery Services vault | Azure Backup vault |
|---|---|---|
| Age | Original | Newer platform, still gaining datasources |
| Protects | Azure VMs, Azure Files, SQL Server and SAP HANA inside a VM, on-premises servers | Azure Database for PostgreSQL, Azure Blobs, Azure managed disks |
| Site Recovery | Yes, ASR uses this vault | No |

- Both vault types are managed from **Backup center**.
- **Vault storage redundancy** (locally redundant, zone-redundant, geo-redundant) is chosen **before the first backup**. Geo-redundant storage keeps a copy in the paired region.
- **Backup policy = schedule + retention.** Attach one policy to many items for consistency.
  - *Schedule*: frequency and time. Azure VMs back up daily, or several times a day with the **enhanced** policy.
  - *Retention*: how long each recovery point is kept, usually tiered (daily, then weekly, then monthly/yearly). More retention means more stored points and more cost.
- **What Azure Backup covers:**
  - *Azure VMs*: disk snapshots on the policy schedule via the backup extension.
  - *Azure Files*: snapshot-based, so a share can be reverted to a previous state.
  - *SQL Server / SAP HANA in an Azure VM*.
  - *On-premises files, folders, and system state*: via the **MARS agent** (Microsoft Azure Recovery Services) on the machine.
- **Restore options for a VM.** Match the restore to the blast radius:

| What broke | Restore |
|---|---|
| One deleted file or folder | File-level recovery: mount the recovery point as a drive and copy back |
| Disks only | Restore or replace disks on an existing VM, or create disks to attach |
| Whole VM lost, corrupted, or encrypted by ransomware | Restore a new VM from a point before the incident |

- Restores target the same region. If the vault is geo-redundant **with cross-region restore enabled**, you can restore to the paired region.
- **Azure Site Recovery (ASR)** continuously replicates VM disks to a **secondary region**. It keeps the workload running when a region or site is down.
  - Configure a target region, resource group, and VNet, plus a **replication policy** (app-consistent snapshot frequency, recovery point retention).
  - Sources: Azure VMs to another Azure region, and on-premises VMware and Hyper-V machines into Azure.
  - **RPO** = how much data you can lose (driven by replication/backup frequency). **RTO** = the deadline for being back online (driven by how quickly failover completes). Continuous replication keeps both low.
- **Failover types:**

| Type | When | Effect |
|---|---|---|
| Test | Validating the DR plan | Brings VMs up in an **isolated** network. Production and replication keep running. Clean up afterward. |
| Planned | Outage known in advance | Source shut down cleanly first, **no data loss** |
| Unplanned | Sudden outage | Recover from the latest available recovery point, accepting a small loss |

- **Failback** returns the workload to the original region after the incident.
- **Backup vs Site Recovery.** They complement each other rather than being alternatives:
  - Backup = get *data* back from a point in time (deletion, corruption, ransomware). It is measured by retention.
  - Site Recovery = keep the *service* running elsewhere (region/site outage). It is measured by RPO and RTO.
  - A production app that must survive both a mistaken deletion and a regional outage needs **both**.
- **Monitoring backups:**
  - *Backup center*: every protected item across vaults, on-demand backup and restore, and job status (succeeded, failed, why).
  - *Backup alerts*: failed backup or restore jobs, or a machine that stopped backing up. Route them through **Azure Monitor action groups** to email or page the team.
  - *Backup reports*: built on a **Log Analytics workspace**. They trend usage, jobs, and policy compliance, which helps with audits and storage planning.
- **Cues:**
  - "Recover from accidental deletion / point-in-time restore" → Azure Backup.
  - "Keep running in another region during an outage" → Azure Site Recovery.
  - "Test failover" → always Site Recovery.
  - "Back up an on-premises file server directly to a vault" → MARS agent.
  - "Back up blobs, managed disks, or PostgreSQL" → Azure Backup vault.
  - "Get notified when a backup job fails" → backup alert + action group.

📖 Full lesson: [Azure Backup & Site Recovery: Vaults, Policies & Failover (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-backup-site-recovery/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

[← Back to the study guide](../README.md)
