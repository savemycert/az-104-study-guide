# Domain 1: Manage Azure identities and governance (24%)

Tied for the largest domain on AZ-104. Questions are mostly administrator decisions: which identity object to create, which role to assign at which scope, and which governance control (Policy, lock, tag, budget) fits the requirement.

## Manage Microsoft Entra users and groups

- **Microsoft Entra ID** is the new name for Azure Active Directory (Azure AD). It is the cloud directory that authenticates users for Azure, Microsoft 365, and other apps. Older docs that say "Azure AD" still describe the same service.
- User properties (department, job title, usage location) do real work: dynamic group rules and licensing read them.

| | Member user | Guest user |
|---|---|---|
| Represents | Your own staff | Partner, vendor, or other outside collaborator |
| Portal action | Users > New user > **Create new user** | Users > New user > **Invite external user** |
| Password managed by | Your tenant | The guest's home organization |
| UPN shape | `name@yourtenant...` | `alice_contoso.com#EXT#@yourtenant.onmicrosoft.com` |

- **Bulk operations** (Users blade): Bulk create, Bulk invite, Bulk delete, Download users.
  - Download the provided CSV template, fill one row per user, upload. Keep the template's header rows.
  - Failed rows are reported under **Bulk operation results**. Fix and re-upload.
  - Cue: "onboard 200 accounts" or "invite 40 contractors" → CSV bulk operation, not a custom script.
- **Group types:**

| | Security group | Microsoft 365 group |
|---|---|---|
| Job | Grant roles, app access, licenses | Team collaboration |
| Members | Users, devices, other groups | Users only |
| Comes with | Nothing extra | Shared mailbox, calendar, SharePoint site, Teams workspace |
| Dynamic rules | User or device | User only |

- **Membership types:**
  - *Assigned*: you add and remove each member by hand. Works in any edition.
  - *Dynamic User / Dynamic Device*: a rule on attributes adds and removes members automatically. Needs **Microsoft Entra ID P1** for members the rule covers.
  - Example rule: `user.department -eq "Sales"`. Change a user's department and the group follows; you maintain the attribute, not the list.
- **Group-based licensing**: assign a product license to a group and members inherit it; leaving the group returns the license. Configured under Microsoft Entra ID > Billing > Licenses > All products > Assign. Individual service plans can be switched off per assignment. Requires P1.
  - Pair it with a dynamic group so new hires are licensed with no admin action.
  - Trap: a user with no **usage location** fails license assignment.
- **External collaboration settings** control who can send invitations, whether guests can invite guests, and which domains are allowed or blocked.
- **Self-service password reset (SSPR)**: Microsoft Entra ID > Password reset. Scope it to **None**, **Selected** (a pilot group), or **All**. Users register authentication methods (phone, email, security questions, Authenticator app), and you set how many are required. **Password writeback** sends resets back to on-premises AD for synced accounts.
- **Entra ID vs on-premises AD DS:** Entra ID is flat and cloud-based and uses HTTPS protocols (OAuth, OpenID Connect, SAML). It has no OUs, no Group Policy, and no forests or domain controllers. AD DS uses LDAP and Kerberos. **Microsoft Entra Connect** syncs the two.
- **Cues:**
  - "Auto-add users by attribute" → dynamic group.
  - "Partner from another company needs access" → guest (B2B) invite.
  - "Team needs a shared mailbox and Teams site" → Microsoft 365 group.
  - "Assign a role or license to many users" → security group.

