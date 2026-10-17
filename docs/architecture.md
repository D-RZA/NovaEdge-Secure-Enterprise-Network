# Network architecture

## Scope and design goal

Build a simulated small-enterprise network for NovaEdge Services Ltd to demonstrate department segmentation, centralized Layer 3 switching, shared server services, controlled WAN access, and ACL-enforced segmentation.

## Logical topology

**Simulated ISP router** → **Cisco 2911 enterprise edge router** → **Cisco 3560 Layer 3 core switch** → **six Cisco 2960 access switches**, one each for Sales, Guest, IT, Administration, Finance, and Servers. The simulated ISP network also hosts an **external web server**. Departmental endpoints connect via their access switches; Guest additionally uses an access point and wireless clients. The Servers VLAN holds DHCP, DNS, Internal Web, and File servers.

> Physical port IDs, model variants, ISP subnets, and device-to-device link addressing must be copied from the final `.pkt` file. They are not reconstructed here from assumptions.

## Design choices

| Layer / component | Purpose | Implementation reported by project owner |
|---|---|---|
| Access (six 2960s) | Associate endpoint access ports with department VLANs; trunk traffic to core | Access and trunk ports configured and tested |
| Core (3560) | Provide each department's default-gateway SVI and route between VLANs | SVIs and IP routing configured and tested |
| Shared services (VLAN 60) | Central DHCP, DNS, internal web and file services | DHCP relay and services reported tested |
| Edge (2911) | Connect enterprise routing domain to simulated ISP, perform NAT/PAT | Static routes and NAT/PAT reported tested |
| Security | Filter prohibited inter-VLAN or outbound flows | Guest, Sales/Finance, server outbound, Admin/Finance ACLs reported tested; evidence pending |
| Device management | SSH-based remote administration | **Not yet working:** connection refused |

## Traffic paths to verify

1. **Same VLAN:** An endpoint talks directly to another endpoint in its VLAN using Layer 2 switching.
2. **Cross VLAN:** A departmental endpoint sends traffic to its VLAN SVI on the 3560; the core applies the relevant ACL policy and routes to the destination subnet if permitted.
3. **DHCP:** Endpoints in non-server VLANs reach the DHCP server through configured DHCP relay on the relevant gateway SVIs. Record real `ip helper-address` statements during export.
4. **External:** Permitted internal traffic traverses the core, enterprise edge router and NAT/PAT toward the simulated ISP test environment.
5. **Protected server traffic:** Defined outbound server traffic should be denied at the verified enforcement point. Exact restrictions and exceptions must be transcribed from the configuration.

## Validation dependencies

- Address/VLAN correctness: [IP plan](ip-addressing.md)
- Required and forbidden flows: [Security policy](security-policy.md)
- Commands, pass criteria and screenshot references: [Validation plan](validation-plan.md)
- Unresolved failure: [SSH troubleshooting](troubleshooting.md)

## To fill from final project

- [ ] Diagram export from the authoritative topology → `../diagrams/network-topology.png`
- [ ] Actual uplink port mapping (access switch ↔ core, core ↔ edge, edge ↔ ISP)
- [ ] Routed link addressing and any ISP/external-server IPs
- [ ] Routing details: default routes and any reverse routes
- [ ] ACL numbers/names and actual application interface/direction
- [ ] NAT inside/outside interface placement and translated address evidence
