# IP addressing and VLAN plan

**Parent planning block:** `192.168.10.0/24`  
**Technique:** Variable Length Subnet Masking (VLSM)  
**Inter-VLAN gateways:** SVIs on Cisco 3560 core switch

| VLAN | Department | Network/CIDR | Mask | Usable host range | Broadcast | Gateway |
|---|---|---|---|---|---|---|
| 10 | Sales | `192.168.10.0/26` | 255.255.255.192 | `192.168.10.1`–`192.168.10.62` | `192.168.10.63` | `192.168.10.1` |
| 20 | Guest | `192.168.10.64/27` | 255.255.255.224 | `192.168.10.65`–`192.168.10.94` | `192.168.10.95` | `192.168.10.65` |
| 30 | IT | `192.168.10.96/27` | 255.255.255.224 | `192.168.10.97`–`192.168.10.126` | `192.168.10.127` | `192.168.10.97` |
| 40 | Administration | `192.168.10.128/28` | 255.255.255.240 | `192.168.10.129`–`192.168.10.142` | `192.168.10.143` | `192.168.10.129` |
| 50 | Finance | `192.168.10.144/28` | 255.255.255.240 | `192.168.10.145`–`192.168.10.158` | `192.168.10.159` | `192.168.10.145` |
| 60 | Servers | `192.168.10.160/28` | 255.255.255.240 | `192.168.10.161`–`192.168.10.174` | `192.168.10.175` | `192.168.10.161` |

The VLAN ranges do not overlap. Addresses `192.168.10.176`–`192.168.10.255` are **not assigned to a VLAN in this design**; any future use needs an explicit subnet plan.

## Server address allocations

| Asset | Address | Purpose | Assignment |
|---|---|---|---|
| Servers VLAN SVI | `192.168.10.161` | Default gateway | Switch SVI |
| DHCP server | `192.168.10.162` | Central DHCP leases | Static |
| DNS server | `192.168.10.163` | Internal name resolution | Static |
| Internal Web server | `192.168.10.164` | Internal HTTP test resource | Static |
| File server | `192.168.10.165` | Internal file services | Static |

*These are the project plan's stated device IP allocations; confirm each in the final Packet Tracer file.*

## DHCP scope records to complete

Do not assume the usable host range equals the actual DHCP pool. The gateway and any statically assigned devices must be excluded. Export the actual server pools and relay settings.

| Scope | VLAN | Start address | End address | Mask | Default router | DNS server | Lease proof |
|---|---|---|---|---|---|---|---|
| Sales | 10 | TODO | TODO | 255.255.255.192 | 192.168.10.1 | Verify configured DNS | Pending |
| Guest | 20 | TODO | TODO | 255.255.255.224 | 192.168.10.65 | Verify configured DNS | Pending |
| IT | 30 | TODO | TODO | 255.255.255.224 | 192.168.10.97 | Verify configured DNS | Pending |
| Administration | 40 | TODO | TODO | 255.255.255.240 | 192.168.10.129 | Verify configured DNS | Pending |
| Finance | 50 | TODO | TODO | 255.255.255.240 | 192.168.10.145 | Verify configured DNS | Pending |

If DHCP also serves other scopes, record them after reviewing the actual server configuration. Confirm which VLAN interfaces contain `ip helper-address 192.168.10.162` (do not assume every interface has it).

## WAN / ISP addresses

**Pending export** from the actual Packet Tracer devices. Record each routed point-to-point subnet, interface IP, external server IP, static/default route and NAT inside/outside boundary. Avoid conflating example address ranges with live public IPs.
