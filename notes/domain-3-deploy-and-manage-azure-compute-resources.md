# Domain 3: Deploy and manage Azure compute resources (24%)

Tied for the largest domain on AZ-104. It covers infrastructure as code, virtual machines, containers, and App Service, and most questions ask you to pick the right deployment mode, placement, service, or pricing tier for a stated requirement.

## Automate deployment of resources by using Azure Resource Manager (ARM) templates or Bicep files

- Both formats are **declarative** (you describe the end state, not the steps) and **idempotent** (redeploying the same file gives the same result, and Resource Manager changes only what differs).
- **ARM template (JSON) sections:**

| Section | Role |
|---|---|
| `$schema` | Template language version (required) |
| `contentVersion` | Your own version stamp (required) |
| `parameters` | Values supplied at deploy time (name, size, environment) |
| `variables` | Values computed or reused inside the template |
| `resources` | What to create or update: `type`, `apiVersion`, `name`, `location`, `properties` |
| `outputs` | Values returned after deployment (IP address, connection string) |

- Exam reading pattern: trace a value from parameter → variable → resource property → output.

- **Bicep:**
  - Concise syntax: `param`, `var`, `resource <symbolicName> '<type>@<apiVersion>'`.
  - Symbolic references (`nic.id`) let Bicep **infer dependencies**. JSON often needs manual `dependsOn`.
  - Type checking, IntelliSense, and reusable **modules**.
  - **Transpiles to ARM JSON** before deployment, so it is the same engine with the same capabilities. Microsoft recommends Bicep for new work, and ARM JSON remains supported.
- **Modifying templates:**
  - Change a parameter via `defaultValue`, a parameters file, `--parameters`, or the portal form.
  - Add a resource: a new `resources` entry (JSON, with `dependsOn` if needed) or a `resource` block (Bicep).
- **Deployment modes:**

| | Incremental (default) | Complete |
|---|---|---|
| Resources in the template | Created or updated | Created or updated |
| Resources in the group but not in the template | Left alone | **Deleted** |

- Trap: a Complete-mode redeploy of a web-app-only template deletes any other resource in that group, such as a storage account.
- **Deploying:**
  - Portal: **Deploy a custom template**.
  - CLI: `az deployment group create --resource-group <rg> --template-file main.bicep` (add `--mode Complete` to switch modes).
  - PowerShell: `New-AzResourceGroupDeployment`.
  - Scopes: resource group, subscription, management group, or tenant.
  - **Template specs** store a template as an Azure resource for shared, versioned reuse.
- **Export and convert:**
  - **Export template** from a resource group, or from a past deployment in **deployment history**. Expect hard-coded values to clean up.
  - `az bicep decompile --file template.json` → JSON to Bicep.
  - `az bicep build --file main.bicep` → Bicep to JSON.

