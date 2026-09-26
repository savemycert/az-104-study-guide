# Domain 2: Implement and manage storage (19%)

Storage questions test configuration choices on a storage account: how callers reach and authorize against it, how many copies it keeps and where, and which Blob or Files feature protects or tiers the data.

## Configure access to storage

- Split every access question in two:
  - **Network**: can the caller reach the endpoint? (firewall, virtual network rules, private endpoint)
  - **Authorization**: is the caller allowed? (access key, SAS, Microsoft Entra ID + RBAC)
  - Both must pass. A valid SAS from a blocked network is still refused.
- **Storage firewall** (Public network access setting):
  - *Enabled from all networks*: the default.
  - *Enabled from selected virtual networks and IP addresses*: add **virtual network rules** (subnets with a `Microsoft.Storage` **service endpoint**) and/or **IP rules** (public IPv4 CIDR ranges only; private RFC 1918 ranges are not accepted).
  - *Disabled*: typical when you use a **private endpoint**, which gives the account a private IP inside your VNet.
  - Keep **Allow trusted Microsoft services** on so services such as Azure Backup, Azure Monitor, and Event Grid still get through.
  - Cues: "only from this subnet" → VNet rule + service endpoint. "Only our office IP range" → IP rule.
- **Access keys** (key1, key2):
  - Each grants full control of the whole account. They cannot be scoped to a container or made read-only.
  - Two keys enable zero-downtime rotation: move apps to key2, regenerate key1, repeat later the other way.
  - Regenerating a key **invalidates every account and service SAS signed with it**. That is a risk and also a last-resort "revoke everything".
  - Store keys in **Azure Key Vault**; prefer RBAC or SAS for daily use.
- **Shared access signature (SAS)**: a signed query string on a URL. It defines permissions (read, write, delete, list, add, create), start and expiry, an optional IP range, the allowed protocol (HTTPS only, or HTTPS and HTTP), and the resources it covers.

| SAS type | Signed by | Covers |
|---|---|---|
| Account SAS | Account key | Several services and account-level operations |
| Service SAS | Account key | One service, specific resources; can use a stored access policy |
| User delegation SAS | Key issued to a Microsoft Entra identity | Blob storage only |

- **Stored access policy**: a named policy on a container, share, queue, or table that holds the permissions and expiry for a service SAS.
  - To revoke, delete or rename the policy or move its expiry into the past. Every SAS referencing it dies with no key rotation.
  - Edit the policy to change or extend all linked SAS tokens at once.
  - Up to **five** per container.
  - A user delegation SAS is revoked through its Entra identity instead.
- **Most to least secure:** Entra ID + RBAC → user delegation SAS → service SAS (with stored access policy) → access keys.
- **Identity-based access for Azure Files (SMB):**
  - Directory sources: on-premises **AD DS** (synced with Entra Connect), **Microsoft Entra Domain Services**, or **Microsoft Entra Kerberos** for hybrid identities.
  - **Share level** = Azure RBAC roles (Storage File Data SMB Share Reader / Contributor / Elevated Contributor).
  - **File and folder level** = Windows NTFS ACLs. Effective access is where both allow.
  - Users sign in with domain credentials; nobody receives the account key.
- **Scenario pattern:** a partner with no Entra identity needs read-only access to one container for two weeks, revocable early → **stored access policy** on the container (read + list, expiry) + **service SAS** that references it. Optionally add an IP range and HTTPS only.

