# AZ-104 Sample Questions with Answers

20 worked practice questions for the **Microsoft Certified: Azure Administrator Associate (AZ-104)** exam. Try each one before you open the answer.

For many more, with the same explanation on every option, use the [AZ-104 practice questions](https://www.savemycert.com/practice/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide) on SaveMyCert.

## Question 1

*Manage Azure identities and governance*

**A contractor from a partner company needs to sign in and collaborate on a shared Azure DevOps project. The partner already has their own corporate email account. Which action should the administrator take in Microsoft Entra ID?** Choose one.

- **a.** Use Create new user to make a member account and set an initial password for the partner
- **b.** Use Invite external user to send a B2B collaboration invitation to the partner's email
- **c.** Create a service principal for the partner's application
- **d.** Add the partner to a Microsoft 365 group so they inherit access

<details>
<summary>Show answer and explanation</summary>

**Answer: b**

- **a** ❌ A member account is for your own organization's staff and would require you to manage the partner's password, which B2B avoids.
- **b** ✅ B2B collaboration invites an external person, who redeems the invitation and keeps signing in with their own home credentials.
- **c** ❌ A service principal is an application identity, not a way to give a human collaborator sign-in access.
- **d** ❌ A group organizes existing accounts; it does not create an identity for an external person who has no account in your tenant yet.

**The concept.** Member users are internal accounts; guest users are external collaborators invited through Microsoft Entra B2B collaboration.

**Why this is correct.** An external partner who already has their own corporate credentials should be a guest, created with Invite external user (B2B) so they redeem an invitation and sign in with their home account. Create new user (B) makes an internal member you must credential, which is wrong for an outsider. A service principal (C) is an app identity, not a person. Adding to a group (D) cannot conjure an identity that does not yet exist.

**How to reason it out**

1. Identify that the person belongs to another organization and already has credentials.
2. External collaborators map to guest (B2B) users, not members.
3. Choose Invite external user to send a B2B invitation.

> **Exam tip:** External partner sign-in with their own credentials means a guest (B2B) invitation, not a member account.

</details>

📖 Learn this topic: [Manage Microsoft Entra Users and Groups (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/manage-entra-users-groups/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 2

*Manage Azure identities and governance*

**An administrator must onboard 200 new full-time employees into Microsoft Entra ID as member accounts as quickly as possible. Which approach is the most efficient?** Choose one.

- **a.** Use Bulk invite to send each employee a B2B invitation
- **b.** Create each user individually with Create new user
- **c.** Use Bulk create on the Users blade with a completed CSV template
- **d.** Create a dynamic group so the 200 users are added automatically

<details>
<summary>Show answer and explanation</summary>

**Answer: c**

- **a** ❌ Bulk invite creates external guest accounts, which is wrong for your own internal full-time employees.
- **b** ❌ Creating 200 accounts one at a time is exactly what bulk operations exist to avoid.
- **c** ✅ Bulk create uploads one CSV row per member account, provisioning hundreds of users in a single background operation.
- **d** ❌ A dynamic group manages membership of accounts that already exist; it does not create the user accounts.

**The concept.** Large-scale user provisioning uses CSV-driven bulk operations, not manual creation or scripting.

**Why this is correct.** Bulk create provisions many member accounts from one CSV upload, the fastest path for 200 employees. Bulk invite (B) makes guests, not members. Creating each user (C) is the slow manual approach bulk operations replace. A dynamic group (D) only manages membership of existing accounts and creates none.

**How to reason it out**

1. Recognize the accounts are internal members, so use bulk create rather than bulk invite.
2. Download the CSV template and fill one row per employee.
3. Upload it with Bulk create and track the result under Bulk operation results.

> **Exam tip:** Provisioning many internal accounts at once is a CSV Bulk create operation.

</details>

📖 Learn this topic: [Manage Microsoft Entra Users and Groups (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/manage-entra-users-groups/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 3

*Manage Azure identities and governance*

**A team lead must be able to fully create and manage resources in a resource group and also grant other users access to those resources by assigning roles. Which built-in Azure role should the administrator assign?** Choose one.

- **a.** Owner
- **b.** User Access Administrator
- **c.** Contributor
- **d.** Reader

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ Owner grants full access to resources and can also manage access by assigning roles to other principals.
- **b** ❌ User Access Administrator can manage access but cannot manage the resources themselves.
- **c** ❌ Contributor can fully manage resources but cannot grant access or assign roles to others.
- **d** ❌ Reader is view-only and cannot create, manage, or grant access to resources.

**The concept.** Owner has full resource access plus the ability to manage access; Contributor manages resources but cannot grant access.

**Why this is correct.** Only Owner both manages resources and assigns roles to others. Contributor (B) manages resources but cannot grant access. Reader (C) is view-only. User Access Administrator (D) can grant access but cannot manage the resources. The requirement needs both, so Owner is correct.

**How to reason it out**

1. The lead must manage resources AND grant access to others.
2. Contributor manages resources but cannot assign roles.
3. Owner adds the manage-access capability, so assign Owner.

> **Exam tip:** Manage resources plus grant access to others means Owner.

</details>

📖 Learn this topic: [Manage Access to Azure Resources: Azure RBAC (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-rbac-role-assignments/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 4

*Manage Azure identities and governance*

**An administrator is documenting how governance settings flow through their Azure organization. From the top down, what is the correct order of the four-level Azure resource hierarchy?** Choose one.

- **a.** Management group, resource group, subscription, resource
- **b.** Subscription, management group, resource group, resource
- **c.** Management group, subscription, resource group, resource
- **d.** Tenant, subscription, resource, resource group

<details>
<summary>Show answer and explanation</summary>

**Answer: c**

- **a** ❌ Resource groups live inside subscriptions, so a subscription must come before a resource group, not after it.
- **b** ❌ Management groups sit above subscriptions, not below them; a subscription is placed under a management group.
- **c** ✅ This is the correct top-down order: management groups contain subscriptions, subscriptions contain resource groups, and resource groups contain resources.
- **d** ❌ A resource lives inside a resource group, so the resource group comes first; the tenant is the Entra identity boundary, not a level of this resource hierarchy.

**The concept.** Azure organizes resources into a four-level hierarchy where governance applied higher up flows downward.

**Why this is correct.** From the top the levels are management group, then subscription, then resource group, then resource. Each resource belongs to exactly one resource group, each resource group to one subscription, and each subscription can sit under a management group. Option B inverts the top two levels, option C swaps subscription and resource group, and option D places the resource before its resource group.

**How to reason it out**

1. Start at the top: management groups organize subscriptions.
2. A subscription contains resource groups, which in turn contain individual resources.
3. Governance such as policy and locks applied at any level is inherited by everything beneath it.

> **Exam tip:** Memorize management group to subscription to resource group to resource, because inheritance follows that exact order.

</details>

📖 Learn this topic: [Azure Subscriptions, Policy, Locks and Governance (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-subscriptions-governance-policy/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 5

*Manage Azure identities and governance*

**A project team needs a shared mailbox, a shared calendar, a SharePoint site, and a Microsoft Teams workspace provisioned together. Which type of object should the administrator create in Microsoft Entra ID?** Choose one.

- **a.** A security group
- **b.** A dynamic device group
- **c.** An administrative unit
- **d.** A Microsoft 365 group

<details>
<summary>Show answer and explanation</summary>

**Answer: d**

- **a** ❌ A security group grants access, roles, and licenses but does not provision shared collaboration resources like a mailbox or Teams site.
- **b** ❌ A dynamic device group collects devices by rule and offers no mailbox, SharePoint site, or Teams workspace.
- **c** ❌ An administrative unit scopes directory administration to a subset of users; it provides no shared collaboration resources.
- **d** ✅ Creating a Microsoft 365 group provisions a shared mailbox, calendar, SharePoint site, and Teams-ready workspace for collaboration.

**The concept.** Security groups grant access; Microsoft 365 groups add shared collaboration resources.

**Why this is correct.** A Microsoft 365 group exists precisely to provision shared collaboration tools — mailbox, calendar, SharePoint, Teams. A security group (B) manages permissions, not collaboration resources. An administrative unit (C) scopes admin duties, not collaboration. A dynamic device group (D) manages devices and provisions nothing shared.

**How to reason it out**

1. Note the requirement is shared collaboration resources, not permissions.
2. Map shared mailbox, SharePoint, and Teams to a Microsoft 365 group.
3. Create the Microsoft 365 group so the resources are provisioned automatically.

> **Exam tip:** A shared mailbox, SharePoint site, and Teams workspace means a Microsoft 365 group.

</details>

📖 Learn this topic: [Manage Microsoft Entra Users and Groups (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/manage-entra-users-groups/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 6

*Implement and manage storage*

**Which type of shared access signature (SAS) is secured with Microsoft Entra ID credentials instead of a storage account access key?** Choose one.

- **a.** Account SAS
- **b.** User delegation SAS
- **c.** Stored access policy
- **d.** Service SAS

<details>
<summary>Show answer and explanation</summary>

**Answer: b**

- **a** ❌ An account SAS is always signed with one of the two storage account access keys, not with Entra ID credentials.
- **b** ✅ Correct. A user delegation SAS is signed with a user delegation key obtained through Microsoft Entra ID credentials, so it never exposes or depends on the storage account access keys.
- **c** ❌ A stored access policy is not a SAS type at all; it is a server-side policy that a service SAS can reference to centralize permissions and expiry.
- **d** ❌ A service SAS is also signed with a storage account access key; it scopes access to a single service but still relies on the key for its signature.

**The concept.** Azure Storage supports three SAS types: account SAS, service SAS, and user delegation SAS. The first two are signed with a storage account access key; the user delegation SAS is signed with a user delegation key requested via Microsoft Entra ID.

**Why this is correct.** The user delegation SAS is the only SAS type whose signature is based on Entra ID credentials. A principal with the right RBAC permissions requests a user delegation key from the Blob service and uses it to sign the SAS, so the account keys never need to be handled or distributed. Account SAS and service SAS both fail this requirement because their signatures come from the shared account keys, and a stored access policy is a management construct for service SAS tokens, not a SAS itself.

**How to reason it out**

1. Recall the three SAS types: account SAS, service SAS, and user delegation SAS.
2. Identify what signs each one: account keys for account and service SAS, an Entra-issued user delegation key for the user delegation SAS.
3. Eliminate the stored access policy option because it is a policy referenced by a service SAS, not a signature mechanism.
4. Select user delegation SAS as the Entra-secured option, which is also Microsoft's recommended SAS type for Blob Storage.

> **Exam tip:** A user delegation SAS is signed with Microsoft Entra ID credentials, making it the most secure SAS type because account keys are never used.

</details>

📖 Learn this topic: [Configure Access to Azure Storage: SAS, Keys & Firewalls (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-access-sas/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 7

*Implement and manage storage*

**A partner application needs read-only access to a single blob in a container for 24 hours. Your security team requires that storage account access keys are never used or distributed. What is the MOST secure way to grant this access?** Choose one.

- **a.** Generate a service SAS for the container signed with key1
- **b.** Share the storage account access key with the partner and ask them to use it only for reads
- **c.** Generate a user delegation SAS scoped to the blob with read permission and a 24-hour expiry
- **d.** Generate an account SAS with read permission across all services

<details>
<summary>Show answer and explanation</summary>

**Answer: c**

- **a** ❌ A service SAS is still signed with a storage account access key, which the security team has ruled out, and container scope is wider than the single blob required.
- **b** ❌ An access key grants full authorization to the entire account and cannot be limited to read-only or to one blob; distributing it directly contradicts the requirement.
- **c** ✅ Correct. A user delegation SAS is signed with Entra ID credentials rather than an account key, and scoping it to one blob with read-only permission and a short expiry follows least privilege.
- **d** ❌ An account SAS is signed with an account access key, violating the requirement, and granting access across all services is far broader than the single blob needed.

**The concept.** When granting temporary, delegated access to blob data, prefer a user delegation SAS: it is signed with Microsoft Entra ID credentials, can be scoped tightly to a resource and permission set, and avoids ever handling account keys.

**Why this is correct.** The user delegation SAS satisfies every constraint: no account key is used in signing, the scope is a single blob, the permission is read-only, and the expiry is 24 hours. The account SAS and service SAS options both fail because their signatures require an account access key, and the account SAS additionally over-grants across services. Handing out the access key itself is the worst option because keys confer full, non-scoped access to all data in the account.

**How to reason it out**

1. Note the constraint: account access keys must not be used or shared, which eliminates account SAS, service SAS, and direct key distribution.
2. Choose the user delegation SAS, which is signed with an Entra-issued user delegation key.
3. Scope the SAS to the specific blob, grant only the read permission, and set the expiry to 24 hours.
4. Deliver the SAS URL to the partner over a secure channel and require HTTPS-only access.

> **Exam tip:** For time-limited delegated access without touching account keys, issue a tightly scoped user delegation SAS.

</details>

📖 Learn this topic: [Configure Access to Azure Storage: SAS, Keys & Firewalls (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-access-sas/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 8

*Implement and manage storage*

**Which Azure Storage redundancy option maintains three copies of your data within a single datacenter and offers the lowest cost?** Choose one.

- **a.** Geo-zone-redundant storage (GZRS)
- **b.** Geo-redundant storage (GRS)
- **c.** Zone-redundant storage (ZRS)
- **d.** Locally redundant storage (LRS)

<details>
<summary>Show answer and explanation</summary>

**Answer: d**

- **a** ❌ GZRS combines ZRS in the primary region with replication to a secondary region; it is one of the most protective and most expensive options, not the cheapest.
- **b** ❌ GRS adds asynchronous replication to a secondary region on top of LRS in the primary, so it is not confined to one datacenter and is not the cheapest.
- **c** ❌ ZRS spreads the three copies across three availability zones in the region, which protects against a datacenter outage but costs more than LRS.
- **d** ✅ Correct. LRS keeps three synchronous copies within one datacenter in the primary region, making it the cheapest redundancy option.

**The concept.** Azure Storage redundancy options trade cost against protection scope: LRS protects within one datacenter, ZRS across availability zones, and GRS/GZRS add a secondary region.

**Why this is correct.** LRS is defined as three synchronous copies inside a single datacenter and sits at the bottom of the price ladder, so it uniquely matches both parts of the question. ZRS leaves the single-datacenter boundary by using three zones, and GRS and GZRS both replicate to another region entirely, adding cost and protection the question does not describe.

**How to reason it out**

1. Map each redundancy option to its protection boundary: datacenter (LRS), availability zones (ZRS), secondary region (GRS/GZRS).
2. Note that price rises with the protection boundary, so LRS is cheapest.
3. Match 'three copies in a single datacenter' plus 'lowest cost' to LRS.
4. Remember LRS still gives eleven nines of durability over a year, but a datacenter-level disaster can cause data loss.

> **Exam tip:** LRS is three copies in one datacenter and the lowest-cost redundancy; every other option widens the failure boundary at higher cost.

</details>

📖 Learn this topic: [Configure & Manage Azure Storage Accounts and Redundancy (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-accounts-redundancy/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 9

*Implement and manage storage*

**You create a container named 'reports' in an Azure Blob Storage account. External users must be able to download individual report blobs anonymously using a direct URL, but they must NOT be able to list the other blobs in the container. Which container access level should you configure?** Choose one.

- **a.** Container (anonymous read access for container and blobs)
- **b.** Blob (anonymous read access for blobs only)
- **c.** Private (no anonymous access)
- **d.** Configure a stored access policy on the container

<details>
<summary>Show answer and explanation</summary>

**Answer: b**

- **a** ❌ The Container level also permits anonymous listing of all blobs in the container, which violates the requirement that users must not see other blobs.
- **b** ✅ The Blob access level allows anonymous read of individual blobs via direct URL while denying anonymous enumeration of the container's contents — exactly the requirement.
- **c** ❌ Private blocks all anonymous access, so external users could not download any blob without a SAS token or credentials.
- **d** ❌ A stored access policy governs SAS tokens; it does not by itself grant anonymous URL access, and users would still need a SAS appended to each URL.

**The concept.** Azure Blob Storage containers support three anonymous access levels: Private (default, no anonymous access), Blob (anonymous read of blobs by exact URL only), and Container (anonymous read plus listing of container contents).

**Why this is correct.** The Blob access level is the precise middle ground: anyone with a blob's full URL can read that blob, but anonymous requests to list the container are rejected. Private would block the downloads entirely, and Container would expose the full blob inventory to anyone, letting them discover reports they were never given links to. A stored access policy is a SAS-management feature, not an anonymous-access setting. Note that 'Allow blob anonymous access' must also be enabled at the storage account level for any container-level setting other than Private to take effect.

**How to reason it out**

1. In the storage account, confirm 'Allow blob anonymous access' is enabled under Configuration.
2. Open the container, select 'Change access level', and choose 'Blob (anonymous read access for blobs only)'.
3. Distribute the direct blob URLs; verify that a GET on a blob URL succeeds while a container list operation returns an authorization error.

> **Exam tip:** Choose the Blob access level when blobs must be publicly readable by URL without exposing the container's file listing.

</details>

📖 Learn this topic: [Configure Azure Files and Azure Blob Storage (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-files-blob-storage/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 10

*Deploy and manage Azure compute resources*

**An administrator authors infrastructure definitions in a Bicep file named main.bicep. What does the Bicep CLI produce when the administrator runs 'bicep build main.bicep'?** Choose one.

- **a.** A compiled binary executable that provisions the resources directly
- **b.** A YAML pipeline definition for Azure DevOps
- **c.** An ARM template in JSON format that Azure Resource Manager can deploy
- **d.** A state file that records the current resources in the subscription

<details>
<summary>Show answer and explanation</summary>

**Answer: c**

- **a** ❌ Bicep is a declarative language, not a compiled program — it never produces executables and never provisions resources itself; deployment is always performed by Azure Resource Manager.
- **b** ❌ Bicep has no relationship to pipeline YAML — CI/CD pipelines can invoke Bicep deployments, but the build command emits ARM JSON, not pipeline definitions.
- **c** ✅ Bicep is a transparent abstraction over ARM JSON — 'bicep build' transpiles the .bicep file into an equivalent ARM template (main.json) that Resource Manager understands.
- **d** ❌ Bicep deliberately has no state file — Azure Resource Manager itself is the source of truth for deployed resources, unlike tools such as Terraform.

**The concept.** Bicep is a domain-specific language that transpiles to standard ARM JSON templates. Everything a Bicep file can express is expressed as an ARM template underneath, so the deployment engine and capabilities are identical.

**Why this is correct.** 'bicep build' converts the .bicep source into an ARM template JSON file — this is the core relationship between the two languages. It does not produce executables (Bicep is declarative, not procedural), it does not generate pipeline YAML (a separate concern), and it maintains no state file because Azure Resource Manager is the source of truth for what exists.

**How to reason it out**

1. Author the infrastructure in main.bicep using Bicep's concise declarative syntax.
2. Run 'bicep build main.bicep' (or let the Azure CLI/PowerShell do it automatically at deploy time) to emit main.json.
3. Deploy either the .bicep file directly or the emitted JSON with 'az deployment group create' — Resource Manager processes the same ARM JSON either way.

> **Exam tip:** Bicep transpiles to ARM JSON — same engine, same capabilities, friendlier syntax, and no state file.

</details>

📖 Learn this topic: [ARM Templates and Bicep: AZ-104 Deployment Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-arm-templates-bicep/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 11

*Deploy and manage Azure compute resources*

**A resource group named RG-App contains a virtual network, a storage account, and a virtual machine. An administrator deploys an ARM template to RG-App that defines only the virtual network, using Complete mode. What happens to the storage account and the virtual machine?** Choose one.

- **a.** They are moved to a recovery resource group for 14 days before deletion
- **b.** They are deleted, because Complete mode removes resources in the resource group that are not defined in the template
- **c.** They are left unchanged, because deployments only affect resources defined in the template
- **d.** The deployment fails with a conflict error because the template does not include them

<details>
<summary>Show answer and explanation</summary>

**Answer: b**

- **a** ❌ There is no recovery holding area for Complete-mode deletions — removed resources are deleted immediately, with no built-in grace period.
- **b** ✅ In Complete mode, Resource Manager makes the resource group match the template exactly — resources present in the group but absent from the template are deleted.
- **c** ❌ That describes Incremental mode, the default. Complete mode was chosen here specifically to make the group match the template, which means deleting the extras.
- **d** ❌ Complete mode does not fail on undeclared resources — it silently deletes them, which is exactly why it must be used with care.

**The concept.** ARM deployments run in one of two modes. Incremental (default) adds or updates resources defined in the template and leaves everything else alone. Complete makes the resource group match the template exactly, deleting resources that exist in the group but are not in the template.

**Why this is correct.** Because the deployment used Complete mode and the template defines only the virtual network, the storage account and VM are not in the desired state and are deleted. 'Left unchanged' describes Incremental mode. The deployment does not error on extra resources — silent deletion is the documented (and dangerous) behavior. There is no recovery resource group or grace period for deleted resources.

**How to reason it out**

1. Before any Complete-mode deployment, list what the template defines and compare it against the resource group's current contents.
2. Run the what-if operation ('az deployment group what-if' or -WhatIf) to preview exactly which resources would be deleted.
3. Only then deploy with '--mode Complete', or keep the default Incremental mode if extra resources must survive.

> **Exam tip:** Complete mode deletes anything in the resource group that the template doesn't define — always preview with what-if first.

</details>

📖 Learn this topic: [ARM Templates and Bicep: AZ-104 Deployment Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-arm-templates-bicep/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 12

*Deploy and manage Azure compute resources*

**An administrator places two virtual machines in an availability set configured with two fault domains. What does each fault domain represent?** Choose one.

- **a.** A physically separate datacenter within the Azure region
- **b.** A paired Azure region used for geo-redundant failover
- **c.** A group of VMs that are rebooted together during planned platform maintenance
- **d.** A group of hardware that shares a common power source and network switch within the datacenter

<details>
<summary>Show answer and explanation</summary>

**Answer: d**

- **a** ❌ Physically separate datacenters within a region are availability zones — fault domains exist inside a single datacenter.
- **b** ❌ Region pairs are a geo-resilience concept for services like GRS storage — they have nothing to do with placement inside an availability set.
- **c** ❌ That describes an update domain — the availability set's other axis, which governs planned maintenance rather than hardware failure.
- **d** ✅ A fault domain is a rack-level hardware grouping — spreading VMs across fault domains means a single power or switch failure cannot take down both.

**The concept.** An availability set spreads VMs across fault domains and update domains inside one datacenter. Fault domains isolate against unplanned hardware failure (shared power/network per rack); update domains isolate against planned maintenance reboots.

**Why this is correct.** A fault domain maps to a rack sharing power and a network switch, so distributing VMs across fault domains ensures a single hardware failure affects only one of them. Separate datacenters in a region are availability zones, a stronger construct. Reboot groupings for planned maintenance are update domains. Region pairs operate at geographic scale, far above VM placement.

**How to reason it out**

1. Create the availability set before the VMs and choose the fault domain count (up to 3 in most regions).
2. Deploy each VM into the availability set at creation time — membership cannot be changed afterward.
3. Azure automatically distributes the VMs across the set's fault and update domains.

> **Exam tip:** Fault domains = shared power/network (unplanned failures); update domains = maintenance reboot groups (planned events).

</details>

📖 Learn this topic: [Azure Virtual Machines: Sizes, Disks, and Zones (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-machines-configuration/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 13

*Deploy and manage Azure compute resources*

**You need to run a containerized nightly batch job that processes a data export and then exits. The container should not be restarted after it completes successfully, but it should be restarted automatically if it fails. Which restart policy should you configure for the Azure Container Instances container group?** Choose one.

- **a.** Manual
- **b.** Always
- **c.** Never
- **d.** OnFailure

<details>
<summary>Show answer and explanation</summary>

**Answer: d**

- **a** ❌ Manual is not a valid ACI restart policy. The only supported values are Always, OnFailure, and Never.
- **b** ❌ Always restarts the container even after a successful exit, so a completed batch job would be re-run in an endless loop. Always is intended for long-running services, not run-once tasks.
- **c** ❌ Never runs the container at most once regardless of the exit code, so a transient failure would leave the job unfinished with no automatic retry.
- **d** ✅ Correct. The OnFailure restart policy restarts containers only when they terminate with a non-zero exit code, which is exactly the behavior a run-to-completion batch job needs: retry on failure, stop on success.

**The concept.** Azure Container Instances container groups support three restart policies — Always, OnFailure, and Never — which control what ACI does when a container process exits.

**Why this is correct.** A batch job is a run-to-completion workload: when it exits with code 0 the work is done and the container should stay stopped, but a non-zero exit means something went wrong and the job should be retried. OnFailure implements exactly that logic — ACI restarts the container only on a failed exit. Always would loop a successful job forever because ACI treats the workload as a service that must stay up, and Never gives you no retry at all, leaving transient failures unrecovered. Manual is not part of the ACI restart policy set.

**How to reason it out**

1. Classify the workload: run-to-completion task (batch, build, one-off script) versus long-running service.
2. For tasks that must retry on error, set --restart-policy OnFailure when creating the container group.
3. For tasks that must run at most once, choose Never; reserve Always for long-lived services.
4. Verify behavior by checking the container state after exit: OnFailure leaves a succeeded container in a Terminated state.

> **Exam tip:** ACI restart policies: Always for services, OnFailure for retryable batch jobs, Never for run-at-most-once tasks.

</details>

📖 Learn this topic: [Provision and Manage Containers in Azure (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-containers-aci-container-apps/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 14

*Implement and manage virtual networking*

**You are planning the address space for a new Azure virtual network that will never be exposed directly to the internet. Which address range is a valid RFC 1918 private range you can assign to the VNet?** Choose one.

- **a.** 10.20.0.0/16
- **b.** 192.169.0.0/16
- **c.** 172.32.0.0/16
- **d.** 11.0.0.0/16

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ 10.0.0.0/8 is one of the three RFC 1918 private ranges, so any block carved from it, such as 10.20.0.0/16, is a valid private VNet address space.
- **b** ❌ The RFC 1918 range is 192.168.0.0/16; 192.169.0.0/16 is adjacent public address space and should not be used for private networks.
- **c** ❌ The RFC 1918 range in the 172 block is 172.16.0.0/12, which ends at 172.31.255.255 — 172.32.0.0 falls just outside it and is public space.
- **d** ❌ 11.0.0.0/8 is publicly routable address space (not part of RFC 1918), so using it risks conflicts with real internet destinations.

**The concept.** Azure virtual networks should use RFC 1918 private address space: 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16. Choosing public ranges can break connectivity to real internet endpoints and complicates future peering and hybrid connectivity.

**Why this is correct.** 10.20.0.0/16 sits inside 10.0.0.0/8, one of the three RFC 1918 private ranges, so it is a safe, valid VNet address space. The distractors are traps built on off-by-one boundaries: 11.0.0.0/16 is outside 10.0.0.0/8 entirely, 172.32.0.0/16 is just past the end of 172.16.0.0/12 (which stops at 172.31.255.255), and 192.169.0.0/16 is just past 192.168.0.0/16. All three are publicly routable ranges.

**How to reason it out**

1. Memorize the three RFC 1918 ranges: 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16.
2. Check whether the candidate range falls entirely inside one of those blocks — pay attention to the exact boundaries (172.16–172.31, not 172.32).
3. Also plan for uniqueness: pick ranges that will not overlap with other VNets or on-premises networks you may later peer or connect to.

> **Exam tip:** Valid private VNet space comes from 10.0.0.0/8, 172.16.0.0/12, or 192.168.0.0/16 — watch the exact block boundaries.

</details>

📖 Learn this topic: [Azure Virtual Networks, Subnets, Peering & User-Defined Routes](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-networks-peering/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 15

*Implement and manage virtual networking*

**You create a subnet with the address range 10.1.1.0/24 in an Azure virtual network. How many IP addresses in that subnet are available for you to assign to resources?** Choose one.

- **a.** 251
- **b.** 250
- **c.** 254
- **d.** 256

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ A /24 has 256 addresses, and Azure reserves 5 in every subnet (network address, default gateway, two for Azure DNS, and broadcast), leaving 251 usable.
- **b** ❌ 250 would imply Azure reserves 6 addresses; the actual number reserved in every Azure subnet is 5.
- **c** ❌ 254 is the classic on-premises answer (256 minus network and broadcast), but Azure reserves 5 addresses per subnet, not 2.
- **d** ❌ 256 is the raw size of a /24 with nothing reserved; no network — Azure included — hands you every address in the block.

**The concept.** Azure reserves 5 IP addresses in every subnet: x.x.x.0 (network), x.x.x.1 (default gateway), x.x.x.2 and x.x.x.3 (mapped for Azure DNS), and the last address (broadcast).

**Why this is correct.** A /24 contains 256 addresses. Subtracting Azure's 5 reserved addresses leaves 251 assignable. The 254 distractor is the traditional networking answer that only subtracts network and broadcast — the most common wrong answer on this question. 256 ignores reservations entirely, and 250 over-counts the reservation.

**How to reason it out**

1. Compute the raw subnet size: 2^(32 - prefix length); for /24 that is 256.
2. Subtract Azure's 5 reserved addresses (first four addresses plus the last one).
3. Size subnets with this overhead in mind — this is also why Azure's smallest supported subnet is a /29, which yields only 3 usable addresses.

> **Exam tip:** Azure reserves 5 IPs in every subnet, so a /24 gives you 251 usable addresses, not 254.

</details>

📖 Learn this topic: [Azure Virtual Networks, Subnets, Peering & User-Defined Routes](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-networks-peering/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 16

*Implement and manage virtual networking*

**A network security group contains two custom inbound rules for the same traffic: rule "AllowWeb" (priority 100, allow TCP 443) and rule "DenyWeb" (priority 200, deny TCP 443). An HTTPS request arrives at a VM in the associated subnet. What happens?** Choose one.

- **a.** The traffic is allowed, because priority 100 is evaluated before priority 200 and processing stops at the first match
- **b.** The traffic is denied, because deny rules always override allow rules regardless of priority
- **c.** The traffic is denied, because higher priority numbers are evaluated first
- **d.** The NSG rejects the configuration, because two rules cannot reference the same port

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ NSG rules are evaluated in priority order — lower number first — and once a rule matches, evaluation stops; AllowWeb at 100 matches and admits the traffic before DenyWeb is ever considered.
- **b** ❌ NSGs have no deny-wins semantics; only priority order decides, and the deny rule here sits at a lower priority (higher number) than the allow.
- **c** ❌ This inverts the rule: in NSGs a LOWER number means HIGHER priority, so 100 is processed before 200.
- **d** ❌ Overlapping rules are perfectly valid in an NSG — priority order exists precisely to resolve which overlapping rule wins.

**The concept.** NSG rules are processed in ascending priority-number order (100 before 200 — lower number wins), and evaluation stops at the first rule whose conditions match. There is no deny-overrides behavior.

**Why this is correct.** AllowWeb (100) matches the HTTPS flow first and processing halts, so the traffic is allowed. The deny-always-wins distractor imports firewall semantics from other products that NSGs do not have. The higher-number-first distractor reverses the ordering. And overlapping rules are legal — the priority mechanism exists to arbitrate them.

**How to reason it out**

1. List the NSG rules sorted by priority number, lowest first.
2. Walk down the list until a rule's source, destination, port, and protocol all match the flow.
3. Apply that rule's action and stop — later rules, including denies, are never reached.
4. If no custom rule matches, the default rules (ending in DenyAllInbound) decide.

> **Exam tip:** In NSGs, lower priority number = evaluated first, and the first match wins — there is no deny-overrides.

</details>

📖 Learn this topic: [Azure NSGs, Bastion, Service Endpoints & Private Endpoints](https://www.savemycert.com/revision/azure-administrator-associate/azure-nsg-bastion-private-endpoints/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 17

*Implement and manage virtual networking*

**You deploy two virtual machines, VM1 and VM2, into the same subnet of a virtual network. Without configuring any DNS settings, VM1 can resolve VM2 by its hostname. Which service provides this name resolution?** Choose one.

- **a.** An Azure DNS public zone hosting the VMs' records
- **b.** An Azure Private DNS zone linked to the virtual network
- **c.** A custom DNS server deployed in the virtual network
- **d.** Azure-provided DNS, reachable at the virtual IP 168.63.129.16

<details>
<summary>Show answer and explanation</summary>

**Answer: d**

- **a** ❌ Public DNS zones host records for internet-facing domains you delegate to Azure DNS. They are not created automatically and do not resolve internal VM hostnames inside a virtual network.
- **b** ❌ A private DNS zone must be explicitly created and linked to the virtual network. Nothing was configured here, so the default Azure-provided DNS is doing the resolution, not a private zone.
- **c** ❌ A custom DNS server only participates in resolution after you configure its IP address in the virtual network's DNS settings. No such configuration was made in this scenario.
- **d** ✅ Correct. Every virtual network includes Azure-provided DNS by default. It resolves the hostnames of VMs within the same virtual network with zero configuration, using the platform virtual IP 168.63.129.16.

**The concept.** Azure-provided DNS is the default name resolution service built into every virtual network. It resolves VM hostnames within the same virtual network automatically, without any zones, links, or servers to configure.

**Why this is correct.** Because no DNS settings were changed, the virtual network is using its default: Azure-provided DNS at 168.63.129.16. This service registers each VM's hostname automatically and answers queries from VMs in the same virtual network. Private DNS zones, public zones, and custom DNS servers all require explicit setup, so none of them can be the answer in a zero-configuration scenario. The key limitation to remember is that Azure-provided DNS works only within a single virtual network and offers no control over the namespace, which is why private DNS zones exist for cross-network and custom-domain scenarios.

**How to reason it out**

1. Identify that no DNS configuration was performed, which means the virtual network is using its default resolver.
2. Recall that Azure-provided DNS (168.63.129.16) automatically registers and resolves VM hostnames within one virtual network.
3. Eliminate private zones, public zones, and custom DNS servers because each requires explicit creation or configuration.

> **Exam tip:** Azure-provided DNS gives automatic hostname resolution inside a single virtual network; anything beyond that needs a private DNS zone or custom DNS.

</details>

📖 Learn this topic: [Azure DNS and Load Balancer: AZ-104 Networking Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-dns-load-balancer/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 18

*Monitor and maintain Azure resources*

**An administrator wants to view the average CPU percentage of an Azure virtual machine over the last hour, with data points that appear within a few minutes of being collected and no agent installation required. Which Azure Monitor data type should the administrator use?** Choose one.

- **a.** Application Insights traces
- **b.** Activity log events
- **c.** Platform metrics
- **d.** Resource logs

<details>
<summary>Show answer and explanation</summary>

**Answer: c**

- **a** ❌ Application Insights collects application-level telemetry such as requests and dependencies from instrumented apps. It does not report host CPU for a VM and requires instrumentation to collect anything.
- **b** ❌ The activity log records subscription-level control-plane operations, such as who started or resized the VM. It contains no performance data like CPU percentage.
- **c** ✅ Platform metrics are numeric time-series values collected automatically from Azure resources at near-real-time frequency. Host-level metrics such as Percentage CPU require no agent and are viewable immediately in metrics explorer.
- **d** ❌ Resource logs record operations performed within a resource and must be routed via a diagnostic setting before you can analyze them. They are event records, not lightweight numeric time-series data, and are not the fastest path to a CPU chart.

**The concept.** Azure Monitor stores two fundamental data types: metrics (lightweight numeric time-series values, collected at regular intervals, near-real-time) and logs (event and trace records with rich properties, queried with KQL in a Log Analytics workspace).

**Why this is correct.** Platform metrics fit every requirement in the scenario: they are collected automatically for all Azure resources with no agent, they are numeric samples suitable for charting an average, and they appear in metrics explorer within minutes. Resource logs need a diagnostic setting and a destination before analysis, the activity log only records control-plane operations (not performance counters), and Application Insights is app telemetry that requires instrumentation and never captures host CPU.

**How to reason it out**

1. Open the virtual machine in the Azure portal and select Metrics under Monitoring.
2. Choose the Percentage CPU metric from the Virtual Machine Host namespace.
3. Set the aggregation to Avg and the time range to the last hour to render the chart.

> **Exam tip:** Platform metrics are agentless, numeric, and near-real-time; logs are event records that need routing and KQL to analyze.

</details>

📖 Learn this topic: [Azure Monitor: Metrics, Logs, Diagnostic Settings & Alerts (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-monitor-metrics-logs-alerts/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 19

*Monitor and maintain Azure resources*

**In Azure Monitor metrics explorer, an administrator charts the Transactions metric for a storage account and wants to see a separate line for each API operation type (for example GetBlob versus PutBlob) on the same chart. What should the administrator apply to the chart?** Choose one.

- **a.** The Count aggregation
- **b.** Splitting by the API name dimension
- **c.** A dynamic threshold
- **d.** A second metric namespace

<details>
<summary>Show answer and explanation</summary>

**Answer: b**

- **a** ❌ Changing the aggregation alters how samples in each time interval are combined into a single value. It still produces one line, not one line per operation type.
- **b** ✅ Applying splitting on a metric dimension renders one series per dimension value, so each API operation type gets its own line on the same chart.
- **c** ❌ Dynamic thresholds belong to metric alert rules, where machine learning derives the alert boundary from historical data. They have no effect on how a metrics explorer chart is drawn.
- **d** ❌ A namespace groups related metrics for a resource type (for example blob versus file metrics). Changing namespace changes which metrics are listed; it does not break one metric into per-operation series.

**The concept.** Many platform metrics carry dimensions - name-value properties such as API name or response type. Metrics explorer can filter on a dimension (limit which values are included) or split on a dimension (draw a separate series per value).

**Why this is correct.** Splitting by the API name dimension is exactly the feature that turns a single aggregated Transactions line into one line per operation type. A namespace only selects which set of metrics is available, an aggregation only changes how each time bucket is summarized into one number, and dynamic thresholds are an alerting concept that never appears on an explorer chart.

**How to reason it out**

1. In metrics explorer, select the storage account, the Transactions metric, and an aggregation such as Sum.
2. Select Apply splitting and choose the API name dimension.
3. Optionally add a filter on the same dimension to limit the chart to specific operations of interest.

> **Exam tip:** Use dimension splitting in metrics explorer to render one series per dimension value; use filtering to narrow which values are charted.

</details>

📖 Learn this topic: [Azure Monitor: Metrics, Logs, Diagnostic Settings & Alerts (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-monitor-metrics-logs-alerts/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

## Question 20

*Monitor and maintain Azure resources*

**You need to configure backup protection for 20 Azure virtual machines in the East US region. Which resource must you create first to store the VM recovery points?** Choose one.

- **a.** A Recovery Services vault
- **b.** A Backup vault
- **c.** A storage account with a blob container
- **d.** An Azure Site Recovery replication policy

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ Azure VM backup is managed through a Recovery Services vault, which stores the recovery points and hosts the backup policies applied to the VMs.
- **b** ❌ A Backup vault protects newer workloads such as Azure Blobs, Azure Disks, and Azure Database for PostgreSQL — it does not support Azure VM backup, which requires a Recovery Services vault.
- **c** ❌ You never create a storage account for VM backups; the vault abstracts the underlying storage and manages it for you.
- **d** ❌ A replication policy belongs to Azure Site Recovery, which replicates VMs for disaster recovery — it does not create point-in-time backup recovery points.

**The concept.** Azure Backup uses two vault types: the Recovery Services vault (Azure VMs, SQL in Azure VMs, SAP HANA, Azure Files, and on-premises MARS/MABS/DPM backups) and the Backup vault (Azure Blobs, Azure Disks, Azure Database for PostgreSQL, and other newer workloads).

**Why this is correct.** Backing up Azure virtual machines is a Recovery Services vault scenario — the vault stores the recovery points and the backup policy that defines schedule and retention. A Backup vault cannot protect Azure VMs, a storage account is never provisioned directly for VM backup because the vault manages its own storage, and a Site Recovery replication policy provides DR replication rather than point-in-time backups.

**How to reason it out**

1. Identify the workload to protect — Azure virtual machines.
2. Match the workload to the vault type: Azure VMs are protected by a Recovery Services vault.
3. Create the Recovery Services vault in the same region as the VMs, then configure a backup policy and enable backup on each VM.

> **Exam tip:** Azure VM backup always goes through a Recovery Services vault — Backup vaults are for Blobs, Disks, and PostgreSQL.

</details>

📖 Learn this topic: [Azure Backup & Site Recovery: Vaults, Policies & Failover (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-backup-site-recovery/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

---

[← Back to the AZ-104 study guide](README.md)
