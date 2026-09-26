# AZ-104 Glossary

198 AZ-104 terms and services, each in one plain-English sentence.

[A](#a) · [B](#b) · [C](#c) · [D](#d) · [E](#e) · [F](#f) · [G](#g) · [H](#h) · [I](#i) · [K](#k) · [L](#l) · [M](#m) · [N](#n) · [O](#o) · [P](#p) · [R](#r) · [S](#s) · [T](#t) · [U](#u) · [V](#v) · [Z](#z)

## A

- **Access key**: One of two account-wide secrets that grant full control over everything in a storage account.
- **Access restrictions**: IP-based allow and deny rules that control inbound traffic to an App Service app.
- **Access tier**: A Blob Storage setting (hot, cool, cold, or archive) that trades storage cost against access cost and availability.
- **Account SAS**: A key-signed SAS that can cover several storage services and account-level operations.
- **Action group**: A reusable set of notifications and automated actions that alert rules trigger when they fire.
- **Address space**: The set of private CIDR blocks assigned to a VNet, from which all of its subnets are carved.
- **Aggregation**: The function (average, minimum, maximum, sum, or count) applied to metric values over a time grain.
- **Alert processing rule**: A rule that suppresses fired alerts or routes them to different action groups across many alert rules at once.
- **Alert rule**: A definition that watches a metric or log-query signal and fires when its condition is met.
- **Alert severity**: A level from 0 (critical) to 4 (verbose) assigned to an alert rule.
- **Allow forwarded traffic**: A peering option that accepts traffic an appliance relays on behalf of another network.
- **App Service Environment**: The single-tenant hosting option used by the Isolated tier for network isolation in your virtual network.
- **App Service managed certificate**: A free TLS certificate that App Service creates and renews for a custom domain.
- **App Service plan**: The compute resources, defined by OS, region, and pricing tier, that host App Service apps and are what you pay for.
- **Append blob**: A blob type optimized for adding data to the end, such as logs.
- **Application security group (ASG)**: A role-based grouping of VM network interfaces that NSG rules can reference instead of IP addresses.
- **ARM template**: A JSON file declaring the parameters, variables, resources, and outputs of a deployment.
- **Assigned membership**: Group membership that an administrator maintains by adding and removing each member manually.
- **Autoregistration**: A virtual network link option that makes Azure maintain an A record for each VM in the linked VNet.
- **Autoscale**: Rules that change the instance count automatically based on a metric or schedule.
- **Availability set**: A grouping that spreads VMs across fault and update domains within one datacenter.
- **Availability zone**: A physically separate datacenter within a region with independent power, cooling, and networking.
- **AzCopy**: A command-line utility for high-performance, scriptable data transfers to, from, and between storage accounts.
- **Azure App Service**: A managed platform for hosting web apps, APIs, and mobile back ends without managing servers.
- **Azure Backup vault**: The newer vault type for datasources such as Azure Blobs, Azure managed disks, and Azure Database for PostgreSQL.
- **Azure Bastion**: A managed service that provides browser-based RDP and SSH to VMs over TLS without giving them public IPs.
- **Azure Container Apps**: A serverless container platform that scales apps on rules and supports scale-to-zero, ingress, and revisions.
- **Azure Container Instances (ACI)**: A serverless service that runs containers with a fixed CPU and memory size and bills per second.
- **Azure Container Registry (ACR)**: A private, managed registry that stores container images for Azure services to pull.
- **Azure Disk Encryption**: In-guest volume encryption using BitLocker on Windows or dm-crypt on Linux.
- **Azure DNS**: A service that hosts DNS records for domains you already own on Microsoft's name servers.
- **Azure File Sync**: A service that synchronizes an Azure file share with on-premises Windows Servers.
- **Azure Files**: A managed service offering SMB and NFS file shares that clients mount like a network drive.
- **Azure Load Balancer**: A regional Layer 4 service that distributes TCP and UDP flows across a backend pool.
- **Azure Monitor**: The Azure service that gathers telemetry from resources, guest operating systems, and applications so you can chart, query, and alert on it.
- **Azure Monitor Agent**: The agent that collects guest operating system data from VMs into a Log Analytics workspace.
- **Azure Monitor Logs**: The Azure Monitor data platform that stores structured event records for querying.
- **Azure Policy**: A service that evaluates resource configuration against rules and can report, block, or correct non-compliant resources.
- **Azure Private Link**: The technology that exposes a PaaS service on a private IP inside your virtual network.
- **Azure Resource Manager (ARM)**: The Azure deployment and management layer that processes every create, update, and delete request.
- **Azure Resource Mover**: The service for moving resources such as VMs to another Azure region.
- **Azure role-based access control (Azure RBAC)**: The authorization system that controls what identities can do to Azure resources.
- **Azure Site Recovery (ASR)**: A disaster recovery service that continuously replicates VMs to a secondary location for failover.
- **Azure Storage Explorer**: A free desktop application for browsing and managing storage accounts through a graphical interface.
- **AzureBastionSubnet**: The dedicated subnet name, at least /26, that Azure Bastion must be deployed into.

## B

- **Backup center**: A single management view of backups across Recovery Services vaults and Backup vaults.
- **Backup policy**: The pairing of a backup schedule with retention rules that set how long each recovery point survives.
- **Bicep**: A concise declarative language for Azure deployments that transpiles to an ARM template.
- **Bicep decompile**: The command that converts an ARM JSON template into a Bicep file.
- **Blob soft delete**: A setting that keeps deleted blobs for a retention period so they can be restored.
- **Blob versioning**: A setting that automatically keeps the previous state of a blob each time it is overwritten.
- **Block blob**: The blob type used for most files and streaming content.
- **Boot diagnostics**: A VM feature that captures serial console output and a boot screenshot to diagnose startup failures.
- **Budget**: A Microsoft Cost Management spending threshold that sends alerts when actual or forecast costs cross set percentages.
- **Bulk operations**: CSV-driven jobs in the Users blade that create, invite, or delete many accounts at once.

## C

- **Cloud tiering**: An Azure File Sync feature that keeps frequently used files on the local server and replaces cold files with pointers to the cloud copy.
- **Complete mode**: A deployment mode that deletes resources in the resource group that the template does not define.
- **Connection Monitor**: A Network Watcher feature that measures reachability, latency, and packet loss between endpoints on a schedule and keeps the history.
- **Container**: A grouping of blobs in Blob Storage with its own public access level.
- **Container Apps environment**: The shared boundary that container apps run inside, with common logging and optional virtual network integration.
- **Container group**: A set of ACI containers on one host sharing a lifecycle, IP address, and storage volumes.
- **Contributor**: A built-in role that can create and manage resources but cannot grant access.
- **Cross-region restore**: A geo-redundant vault option that allows restoring backup data into the paired region.
- **Custom role**: A role you define in JSON with Actions, NotActions, DataActions, NotDataActions, and AssignableScopes when no built-in role fits.
- **Customer-managed key**: An encryption key you hold in Azure Key Vault or Managed HSM to control rotation and revocation of storage encryption.

## D

- **Deallocate**: Stopping a VM so it releases its host hardware, required before some resizes.
- **Deny assignment**: An Azure-created block on specific actions that overrides any role assignment allowing them.
- **Deployment slot**: A live copy of an app in the same plan, with its own hostname, used to test before swapping into production.
- **Diagnostic setting**: A per-resource configuration that sends chosen log categories and metrics to a workspace, storage account, or event hub.
- **DNS delegation**: Pointing a domain's NS records at the registrar to the name servers Azure assigns to a zone.
- **Dynamic membership**: Group membership calculated from a rule on user or device attributes, requiring Microsoft Entra ID P1.

## E

- **Effective routes**: The combined list of system routes, UDRs, and peering routes applied to a network interface.
- **Effective security rules**: The merged view of subnet and NIC NSG rules that actually apply to a network interface.
- **Encryption at host**: Encryption performed on the VM's physical host that covers the temporary disk and disk caches.
- **Event hub**: A diagnostic setting destination used to stream monitoring data to a third-party SIEM or external tool.
- **External collaboration settings**: Tenant settings that control who can invite guests, whether guests can invite others, and which domains are allowed or blocked.

## F

- **Failback**: Returning a failed-over workload to its original region after the incident ends.
- **Fault domain**: A group of hardware sharing power and network, so a single failure affects only the VMs in it.
- **File share quota**: The maximum size, in GiB, that an Azure file share can grow to.
- **File-level recovery**: A restore that mounts a recovery point as a drive so individual files can be copied back.

## G

- **Gateway transit**: A peering option that lets a spoke VNet reach on-premises networks through the hub VNet's VPN or ExpressRoute gateway.
- **GatewaySubnet**: The dedicated subnet name required for a VPN or ExpressRoute gateway.
- **General-purpose v2 (GPv2)**: The default storage account kind, supporting every storage service and access tier.
- **Geo-redundant storage (GRS)**: LRS in the primary region plus asynchronous replication to a paired secondary region.
- **Geo-replication (ACR)**: A Premium-tier registry feature that keeps one registry synchronized across several regions.
- **Geo-zone-redundant storage (GZRS)**: ZRS in the primary region plus asynchronous replication to a paired secondary region.
- **Global peering**: Peering between VNets that sit in different Azure regions.
- **Group-based licensing**: Assigning product licenses to a group so that every member inherits them automatically.
- **Guest user**: An external person invited through B2B collaboration who signs in with credentials from their own home organization.

## H

- **Health probe**: A periodic check on a port or HTTP path that decides whether a backend instance receives new connections.

## I

- **Idempotent deployment**: A deployment that produces the same result however many times it runs.
- **Identity-based access (Azure Files)**: Authentication to SMB file shares with AD DS, Microsoft Entra Domain Services, or Microsoft Entra Kerberos instead of the account key.
- **Inbound NAT rule**: A load balancer rule that forwards one frontend port to a single specific backend VM.
- **Incremental mode**: The default deployment mode, which adds or updates template resources and leaves other resources in the group alone.
- **Infrastructure as code**: Defining Azure resources in text files that Resource Manager deploys the same way every time.
- **Infrastructure encryption**: A second layer of encryption with a separate key, enabled only when the storage account is created.
- **Initiative**: A set of policy definitions grouped so they are assigned and tracked as one unit.
- **Internal load balancer**: A load balancer with a private IP frontend that distributes traffic originating inside the network.
- **IP flow verify**: A Network Watcher tool that checks whether a specific flow is allowed and names the rule that blocks it.
- **IP forwarding**: A NIC setting that lets an appliance accept and pass on packets not addressed to itself.

## K

- **Kusto Query Language (KQL)**: The read-only query language used to filter, aggregate, and shape log data in Azure Monitor.

## L

- **Last Sync Time**: A property showing how current the data in the geo-replicated secondary region is.
- **Lifecycle management policy**: Rules that move blobs to cooler tiers or delete them based on their age.
- **Locally redundant storage (LRS)**: Redundancy that keeps three copies of data within a single datacenter.
- **Log alert**: An alert rule whose condition is based on the results of a KQL query.
- **Log Analytics workspace**: The container that stores log data and sets its access control, retention, and pricing.

## M

- **Managed disk**: A VM disk whose underlying storage and replication Azure manages for you.
- **Managed identity**: An identity that Azure creates and manages for a resource so it can authenticate without stored credentials.
- **Management group**: A container above subscriptions for applying policy and access to many subscriptions at once.
- **MARS agent**: The Microsoft Azure Recovery Services agent that backs up on-premises files, folders, and system state to a vault.
- **Member user**: An internal account belonging to your organization, with credentials managed in your tenant.
- **Metric**: A numeric value sampled at regular intervals and stored as a time series, available in near real time.
- **Metrics Explorer**: The Azure Monitor tool for charting metrics with aggregations, filters, and splitting by dimension.
- **Microsoft 365 group**: An Entra group for collaboration that provisions a shared mailbox, calendar, SharePoint site, and Teams workspace for its user members.
- **Microsoft Entra B2B collaboration**: The invitation-based feature that lets outside users access your apps and resources as guests.
- **Microsoft Entra Connect**: The tool that synchronizes identities between on-premises Active Directory and Microsoft Entra ID.
- **Microsoft Entra ID**: Microsoft's cloud identity and access management service, formerly called Azure Active Directory, that holds the users and groups signing in to Azure and Microsoft 365.
- **Microsoft Entra roles**: Directory roles, such as Global Administrator, that control management of Entra ID objects rather than Azure resources.

## N

- **Network insights**: An Azure Monitor view of topology and health across network resources.
- **Network security group (NSG)**: A set of priority-ordered allow and deny rules filtering traffic at a subnet or NIC.
- **Network virtual appliance (NVA)**: A VM, such as a third-party firewall, that inspects or forwards traffic for other resources.
- **Network Watcher**: A regional Azure service of diagnostic tools for troubleshooting connectivity, routing, and security rules.
- **Next hop**: A Network Watcher tool that shows where Azure will route a packet for a given destination.
- **Next hop type**: The part of a route that says where matching traffic goes, such as Virtual appliance, Internet, or None.
- **Non-transitive peering**: The rule that two peerings through a shared VNet do not connect the outer VNets to each other.
- **NSG flow logs**: A Network Watcher feature that records traffic a network security group allowed or denied.
- **NSG priority**: A number from 100 to 4096 where the lowest number is evaluated first and the first match ends processing.

## O

- **Object replication**: Asynchronous copying of block blobs between two storage accounts according to container-level rules.
- **Owner**: A built-in role with full access to resources plus the right to assign roles to others.

## P

- **Packet capture**: A Network Watcher tool that records actual network packets on a VM for inspection.
- **Page blob**: A blob type used for random read and write access that backs Azure VM disks.
- **Parameter**: A template value supplied at deployment time so one file can serve several environments.
- **Password writeback**: The SSPR option that sends a password reset in the cloud back to on-premises Active Directory for synced accounts.
- **Planned failover**: A failover after a clean source shutdown, used for a known outage and avoiding data loss.
- **Platform logs**: Resource-emitted diagnostic records that are not kept anywhere queryable until a diagnostic setting routes them.
- **Policy effect**: The outcome an assignment applies to a non-compliant resource, such as Audit, Deny, Modify, or DeployIfNotExists.
- **Premium performance**: An SSD-backed storage account option chosen at creation for low latency and high transaction rates.
- **Private DNS zone**: A DNS zone that resolves names only for the VNets linked to it.
- **Private endpoint**: A network interface that gives a storage account a private IP address inside your virtual network.
- **Public DNS zone**: An Azure DNS zone that answers internet queries for a domain once the registrar delegates to it.
- **Public IP address**: A standalone Azure resource that gives a NIC, load balancer, gateway, Bastion host, or NAT gateway an internet-routable address.

## R

- **Read-access geo-redundant storage (RA-GRS / RA-GZRS)**: Geo-redundant options that also let applications read from the secondary region at any time.
- **Reader**: A built-in role that can view resources but not change them.
- **Recovery point**: A point-in-time copy of protected data that a restore starts from.
- **Recovery Point Objective (RPO)**: The maximum amount of data, measured in time, that a workload can afford to lose.
- **Recovery Services vault**: The original vault for Azure VM, Azure Files, in-VM SQL and SAP HANA, and on-premises backups, also used by Site Recovery.
- **Recovery Time Objective (RTO)**: The maximum time allowed to get a workload running again after an incident.
- **Rehydration**: Moving an archived blob back to an online tier so it can be read, at Standard or High priority.
- **Remediation task**: A job that brings existing resources into compliance using a Modify or DeployIfNotExists policy and a managed identity.
- **Replication policy**: The Site Recovery setting for app-consistent snapshot frequency and recovery point retention.
- **Resource group**: A logical container for related resources that share a lifecycle, permissions, and policies.
- **Resource lock**: A CanNotDelete or ReadOnly setting that blocks deletion or changes regardless of the caller's RBAC rights.
- **Restart policy**: The ACI setting (Always, OnFailure, or Never) that decides whether a container restarts after it exits.
- **Retention**: How long each recovery point is kept, often tiered by daily, weekly, monthly, and yearly points.
- **Revision**: An immutable version of a container app that can receive a share of traffic.
- **Role assignment**: The binding of a security principal to a role definition at a specific scope.
- **Role definition**: A named set of permitted and excluded actions, such as Reader or Contributor.
- **Root management group**: The single top-level management group in each tenant that every other management group descends from.

## S

- **Scale rule**: A Container Apps trigger based on HTTP, TCP, CPU, memory, or an event source that adds or removes replicas.
- **Scope**: The level where a role, policy, or lock applies: management group, subscription, resource group, or resource.
- **Security group**: An Entra group used to grant roles, app access, and licenses, which can contain users, devices, and other groups.
- **Security principal**: The identity a role is assigned to: a user, group, service principal, or managed identity.
- **Self-service password reset (SSPR)**: A feature that lets users reset or unlock their own passwords after verifying with registered authentication methods.
- **Service endpoint**: A subnet setting that lets a virtual network rule admit that subnet's traffic to a storage account over the Azure backbone.
- **Service principal**: The identity an application uses to access Azure resources.
- **Service SAS**: A key-signed SAS limited to resources within a single storage service.
- **Service tag**: A named group of IP prefixes, such as Internet, VirtualNetwork, or AzureLoadBalancer, used in place of addresses in NSG rules.
- **Share snapshot**: A read-only, incremental, point-in-time copy of an entire Azure file share.
- **Shared access signature (SAS)**: A signed token appended to a URL that grants limited permissions on specific resources for a set time.
- **Slot setting**: An app setting or connection string marked to stay with its deployment slot during a swap.
- **Standard SKU public IP**: A statically allocated, zone-capable public IP that blocks inbound traffic until an NSG allows it.
- **Storage account**: The top-level Azure resource that provides a unique namespace for blobs, files, queues, and tables.
- **Storage firewall**: The storage account's network rules that limit access to selected virtual networks and public IP ranges, or disable public access.
- **Storage insights**: An Azure Monitor view of storage account capacity, transactions, availability, and latency.
- **Storage Service Encryption (SSE)**: Always-on 256-bit AES encryption of data at rest in Azure Storage.
- **Stored access policy**: A named policy on a container, share, queue, or table that holds a service SAS's permissions and expiry so it can be revoked or changed later.
- **Subnet**: A non-overlapping range within a VNet's address space, in which Azure reserves five addresses.
- **Subscription**: A billing boundary and access scope that contains resource groups.
- **Sync group**: The Azure File Sync object that links one cloud endpoint with one or more server endpoints.
- **System route**: A default route Azure creates automatically for a subnet, which you can override but not delete.

## T

- **Tag**: A name-value pair attached to a resource, resource group, or subscription for organizing and cost reporting, not inherited by default.
- **Template spec**: An Azure resource that stores a template so a team can share and version it.
- **Test failover**: A Site Recovery drill that starts replicated VMs in an isolated network without affecting production or replication.

## U

- **Ultra Disk**: The highest-performance managed disk type, for demanding databases that need sub-millisecond latency.
- **Unplanned failover**: A failover from the latest available recovery point after a sudden outage.
- **Update domain**: A group of VMs that Azure restarts together during planned maintenance.
- **Usage location**: A user property that must be set before licenses can be assigned to that user.
- **User Access Administrator**: A built-in role that manages role assignments without managing the resources themselves.
- **User delegation SAS**: A Blob storage SAS signed with a key issued to a Microsoft Entra identity instead of an account key.
- **User-defined route (UDR)**: A custom route in a route table, associated with a subnet, that overrides Azure's default routing.

## V

- **Virtual Machine Scale Set**: A group of identical, load-balanced VMs that can scale automatically.
- **Virtual network (VNet)**: A private, isolated network in one Azure region, defined by one or more CIDR address ranges.
- **Virtual network link**: The connection that lets a VNet resolve, and optionally autoregister into, a private DNS zone.
- **Virtual network peering**: A link that lets two VNets communicate over private IPs across the Microsoft backbone without a gateway.
- **VM insights**: An Azure Monitor experience for VM guest performance, health, and process dependencies.
- **VM size**: The combination of vCPUs, memory, and related capabilities assigned to a virtual machine.
- **VNet integration**: An App Service feature that lets an app make outbound calls to resources in a virtual network.

## Z

- **Zone-redundant storage (ZRS)**: Redundancy that keeps three copies spread across three availability zones in one region.

[← Back to the study guide](README.md)
