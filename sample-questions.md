# AZ-104 Sample Questions with Answers

5 worked practice questions for the **Microsoft Certified: Azure Administrator Associate (AZ-104)** exam. Try each one before you open the answer.

For many more, with the same explanation on every option, use the [AZ-104 practice questions](https://www.savemycert.com/practice/azure-administrator-associate/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide) on SaveMyCert.

## Question 1

*Manage Azure identities and governance*

**A contractor from a partner company needs to sign in and collaborate on a shared Azure DevOps project. The partner already has their own corporate email account. Which action should the administrator take in Microsoft Entra ID?** Choose one.

- **a.** Use Invite external user to send a B2B collaboration invitation to the partner's email
- **b.** Use Create new user to make a member account and set an initial password for the partner
- **c.** Create a service principal for the partner's application
- **d.** Add the partner to a Microsoft 365 group so they inherit access

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ B2B collaboration invites an external person, who redeems the invitation and keeps signing in with their own home credentials.
- **b** ❌ A member account is for your own organization's staff and would require you to manage the partner's password, which B2B avoids.
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

*Implement and manage storage*

**Which type of shared access signature (SAS) is secured with Microsoft Entra ID credentials instead of a storage account access key?** Choose one.

- **a.** User delegation SAS
- **b.** Account SAS
- **c.** Service SAS
- **d.** Stored access policy

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ Correct. A user delegation SAS is signed with a user delegation key obtained through Microsoft Entra ID credentials, so it never exposes or depends on the storage account access keys.
- **b** ❌ An account SAS is always signed with one of the two storage account access keys, not with Entra ID credentials.
- **c** ❌ A service SAS is also signed with a storage account access key; it scopes access to a single service but still relies on the key for its signature.
- **d** ❌ A stored access policy is not a SAS type at all; it is a server-side policy that a service SAS can reference to centralize permissions and expiry.

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

## Question 3

*Deploy and manage Azure compute resources*

**An administrator authors infrastructure definitions in a Bicep file named main.bicep. What does the Bicep CLI produce when the administrator runs 'bicep build main.bicep'?** Choose one.

- **a.** An ARM template in JSON format that Azure Resource Manager can deploy
- **b.** A compiled binary executable that provisions the resources directly
- **c.** A YAML pipeline definition for Azure DevOps
- **d.** A state file that records the current resources in the subscription

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ Bicep is a transparent abstraction over ARM JSON — 'bicep build' transpiles the .bicep file into an equivalent ARM template (main.json) that Resource Manager understands.
- **b** ❌ Bicep is a declarative language, not a compiled program — it never produces executables and never provisions resources itself; deployment is always performed by Azure Resource Manager.
- **c** ❌ Bicep has no relationship to pipeline YAML — CI/CD pipelines can invoke Bicep deployments, but the build command emits ARM JSON, not pipeline definitions.
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

## Question 4

*Implement and manage virtual networking*

**You are planning the address space for a new Azure virtual network that will never be exposed directly to the internet. Which address range is a valid RFC 1918 private range you can assign to the VNet?** Choose one.

- **a.** 10.20.0.0/16
- **b.** 11.0.0.0/16
- **c.** 172.32.0.0/16
- **d.** 192.169.0.0/16

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ 10.0.0.0/8 is one of the three RFC 1918 private ranges, so any block carved from it, such as 10.20.0.0/16, is a valid private VNet address space.
- **b** ❌ 11.0.0.0/8 is publicly routable address space (not part of RFC 1918), so using it risks conflicts with real internet destinations.
- **c** ❌ The RFC 1918 range in the 172 block is 172.16.0.0/12, which ends at 172.31.255.255 — 172.32.0.0 falls just outside it and is public space.
- **d** ❌ The RFC 1918 range is 192.168.0.0/16; 192.169.0.0/16 is adjacent public address space and should not be used for private networks.

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

## Question 5

*Monitor and maintain Azure resources*

**An administrator wants to view the average CPU percentage of an Azure virtual machine over the last hour, with data points that appear within a few minutes of being collected and no agent installation required. Which Azure Monitor data type should the administrator use?** Choose one.

- **a.** Platform metrics
- **b.** Resource logs
- **c.** Activity log events
- **d.** Application Insights traces

<details>
<summary>Show answer and explanation</summary>

**Answer: a**

- **a** ✅ Platform metrics are numeric time-series values collected automatically from Azure resources at near-real-time frequency. Host-level metrics such as Percentage CPU require no agent and are viewable immediately in metrics explorer.
- **b** ❌ Resource logs record operations performed within a resource and must be routed via a diagnostic setting before you can analyze them. They are event records, not lightweight numeric time-series data, and are not the fastest path to a CPU chart.
- **c** ❌ The activity log records subscription-level control-plane operations, such as who started or resized the VM. It contains no performance data like CPU percentage.
- **d** ❌ Application Insights collects application-level telemetry such as requests and dependencies from instrumented apps. It does not report host CPU for a VM and requires instrumentation to collect anything.

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

[← Back to the AZ-104 study guide](README.md)