📖 Full lesson: [Configure Access to Azure Storage: SAS, Keys & Firewalls (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-access-sas/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Configure and manage storage accounts

- One storage account = one namespace for blobs, files, queues, and tables. Decide account kind, performance, and redundancy at creation, along with subscription, resource group, and region.
- **Account kinds:**
  - **General-purpose v2**: the default. All services and all access tiers.
  - Legacy: general-purpose v1 (no access tiers), BlobStorage.
  - Premium kinds: premium block blobs, premium file shares (FileStorage), premium page blobs.
- **Performance:** *Standard* for most workloads at the lowest cost. *Premium* (SSD) for low latency and high transaction rates. Premium is an account kind you choose at creation, not a later toggle.
- Default answer unless the scenario says otherwise: **GPv2 + Standard**.
- **Redundancy** (every option keeps at least three copies):

| Option | Primary region | Secondary region | Survives |
|---|---|---|---|
| LRS | 3 copies in one datacenter | None | Disk or server failure |
| ZRS | 3 copies across 3 availability zones | None | Zone / datacenter outage |
| GRS | LRS | Async copy to paired region | Region outage |
| GZRS | ZRS | Async copy to paired region | Zone and region outage |
| RA-GRS / RA-GZRS | As GRS / GZRS | Readable at any time | Same, plus reads from secondary |

- Plain GRS and GZRS secondaries are **not readable** until a failover. Only the **RA-** variants expose a `-secondary` endpoint for reads.
- The secondary is **eventually consistent** (asynchronous replication) and read-only. Check **Last Sync Time** to see how far behind it is.
- Choosing: "stay in one region, survive a datacenter loss" → ZRS. "Survive a region outage" → GRS/GZRS. "…and read from the secondary" → RA-GRS/RA-GZRS. Add zone protection in the primary → the GZ variants.
- **Object replication**: asynchronous copy of **block blobs** between two storage accounts you own (can be different regions or subscriptions).
  - Prerequisites: **blob versioning on both accounts**, **change feed on the source**.
  - A policy holds rules mapping source container → destination container, with an optional prefix filter and a choice to include existing blobs.
  - Not for files, queues, or tables. Not the same as redundancy, which Azure manages inside one account.
- **Encryption at rest**: Storage Service Encryption, 256-bit AES, always on, cannot be disabled.
  - *Microsoft-managed keys*: the default.
  - *Customer-managed keys*: your key in **Azure Key Vault** or Managed HSM, so you control rotation and revocation.
  - *Infrastructure encryption*: a second encryption layer with a separate key. It can be enabled **only at account creation**.
- **Moving data:**
  - **Azure Storage Explorer**: free desktop GUI (Windows, macOS, Linux) for browsing, uploading, and managing policies. Connect with Entra sign-in, key, SAS, or connection string.
  - **AzCopy**: command line for scripted and bulk transfers, including account-to-account copies. Authenticates with SAS or Entra, resumes interrupted jobs, and powers Storage Explorer's transfers.

📖 Full lesson: [Configure & Manage Azure Storage Accounts and Redundancy (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-storage-accounts-redundancy/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Configure Azure Files and Azure Blob Storage

- **Which service:** a shared drive that many machines mount over SMB or NFS → **Azure Files**. Unstructured objects accessed over HTTP(S) (media, backups, logs, static web content) → **Blob Storage**. Both share the account's name, redundancy, and keys.
- **Blob types** (set per blob at upload, not per container): *block* (most files), *append* (logging, add to the end only), *page* (backs VM disks).
- **File shares** (Data storage > File shares > + File share):
  - Protocol: **SMB** (Windows, Linux, macOS) or **NFS** (Linux; needs a premium FileStorage account).
  - **Quota** caps share size in GiB.
  - Mount from Windows via `\\<account>.file.core.windows.net\<share>` (the portal generates the script). Linux uses `cifs` for SMB.
  - Trap: SMB uses outbound **port 445**. On-premises networks that block it cannot mount.
  - Standard shares run on GPv2 (HDD). Premium shares run on FileStorage (SSD) for IOPS-heavy work.
- **Protecting Azure Files:**
  - **Share snapshot**: a read-only, incremental, point-in-time copy of the whole share. Restore one file or the whole share. Windows users see snapshots as **Previous Versions**.
  - **Soft delete for file shares**: a deleted share is kept for a retention period (1–365 days) and can be undeleted.
  - Snapshots recover files; soft delete recovers a deleted share.
- **Azure File Sync**: syncs an Azure file share with on-premises Windows Servers.
  - Components: **Storage Sync Service** → **sync group** → one **cloud endpoint** (the share) + one or more **server endpoints** (paths on servers running the agent).
  - **Cloud tiering** keeps hot files on the server and replaces cold files with pointers (reparse points) that are recalled on open. Governed by a **volume free-space policy** and an optional **date policy**.
- **Containers** (Data storage > Containers > + Container), public access level:

| Level | Anonymous read | Anonymous list |
|---|---|---|
| Private | No | No |
| Blob | Yes, by exact URL | No |
| Container | Yes | Yes |

- Trap: the account-level **Allow Blob anonymous access** setting is off by default on new accounts and overrides container settings. Keep containers private; share with SAS.
- **Access tiers:**

| Tier | Online? | Storage cost | Access cost | Minimum retention |
|---|---|---|---|---|
| Hot | Yes | Highest | Lowest | None |
| Cool | Yes | Lower | Higher | 30 days |
| Cold | Yes | Lower still | Higher still | 90 days |
| Archive | **No** | Lowest | Highest | 180 days |

- Moving or deleting a blob before its minimum retention incurs an **early-deletion charge**.
- An account-level default tier applies to new blobs. Archive is set per blob or by a lifecycle rule, never as the account default.
- **Rehydrating archive:** an archived blob cannot be read, copied, or snapshotted until it is rehydrated.
  - Two ways: change its tier to hot, cool, or cold, or copy it to a new blob in an online tier.
  - Priority: **Standard** (can take up to about 15 hours) or **High** (faster, costs more).
- **Blob data protection:**
  - **Blob soft delete**: recovers deleted (or overwritten) blobs within the retention window.
  - **Container soft delete**: recovers a deleted container.
  - **Blob versioning**: every overwrite saves the prior state as a version with its own ID.
  - Soft delete undoes deletions; versioning undoes overwrites. Enable all three on important data.
- **Lifecycle management**: a policy of rules filtered by prefix or blob index tags. Actions (`tierToCool`, `tierToCold`, `tierToArchive`, `delete`) fire on days since last modified or last accessed, and can target versions and snapshots separately. Evaluated once a day.
  - Pattern: cool at 30 days, archive at 90, delete at 365.
- **Cues:**
  - "Must rehydrate before reading" → archive.
  - "Move blobs to cheaper tiers as they age" → lifecycle management.
  - "Recover a deleted blob" → soft delete.
  - "Keep the previous copy on overwrite" → versioning.
  - "Cache an Azure share on an on-premises server" → Azure File Sync.

📖 Full lesson: [Configure Azure Files and Azure Blob Storage (AZ-104)](https://www.savemycert.com/revision/azure-administrator-associate/configure-azure-files-blob-storage/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

[← Back to the study guide](../README.md)