📖 Full lesson: [ARM Templates and Bicep: AZ-104 Deployment Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-arm-templates-bicep/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Create and configure virtual machines

- **Every VM needs five choices:** image, size, admin credentials (password, or SSH key for Linux), resource group, region. Creation also makes an OS disk, a **NIC** attached to a subnet, and usually a public IP.
- **Size families:**

| Family | Category | Fits |
|---|---|---|
| B | Burstable general purpose | Dev/test, small sites with occasional spikes |
| D | General purpose | Most production web and app servers |
| F | Compute optimized (more CPU per GB) | Batch, CPU-bound web tiers |
| E | Memory optimized (more GB per CPU) | Databases, in-memory caches, analytics |

- **Resizing:** if the target size is available on the current hardware cluster, it applies to the running VM with a restart. If not, **stop (deallocate)**, resize, then start. Plan for a short outage.
- **Disks:**
  - OS disk boots the VM. The **temporary disk** is ephemeral, so never keep data there. **Data disks** hold application data. All are managed disks.

| Disk type | Fits |
|---|---|
| Standard HDD | Backups, archive, rarely accessed data (cheapest) |
| Standard SSD | Light production, web servers, dev/test |
| Premium SSD | Production databases, latency-sensitive apps |
| Ultra Disk | Most demanding databases: highest IOPS, sub-millisecond latency |

- Host caching: **ReadOnly** for read-heavy data disks, **None** for write-heavy logs. The OS disk defaults to **ReadWrite**.
- Data disks can be added to a running VM and **expanded**, never **shrunk**.
- **High availability placement:**
  - **Availability zones**: separate datacenters in a region. 2+ VMs across zones = **99.99%** SLA. Survives a datacenter loss.
  - **Availability sets**: **fault domains** (separate racks, power, network) + **update domains** (rebooted separately during maintenance) inside one datacenter. 2+ VMs = **99.95%** SLA.
  - Scenario: two web VMs that must survive a datacenter outage → one per zone behind a zone-redundant load balancer.
- **Virtual Machine Scale Sets:** many identical, load-balanced instances that **autoscale** on a metric (such as CPU) or a schedule, and can span zones. Suited to stateless web and app tiers.
- **Encryption:**
  - **Encryption at host** encrypts on the physical host, covering the **temp disk and disk caches**, with OS and data disk data flowing encrypted to storage. Enabled per VM after registering the feature on the subscription.
  - **Azure Disk Encryption** runs inside the guest OS (BitLocker on Windows, dm-crypt on Linux).
  - Storage-side encryption of managed disks is always on regardless.
- **Moving a VM:**
  - To another **resource group or subscription** → the built-in **Move** operation. Select the VM plus its disks, NIC, and public IP. The region does not change.
  - To another **region** → **Azure Resource Mover** (validate, prepare, copy, commit).

📖 Full lesson: [Azure Virtual Machines: Sizes, Disks, and Zones (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-machines-configuration/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Provision and manage containers in the Azure portal

- **ACR stores images, ACI runs one, Container Apps scales many.** AKS is recognition-level only.
- **Azure Container Registry (ACR):**
  - Private registry at `<name>.azurecr.io`. `az acr login --name <name>`, then `docker push` / `docker pull`.
  - SKUs: **Basic** (learning, small), **Standard** (most production), **Premium** (highest throughput, **geo-replication** and **private endpoints**, both Premium-only).
  - Auth: the **admin user** (single shared credential, off by default, testing only), **Microsoft Entra ID** with **AcrPush** / **AcrPull** roles, or a **managed identity** so ACI, Container Apps, or AKS pull with no stored secret.
- **Azure Container Instances (ACI):**
  - Pick an image, CPU cores, and memory (GB). Starts in seconds with no VM or orchestrator.
  - Billed **per second** for requested vCPU and memory while running.
  - **Restart policy:** Always, OnFailure, or Never. Use OnFailure or Never for run-to-completion jobs so billing stops when they finish.
  - Public IP + DNS name, or deploy into a VNet for private access.
  - **Container groups**: containers on one host sharing lifecycle, one IP and port set, and volumes (like a Kubernetes pod). The host provides the sum of the containers' CPU and memory.
  - **No autoscaling.** More capacity means deploying more instances yourself.
- **Azure Container Apps:**
  - Serverless, with Kubernetes, KEDA, Dapr, and Envoy managed for you. Apps live in a **Container Apps environment** (shared boundary with a Log Analytics workspace, optionally in your VNet).
  - **Scale rules** between **min and max replicas**: HTTP concurrency, TCP, CPU or memory, or KEDA event sources (for example Service Bus or Storage queue length).
  - **Min replicas = 0 → scale-to-zero**: no replicas and no compute cost when idle.
  - Built-in **HTTPS ingress** (external or internal).
  - **Revisions**: immutable versions. Split traffic for blue-green or canary releases, and shift traffic back to roll back.
- **Picking a service:**
  - "Run a single container quickly, no orchestration" or "nightly job that exits" → ACI.
  - "API with spiky traffic, scale to zero" or "queue-driven microservice" → Container Apps.
  - "Full Kubernetes API, Helm, custom operators" → AKS.

📖 Full lesson: [Provision and Manage Containers in Azure (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-containers-aci-container-apps/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Create and configure Azure App Service

- **App Service** is PaaS for web apps, APIs, and mobile back ends. Every app runs on an **App Service plan**, which holds the OS, region, pricing tier, and VM instances. **You pay for the plan**, and apps on one plan share its CPU, memory, and instances.
- **Tiers** (pick the lowest that meets the requirement):

| Tier | Hardware | Notable |
|---|---|---|
| Free | Shared | Dev/test; no custom domain |
| Shared | Shared | Custom domains; no TLS binding, no autoscale |
| Basic | Dedicated | Custom TLS bindings; manual scale out only |
| Standard | Dedicated | Autoscale; deployment slots (up to 5) |
| Premium | Dedicated, faster | More instances; slots (up to 20) |
| Isolated | App Service Environment in your VNet | Network isolation, maximum scale |

- **Scale up vs scale out:**
  - *Up* = move to a bigger tier (Scale up blade). More CPU and memory per instance, and it can unlock features.
  - *Out* = more instances (Scale out blade), via a manual count or **autoscale** on a metric (CPU, memory, HTTP queue length) or a schedule. Autoscale needs Standard or higher.
- **Creating a web app:** Create a resource > Web App. The default host is `<name>.azurewebsites.net`. Choose the plan, region, and OS.
  - Publish **Code** (pick a runtime stack: .NET, Node.js, Python, Java, PHP) or **Container** (custom image, for example from ACR).
  - App settings and connection strings live on the **Configuration** blade.
- **Custom domain** (Custom domains > Add custom domain):
  - **CNAME** for a subdomain → `<name>.azurewebsites.net`. **A record** → the app's inbound IP for an apex domain.
  - Plus a **TXT** record with the `asuid` value to prove ownership.
- **Certificates and TLS:**
  - Sources: free **App Service managed certificate** (auto-renews), your own uploaded **PFX**, or one imported from **Azure Key Vault**.
  - Create a **TLS/SSL binding** (usually SNI) for the domain.
  - Turn on **HTTPS Only** to redirect HTTP, and optionally raise the **minimum TLS version**.
- **Deployment slots:**
  - A live app in the same plan with its own hostname and config (for example `staging`).
  - Deploy to staging, let it warm up, test, then **swap** for zero-downtime release. **Swap back** to roll back.
  - Mark settings as **deployment slot settings** ("sticky") so they stay put. A staging database connection string must not follow the code into production.
- **Backups:** scheduled or on-demand, to a storage account, covering configuration, file content, and a connected database. Restore to the same or a new app. Configured on the **Backups** blade.
- **Networking:**
  - **Outbound** to private resources → **VNet integration**.
  - **Inbound** control → **access restrictions** (IP allow/deny rules) or a **private endpoint** (private IP, no public exposure).
- **Cues:**
  - "Cheapest tier with slots or autoscale" → Standard.
  - "Bigger instances" → scale up.
  - "Add instances when CPU is high" → autoscale rule.
  - "Release with zero downtime and instant rollback" → deployment slot swap.
  - "Free auto-renewing certificate" → App Service managed certificate.

📖 Full lesson: [Create and Configure Azure App Service (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-app-service-configuration/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

[← Back to the study guide](../README.md)
