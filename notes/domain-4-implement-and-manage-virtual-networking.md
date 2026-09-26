# Domain 4: Implement and manage virtual networking (19%)

This domain tests hands-on network administration: sizing subnets, peering VNets, steering traffic with routes, locking access down with NSGs, Bastion and endpoints, and picking the right DNS and load-balancing service. Most questions hinge on one trigger phrase, so learn the cues as well as the features.

## Configure and manage virtual networks in Azure

- **Address space**: a VNet gets one or more private CIDR blocks (for example `10.0.0.0/16`). Subnets are non-overlapping slices of that range.
- **Plan for overlap early:** a VNet whose range overlaps another VNet or your on-premises network cannot be peered with it.
- **Five reserved addresses per subnet:** network address, default gateway, two mapped to Azure DNS, and broadcast.

| Subnet | Total addresses | Usable |
|---|---|---|
| /24 | 256 | 251 |
| /29 | 8 | 3 |

- **Special subnet names** (the name is the requirement):
  - `AzureBastionSubnet` for Azure Bastion.
  - `GatewaySubnet` for a VPN or ExpressRoute gateway.
- **VNet peering**: private-IP connectivity between two VNets across the Microsoft backbone. No gateway or VPN tunnel sits in the path.
  - *Regional (local) peering*: both VNets are in one region.
  - *Global peering*: the VNets are in different regions.
  - Create a link on **each** side. Status reads *Connected* only once both links exist.
- **Peering is non-transitive.** A↔B plus B↔C gives A no route to C. Fix it with a direct A↔C peering, or send traffic through an NVA in the hub using UDRs.
- **Peering link settings:**

| Setting | Set on | Effect |
|---|---|---|
| Allow virtual network access | Both (on by default) | The two VNets can talk |
| Allow forwarded traffic | Hub, typically | Accepts traffic that an NVA relays for another network |
| Allow gateway transit | Hub (owns the VPN/ExpressRoute gateway) | Lets peers use this gateway |
| Use remote gateways | Spoke | Spoke reaches on-premises through the hub's gateway |

- **Trap:** a spoke that already has its own gateway cannot use remote gateways.
- **Public IP addresses** are standalone resources attached to a NIC, load balancer, VPN gateway, Bastion, or NAT gateway.

| | Standard SKU | Basic SKU |
|---|---|---|
| Allocation | Static only | Static or dynamic |
| Inbound by default | Closed until an NSG allows it | Open |
| Availability zones | Supported | No |
| Status | Use for new workloads; required by Standard Load Balancer | Legacy, on Microsoft's retirement path |

- *Dynamic* addresses are assigned at start and can change after a stop or deallocation. *Static* addresses hold for the resource's life, so pick static for DNS records and firewall allowlists.
- **Routing:** system routes are created automatically and cannot be deleted, only overridden. A **user-defined route (UDR)** sits in a **route table**, and the table is bound to a subnet.
  - Next hop types: Virtual appliance, Virtual network gateway, Virtual network, Internet, None (drops the traffic).
  - The longest matching prefix wins. On a tie between a UDR and a system route, the UDR wins.
- **Force traffic through a firewall/NVA:**
  1. Enable **IP forwarding** on the appliance's NIC (without it, forwarded packets are dropped).
  2. Add a route for `0.0.0.0/0`, next hop *Virtual appliance*, with the appliance's private IP.
  3. Associate the route table with the source subnet.
  4. Confirm with *Effective routes* on a VM's NIC.
- **Troubleshooting with Network Watcher** (a regional service):

| Tool | Answers |
|---|---|
| IP flow verify | Is this 5-tuple allowed, and which rule blocks it? |
| Next hop | Where does this packet go? |
| Effective routes | Which routes actually apply to this NIC? |
| Effective security rules | Which merged NSG rules apply to this NIC? |
| Connection troubleshoot | Can source reach destination end to end, and where does it fail? |

- **Cues:**
  - "A peered to B, B peered to C, A can't reach C" → non-transitive; add peering or route via the hub.
  - "Spokes share the hub's VPN gateway" → gateway transit on hub + use remote gateways on spoke.
  - "Inspect all outbound subnet traffic" → UDR `0.0.0.0/0` to a virtual appliance.