📖 Full lesson: [Manage Microsoft Entra Users and Groups (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/manage-entra-users-groups/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Manage access to Azure resources

- **Azure RBAC** answers authorization (what you may do to Azure resources). Microsoft Entra ID answers authentication (who you are). Managed on the **Access control (IAM)** blade of every management group, subscription, resource group, and resource.
- **A role assignment = security principal + role definition + scope.**
  - *Principal*: user, group, service principal (app identity), or managed identity. Assign to groups where possible.
  - *Role definition*: `Actions` / `NotActions` (management plane) and `DataActions` / `NotDataActions` (data plane, for example reading a blob).
  - *Scope*: management group, subscription, resource group, or single resource. Set by where you open IAM.

| Built-in role | Manage resources | Grant access to others |
|---|---|---|
| Owner | Yes | Yes |
| Contributor | Yes | **No** |
| Reader | View only | No |
| User Access Administrator | No | Yes |

- Service-specific roles (Virtual Machine Contributor, Storage Blob Data Reader) are the least-privilege pick when full Contributor is too much.
- **Scope and inheritance:**
  - Order: management group > subscription > resource group > resource.
  - Assignments flow **down only**. A resource group assignment grants nothing on the parent subscription or sibling groups.
  - Resources created later under the scope inherit the assignment too.
  - Cue: "auditors need read access to all 12 resource groups" → one **Reader** assignment at the subscription.
- **Effective permissions:**
  - RBAC is **additive**: the union of direct, inherited, and group-based assignments. Reader at the subscription plus Contributor on one resource group = Contributor there, Reader elsewhere.
  - **Deny assignments** beat any allow. Azure creates them (for example, for managed applications); you do not author them on the IAM blade, but you can view them.
  - Unexpected access usually comes from a parent scope or a forgotten group membership.
- **Custom roles**: use one only when no built-in role fits (for example "restart VMs but not create or delete them").
  - Defined in JSON: `Actions`, `NotActions`, `DataActions`, `NotDataActions`, `AssignableScopes`. Effective = Actions minus NotActions.
  - Easiest path: **clone** a built-in role and trim it. Create from IAM > Add > Add custom role, PowerShell, CLI, or an ARM template.
- **Checking access** on the IAM blade:
  - **Check access**: pick a principal and see its effective assignments at this scope, including inherited ones.
  - **Role assignments** tab: every assignment at this scope, marked as direct or **inherited**.
  - Also: **Roles** tab (definitions) and **Deny assignments** tab.
- **Trap:** Azure RBAC and Microsoft Entra roles are separate systems. A Global Administrator does not automatically hold subscription access (they can deliberately **elevate access**). A subscription Owner cannot manage directory users.

📖 Full lesson: [Manage Access to Azure Resources: Azure RBAC (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-rbac-role-assignments/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Manage Azure subscriptions and governance

- **Hierarchy:** management group → subscription → resource group → resource. Policy assignments, role assignments, and locks applied higher up are inherited below.
- **Management groups:**
  - One **root management group** per tenant. Up to six levels beneath it (root and subscription levels not counted).
  - A subscription has exactly one parent management group.
  - Assign company-wide policy at a management group rather than repeating it per subscription.
- **Subscriptions** are a billing boundary and a scope for access and policy. Separate production and development into different subscriptions.
  - Moving a subscription to another management group → it picks up the new parent's policies and role assignments right away (and may suddenly show non-compliant resources).
  - Moving a *resource* between resource groups or subscriptions is a different operation: not every type supports it, and the region never changes.
- **Azure Policy building blocks:**
  - *Policy definition*: one rule (built-ins such as **Allowed locations**, **Require a tag**).
  - *Initiative* (policy set definition): many definitions managed and assigned as one unit.
  - *Assignment*: a definition or initiative applied at a scope, with parameters, an effect, and optional **exclusions**.
  - Evaluates existing resources and every create/update; results appear in the **compliance** view.

| Effect | Result for a non-compliant resource |
|---|---|
| Audit | Allowed, but flagged non-compliant |
| Deny | Create or update is blocked |
| Append | Extra fields added at creation |
| Modify | Properties or tags added, changed, or removed; can remediate |
| DeployIfNotExists | Missing related resource deployed; can remediate |
| Disabled | Assignment kept but not evaluated |

- **Remediation** fixes resources that already exist. Only **deployIfNotExists** and **modify** can remediate. The assignment needs a **managed identity** with a suitable role, then you run a **remediation task**. Deny only stops new violations; it never fixes old ones.
- **Policy vs RBAC:** Policy governs *what* configuration is allowed; RBAC governs *who* can act. A Deny policy stops even an Owner.
- **Resource locks** (subscription, resource group, or resource scope; inherited by children):

| Lock | Read | Modify | Delete |
|---|---|---|---|
| CanNotDelete | Yes | Yes | No |
| ReadOnly | Yes | No | No |

- Locks override RBAC: an Owner cannot delete a CanNotDelete-locked resource. Remove the lock, change, reapply.
- Managing locks needs `Microsoft.Authorization/locks/*` (included in Owner and User Access Administrator).
- **Trap:** ReadOnly can break operations that look like reads but use a write call, such as listing storage account keys.
- **Tags** (name-value pairs, up to 50 per resource) are **not inherited** by default. Enforce with a Require-a-tag policy (Deny) and copy onto existing resources with a **modify** policy plus remediation.
- **Cost control:**
  - **Cost analysis** in Microsoft Cost Management breaks spending down, including by tag.
  - **Budgets** (monthly, quarterly, or annual) raise **alerts** at a percentage of actual or forecast spend. A budget notifies; it does not shut anything down.
  - **Azure Advisor** gives cost recommendations such as right-sizing or shutting down underused VMs.
  - Pricing calculator and TCO calculator estimate costs before deployment.
- **Cues:**
  - "Block resources outside approved regions" → Allowed locations with **Deny**.
  - "Report but don't block" → **Audit**.
  - "Group of policies" → initiative.
  - "Prevent accidental deletion" → **CanNotDelete** lock.
  - "Fix existing non-compliant resources" → remediation with modify or deployIfNotExists.
  - "Notify when spend reaches 80%" → budget alert.

📖 Full lesson: [Azure Subscriptions, Policy, Locks and Governance (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/azure-subscriptions-governance-policy/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

[← Back to the study guide](../README.md)
