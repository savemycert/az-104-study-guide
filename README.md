# AZ-104 Study Guide: Microsoft Certified: Azure Administrator Associate

A free, open study guide for the **Microsoft Certified: Azure Administrator Associate (AZ-104)** exam. It covers every domain and topic in the official exam guide as a checklist, lists the facts worth memorizing, and links each topic to a full free lesson.

Maintained by [SaveMyCert](https://www.savemycert.com/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide), where you can read every lesson free, [practice with explained questions](https://www.savemycert.com/practice/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide) and [take timed mock exams](https://www.savemycert.com/mocks/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide).

## Contents

- [Exam at a glance](#exam-at-a-glance)
- [Exam domains](#exam-domains)
- [Syllabus checklist](#syllabus-checklist)
  - [Domain 1: Manage Azure identities and governance](#domain-1-manage-azure-identities-and-governance)
  - [Domain 2: Implement and manage storage](#domain-2-implement-and-manage-storage)
  - [Domain 3: Deploy and manage Azure compute resources](#domain-3-deploy-and-manage-azure-compute-resources)
  - [Domain 4: Implement and manage virtual networking](#domain-4-implement-and-manage-virtual-networking)
  - [Domain 5: Monitor and maintain Azure resources](#domain-5-monitor-and-maintain-azure-resources)
- [How to study for AZ-104](#how-to-study-for-az-104)
- [Sample questions](sample-questions.md)
- [Free resources](#free-resources)

## Exam at a glance

| | |
|---|---|
| Exam code | AZ-104 |
| Level | Associate |
| Questions | 40–60 |
| Time limit | 100 min |
| Passing score | 700 / 1000 |
| Format | Multiple choice & more |
| Exam fee | $165 |
| Valid for | 1 year (free renewal) |

Exam details change. Always confirm them in the official [Microsoft AZ-104 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-104) from Microsoft Learn.

## Exam domains

| # | Domain | Weight | Topics |
|---|---|---|---|
| 1 | [Manage Azure identities and governance](#domain-1-manage-azure-identities-and-governance) | 24% | 3 |
| 2 | [Implement and manage storage](#domain-2-implement-and-manage-storage) | 19% | 3 |
| 3 | [Deploy and manage Azure compute resources](#domain-3-deploy-and-manage-azure-compute-resources) | 24% | 4 |
| 4 | [Implement and manage virtual networking](#domain-4-implement-and-manage-virtual-networking) | 19% | 3 |
| 5 | [Monitor and maintain Azure resources](#domain-5-monitor-and-maintain-azure-resources) | 14% | 2 |

That is 5 domains and 15 topics. Spend your time in proportion to the weights: the heaviest domain decides more of your score than the lightest.

## Syllabus checklist

Tick each topic off once you can explain it without notes. The "Must know" facts are the ones questions turn on. Each lesson link goes to the complete, free lesson.

### Domain 1: Manage Azure identities and governance

**Weight: 24%.** Microsoft Entra users and groups, access to Azure resources, and subscription-level governance. Official weighting 20–25%.

- [ ] **Manage Microsoft Entra users and groups**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating users and groups; managing user and group properties; managing licenses in Microsoft Entra ID; managing external users; configuring self-service password reset (SSPR).
  - 📖 Lesson: [Manage Microsoft Entra Users and Groups (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/manage-entra-users-groups/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Microsoft Entra ID (formerly Azure Active Directory) is Azure's cloud identity service holding all users and groups.
  - Must know: Member users are internal accounts; guest users are external collaborators invited through B2B who sign in with their own home credentials.
- [ ] **Manage access to Azure resources**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Managing built-in Azure roles; assigning roles at different scopes; interpreting access assignments.
  - 📖 Lesson: [Manage Access to Azure Resources: Azure RBAC (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-rbac-role-assignments/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Azure RBAC is the authorization system that controls access to Azure resources; Microsoft Entra ID handles authentication.
  - Must know: Every role assignment has three parts: a security principal (who), a role definition (what), and a scope (where).
- [ ] **Manage Azure subscriptions and governance**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Implementing and managing Azure Policy; configuring resource locks; applying and managing tags on resources; managing resource groups and subscriptions; managing costs by using alerts, budgets, and Azure Advisor recommendations; configuring management groups.
  - 📖 Lesson: [Azure Subscriptions, Policy, Locks and Governance (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-subscriptions-governance-policy/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: The Azure hierarchy is management group → subscription → resource group → resource, and governance applied at a higher scope is inherited by everything beneath it.
  - Must know: Management groups let you assign policy and access to many subscriptions at once; moving a subscription to a new management group makes it inherit that group's governance.

### Domain 2: Implement and manage storage

**Weight: 19%.** Storage access control, storage account configuration, and Azure Files and Blob Storage. Official weighting 15–20%.

- [ ] **Configure access to storage**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Configuring Azure Storage firewalls and virtual networks; creating and using shared access signature (SAS) tokens; configuring stored access policies; managing access keys; configuring identity-based access for Azure Files.
  - 📖 Lesson: [Configure Access to Azure Storage: SAS, Keys & Firewalls (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-access-sas/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Storage access has two layers: networking (firewall, virtual network rules, private endpoints) and authorization (keys, SAS, Entra RBAC) — decide them separately.
  - Must know: Storage firewalls restrict access to selected virtual network subnets via service endpoints and to public IP ranges; private endpoints remove public exposure entirely.
- [ ] **Configure and manage storage accounts**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating and configuring storage accounts; configuring Azure Storage redundancy; configuring object replication; configuring storage account encryption; managing data by using Azure Storage Explorer and AzCopy.
  - 📖 Lesson: [Configure & Manage Azure Storage Accounts and Redundancy (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-accounts-redundancy/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: General-purpose v2 (GPv2) is the default account kind, supporting all services and access tiers; choose Standard performance for most workloads and Premium for low-latency, high-transaction needs.
  - Must know: Every redundancy option keeps at least three copies: LRS stays in one datacenter, ZRS spans three availability zones, and GRS and GZRS add a second region.
- [ ] **Configure Azure Files and Azure Blob Storage**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating and configuring a file share in Azure Files and a container in Azure Blob Storage; configuring storage tiers; configuring soft delete for blobs and containers; configuring snapshots and soft delete for Azure Files; configuring blob lifecycle management and blob versioning.
  - 📖 Lesson: [Configure Azure Files and Azure Blob Storage (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-files-blob-storage/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Azure Files provides managed SMB and NFS file shares you mount like a network drive; Azure Blob Storage provides containers of block, append, and page blobs for object data.
  - Must know: Create a file share with a protocol and quota, mount SMB over port 445, and use a premium FileStorage account for NFS or low-latency workloads.

### Domain 3: Deploy and manage Azure compute resources

**Weight: 24%.** ARM template and Bicep deployments, virtual machines, containers, and Azure App Service. Official weighting 20–25%.

- [ ] **Automate deployment of resources by using Azure Resource Manager (ARM) templates or Bicep files**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Interpreting an ARM template or a Bicep file; modifying an existing ARM template or Bicep file; deploying resources by using an ARM template or a Bicep file; exporting a deployment as an ARM template or converting an ARM template to a Bicep file.
  - 📖 Lesson: [ARM Templates and Bicep: AZ-104 Deployment Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-arm-templates-bicep/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: An ARM template is JSON with parameters, variables, resources, and outputs; parameters are supplied at deployment, outputs are returned after it.
  - Must know: Bicep is a cleaner declarative language that transpiles to ARM JSON, infers dependencies from symbolic references, and can do anything ARM JSON can.
- [ ] **Create and configure virtual machines**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating a virtual machine; configuring encryption at host; moving a VM to another resource group, subscription, or region; managing VM sizes and disks; deploying VMs to availability zones and availability sets; deploying and configuring Azure Virtual Machine Scale Sets.
  - 📖 Lesson: [Azure Virtual Machines: Sizes, Disks, and Zones (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-machines-configuration/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Creating a VM requires five choices: image, size, admin credentials, resource group, and region; the VM attaches to a network interface (NIC).
  - Must know: VM size families map to workloads — B and D are general purpose, F is compute optimized, and E is memory optimized; resizing to a size on different hardware requires stopping (deallocating) the VM.
- [ ] **Provision and manage containers in the Azure portal**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating and managing an Azure Container Registry; provisioning containers by using Azure Container Instances and Azure Container Apps; managing sizing and scaling for containers, including Container Instances and Container Apps.
  - 📖 Lesson: [Provision and Manage Containers in Azure (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-containers-aci-container-apps/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Azure Container Registry (ACR) is a private, managed registry that stores container images; ACI, Container Apps, and AKS pull from it.
  - Must know: ACR SKUs are Basic, Standard, and Premium; geo-replication and private endpoints are Premium-only, and a managed identity lets services pull images without stored credentials.
- [ ] **Create and configure Azure App Service**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Provisioning an App Service plan and configuring its scaling; creating an App Service; configuring certificates and Transport Layer Security (TLS); mapping an existing custom DNS name; configuring backup, networking settings, and deployment slots for an App Service.
  - 📖 Lesson: [Create and Configure Azure App Service (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-app-service-configuration/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Azure App Service is a managed PaaS for web apps and APIs; every app runs on an App Service plan, which is the compute you pay for.
  - Must know: The plan's pricing tier (Free, Shared, Basic, Standard, Premium, Isolated) decides compute, whether apps share hardware, and which features are available.

### Domain 4: Implement and manage virtual networking

**Weight: 19%.** Virtual networks and subnets, secure network access, name resolution, and load balancing. Official weighting 15–20%.

- [ ] **Configure and manage virtual networks in Azure**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating and configuring virtual networks and subnets; creating and configuring virtual network peering; configuring public IP addresses; configuring user-defined routes; troubleshooting network connectivity.
  - 📖 Lesson: [Azure Virtual Networks, Subnets, Peering & User-Defined Routes](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-networks-peering/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Azure reserves five IP addresses in every subnet (network, gateway, two for DNS, broadcast), so a /24 yields 251 usable hosts.
  - Must know: VNet peering connects two VNets over the Microsoft backbone; regional peering is same-region and global peering spans regions.
- [ ] **Configure secure access to virtual networks**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating and configuring network security groups (NSGs) and application security groups; evaluating effective security rules in NSGs; implementing Azure Bastion; configuring service endpoints and private endpoints for Azure platform as a service (PaaS).
  - 📖 Lesson: [Azure NSGs, Bastion, Service Endpoints & Private Endpoints](https://www.savemycert.com/revision/azure-administrator-associate/azure-nsg-bastion-private-endpoints/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: NSG rules use priority 100-4096 where a lower number means higher priority, and Azure stops at the first matching rule.
  - Must know: Every NSG rule is a 5-tuple: source, source port, destination, destination port, and protocol, with an allow or deny action.
- [ ] **Configure name resolution and load balancing**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Configuring Azure DNS; configuring an internal or public load balancer; troubleshooting load balancing.
  - 📖 Lesson: [Azure DNS and Load Balancer: AZ-104 Networking Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-dns-load-balancer/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: A public Azure DNS zone hosts a domain's records (A, CNAME, MX, TXT) and becomes authoritative once you delegate the domain by updating the name servers at the registrar.
  - Must know: A private DNS zone resolves VM names privately inside and between VNets via a virtual network link; enable autoregistration to have Azure create and remove VM A records automatically.

### Domain 5: Monitor and maintain Azure resources

**Weight: 14%.** Azure Monitor metrics, logs, and alerting, plus backup and disaster recovery. Official weighting 10–15%.

- [ ] **Monitor resources in Azure**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Interpreting metrics in Azure Monitor; configuring log settings; querying and analyzing logs; setting up alert rules, action groups, and alert processing rules; configuring and interpreting monitoring of virtual machines, storage accounts, and networks by using Azure Monitor Insights; using Azure Network Watcher and Connection monitor.
  - 📖 Lesson: [Azure Monitor: Metrics, Logs, Diagnostic Settings & Alerts (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-monitor-metrics-logs-alerts/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: Azure Monitor collects two fundamental data types: metrics (numeric, time-series, near real time) and logs (structured events queried with KQL).
  - Must know: Interpret metrics in Metrics Explorer using aggregations, filters, and splitting; metrics are collected automatically and need no query language.
- [ ] **Implement backup and recovery**
  <br>Skills outline section (AZ-104, as of April 17, 2026). Creating a Recovery Services vault and an Azure Backup vault; creating and configuring a backup policy; performing backup and restore operations by using Azure Backup; configuring Azure Site Recovery for Azure resources; performing a failover to a secondary region; configuring and interpreting reports and alerts for backups.
  - 📖 Lesson: [Azure Backup & Site Recovery: Vaults, Policies & Failover (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-backup-site-recovery/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
  - Must know: A Recovery Services vault holds Azure Backup for VMs, Azure Files, and in-VM SQL/SAP, and is the vault Azure Site Recovery uses; the newer Azure Backup vault covers modern workloads like Azure databases, blobs, and managed disks.
  - Must know: A backup policy is a schedule (how often backups run) plus retention (how long each recovery point is kept).

## How to study for AZ-104

1. **Read the lesson for each topic** in the checklist above, starting with the heaviest domain. Every lesson is free on the [AZ-104 revision notes](https://www.savemycert.com/revision/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide).
2. **Practice straight after reading.** Answer [AZ-104 practice questions](https://www.savemycert.com/practice/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide) on the topic you just read. Each option comes with an explanation of why it is right or wrong.
3. **Review what you got wrong**, re-read that lesson section, and tick the topic off only when you get its questions right.
4. **Take a full-length [AZ-104 mock exam](https://www.savemycert.com/mocks/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)** under the real time limit. Aim to pass mocks comfortably before you book.
5. **On the last day**, skim the [AZ-104 cheat sheet](https://www.savemycert.com/cheat-sheet/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide) instead of starting anything new.

## Sample questions

[sample-questions.md](sample-questions.md) has 5 worked AZ-104 questions with the answer, why each option is right or wrong, and the reasoning steps.

## Free resources

- [Microsoft AZ-104 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-104): the official source (Microsoft Learn)
- [AZ-104 certification overview](https://www.savemycert.com/certifications/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)
- [AZ-104 revision notes](https://www.savemycert.com/revision/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide): every lesson, free to read
- [AZ-104 practice questions](https://www.savemycert.com/practice/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide): with an explanation on every option
- [AZ-104 mock exams](https://www.savemycert.com/mocks/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide): full-length and timed
- [AZ-104 cheat sheet](https://www.savemycert.com/cheat-sheet/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide): the key facts on one page
- [All certification study guides](https://github.com/savemycert-sketch/certification-study-guides)

## Contributing

Spotted an error or an out-of-date fact? [Open an issue](../../issues) with the topic and a link to the official source. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License and disclaimer

This guide is licensed under [CC BY 4.0](LICENSE). You can reuse and adapt it, including commercially, as long as you credit **SaveMyCert** with a link to https://www.savemycert.com/.

This is an independent study resource. It is not affiliated with or endorsed by Microsoft Learn. Microsoft Certified: Azure Administrator Associate and AZ-104 are trademarks of their respective owner. Exam domains and weights are taken from the official exam guide linked above.