📖 Full lesson: [Azure Virtual Networks, Subnets, Peering & User-Defined Routes](https://www.savemycert.com/revision/azure-administrator-associate/azure-virtual-networks-peering/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Configure secure access to virtual networks

- **Network security group (NSG)**: allow/deny rules for traffic to and from resources.
  - Priority runs **100 to 4096**. The **lower number is evaluated first**, and processing stops at the first match.
  - Each rule matches on source and destination (address plus port) and protocol, the **5-tuple**, plus a direction and an action.
  - Sources and destinations can be IP ranges, **service tags** (`Internet`, `VirtualNetwork`, `AzureLoadBalancer`), or ASGs.
- **Default rules** cannot be deleted, but higher-priority custom rules override them.
  - Inbound: VNet traffic and Azure load balancer traffic are allowed, then `DenyAllInBound`.
  - Outbound: VNet and internet traffic are allowed, then everything else is denied.
  - So any inbound service (RDP, SSH, HTTPS) needs an explicit allow rule.
- **Where to attach an NSG:**
  - *Subnet*: broad rules for every resource in the tier.
  - *NIC*: an exception for one VM.
  - *Both*: the traffic must clear both.

| Direction | Evaluated first | Then |
|---|---|---|
| Inbound to a VM | Subnet NSG | NIC NSG |
| Outbound from a VM | NIC NSG | Subnet NSG |

- **Rule of thumb:** first match by priority *within* an NSG, most restrictive *across* the pair. A deny in either one drops the packet.
- **Trap:** an allow added to the NIC NSG does nothing while the subnet NSG still denies that flow.
- **Effective security rules** (on the NIC, or via Network Watcher) merges both NSGs and names the rule that allows or blocks a flow. Use it instead of reading two rule lists by hand.
- **Application security group (ASG)**: a label for VMs by role (`WebServers`, `DbServers`), used as a source or destination in NSG rules instead of IPs.
  - Put a new VM's NIC into the ASG and it picks up every rule that references the group, with no rule edits.
  - The NICs in the ASGs a rule uses must be in the same VNet.
  - ASGs define *which machines*. The NSG still defines *what traffic*.
- **Azure Bastion**: managed RDP/SSH to VMs from the portal over TLS (port 443). Workload VMs keep **no public IP**, and ports 3389/22 stay closed to the internet.
  - Deploy into a subnet named exactly `AzureBastionSubnet`, at least `/26`.
  - Bastion has its own Standard public IP. Workload VMs keep private IPs only.
  - The VM's NSG must still allow RDP/SSH **from the Bastion subnet range**.
- **Securing PaaS access** (Storage, SQL Database):

| | Service endpoint | Private endpoint |
|---|---|---|
| Mechanism | Extends the subnet's identity to the service over the backbone | Private Link puts a NIC with a private IP from your subnet in front of the service |
| Service's public endpoint | Remains | Can be disabled entirely |
| Service firewall | Restricted to the chosen subnet | Public network access turned off |
| DNS | No change | Pair with a private DNS zone so the hostname resolves privately |
| Cost | Free | Billed per endpoint and data |

- **Cues:**
  - "Connect to a VM without a public IP / without exposing it" → Azure Bastion.
  - "Group VMs by role in NSG rules instead of IPs" → application security group.
  - "Give the storage account a private IP in the VNet" or "no public exposure" → private endpoint.
  - "Keep traffic off the internet, keep it simple and free" → service endpoint.

📖 Full lesson: [Azure NSGs, Bastion, Service Endpoints & Private Endpoints](https://www.savemycert.com/revision/azure-administrator-associate/azure-nsg-bastion-private-endpoints/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

## Configure name resolution and load balancing

- **Azure DNS** hosts records for domains you already own. It is not a registrar.
- **Public DNS zone**: answers internet queries for a domain such as `contoso.com`.
  - Records are grouped into record sets by name and type. Know A (IPv4), CNAME (alias), MX (mail), and TXT (verification, SPF).
  - **Delegation:** Azure assigns the zone four name servers. Update the domain's NS records at the registrar to point at them.
- **Azure-provided DNS** (`168.63.129.16`) resolves VM names inside **one** VNet only, with a fixed Azure suffix.
- **Private DNS zone**: custom-domain resolution inside and across VNets, never exposed to the internet.
  - Connect it to each VNet with a **virtual network link**. Every linked VNet can resolve the zone.
  - **Autoregistration** on a link makes Azure create, update, and remove an A record for each VM in that VNet. Without it, you manage records by hand.
- **Azure Load Balancer**: Layer 4, TCP/UDP, regional. It never inspects URLs, headers, or cookies.

| Component | Role |
|---|---|
| Frontend IP configuration | Address clients connect to (public or private) |
| Backend pool | VMs or scale-set instances receiving traffic |
| Health probe | Decides which backends are eligible |
| Load-balancing rule | Maps frontend IP:port to backend port, tied to a probe |
| Inbound NAT rule | Forwards one frontend port to one specific VM (for example, direct RDP/SSH) |

- **Distribution:** a five-tuple hash by default. Switch to **session persistence** (source IP affinity, two- or three-tuple) when a client must keep hitting the same backend.
- **Public vs internal:** identical components. Only the frontend differs.
  - Public = public IP frontend, internet-facing tier, also gives backends outbound connectivity.
  - Internal = private IP from a subnet, reachable only inside the network, fronting an app or database tier.
- **SKUs:**
  - *Standard*: zonal or zone-redundant, larger pools, HA ports, outbound rules, better diagnostics, and **secure by default**. Without an NSG rule allowing the application traffic, clients get nothing.
  - *Basic*: legacy, no zone redundancy, being retired. Don't choose it for new designs.
- **Pick the service:**

| Need | Service |
|---|---|
| Any TCP/UDP within a region | Azure Load Balancer |
| URL path or host routing, TLS termination, WAF, in one region | Application Gateway |
| Route users to the best regional endpoint via DNS (priority, weighted, performance, geographic) | Traffic Manager |
| Global HTTP(S) entry with CDN, WAF, and edge TLS | Azure Front Door |

- **Troubleshooting:** backends that fail the health probe drop out of rotation. If all of them fail, nothing is distributed.
  1. Does the app listen on the **probe port**, and does an HTTP(S) probe **path** return 200?
  2. Does the NSG allow the **`AzureLoadBalancer`** service tag on the probe port? (Probes come from `168.63.129.16`.)
  3. Is the backend VM running, with its service started and host firewall open?
- **Classic break:** the app moved to a new port but the probe still checks the old one, so every instance goes unhealthy.
- **Cues:**
  - "Resolve VM names privately, including across peered VNets" → private DNS zone + VNet link (+ autoregistration).
  - "Host the domain's records, change name servers at the registrar" → public zone + delegation.
  - "Balance internal TCP traffic to a private app tier" → internal load balancer.
  - "Backend removed from rotation" → health probe.

📖 Full lesson: [Azure DNS and Load Balancer: AZ-104 Networking Guide](https://www.savemycert.com/revision/azure-administrator-associate/azure-dns-load-balancer/?utm_source=github&utm_medium=readme&utm_campaign=az-104-study-guide)

[← Back to the study guide](../README.md)
