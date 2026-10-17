# Final validation and evidence plan

## Acceptance rules

**PASS** means the test was rerun, the observed result matched the expected result, and a meaningful screenshot or command output is stored in the repository. **FAIL** means an observed mismatch. **PENDING** means no final evidence has been archived. **REPORTED** means the project owner previously indicated success but final proof is not attached.

Record outcomes in [`../evidence/test-results/validation-matrix.csv`](../evidence/test-results/validation-matrix.csv), replacing *REPORTED* or *PENDING* with factual results and adding evidence paths. Do not edit outcomes based solely on expected behavior.

## Check sequence

| ID | Area | Procedure / command | Expected outcome | Status at scaffolding |
|---|---|---|---|---|
| T01 | VLAN segmentation | On core/access switches: `show vlan brief` | Correct VLAN IDs/access ports | REPORTED |
| T02 | Trunks | `show interfaces trunk` | Correct trunks and allowed VLANs | REPORTED |
| T03 | SVI configuration | Core: `show ip interface brief` | All expected VLAN gateways up/up | REPORTED |
| T04 | L3 routing | Core: `show ip route` | Directly connected subnets and needed routes | REPORTED |
| T05 | DHCP | Renew/show IPv4 settings on one client per DHCP VLAN | IP, mask, gateway and DNS correspond to design | REPORTED |
| T06 | DHCP relay | Check real SVI helper statements / DHCP allocation | Non-server VLAN clients obtain leases | REPORTED |
| T07 | Internal DNS | Resolve configured internal name from authorized client | Expected answer / IP | REPORTED |
| T08 | Internal web | Open internal web service from authorized client | Page loads | REPORTED |
| T09 | NAT/PAT | Generate allowed external traffic; inspect router NAT translations | Translation and external reachability | REPORTED |
| T10 | Sales → Finance | Ping/appropriate test from Sales PC to Finance PC | Denied | REPORTED |
| T11 | Guest → internal | Test guest endpoint against internal web or protected endpoint | Denied | REPORTED |
| T12 | Guest → external | Guest browser to external server | Allowed | REPORTED |
| T13 | Server egress | Test the **documented** denied and allowed server flows | ACL policy enforced | REPORTED; scope to document |
| T14 | Admin/Finance policies | Test exact forbidden/allowed flows from real ACLs | ACL policy enforced | REPORTED; scope to document |
| T15 | SSH management | From authorized management client: `ssh -l netadmin 192.168.10.97` | Authenticated remote CLI session | **FAIL — connection refused** |
| T16 | Final regression | Repeat required positive and negative tests and inspect configs | No regressions | PENDING |

## Evidence naming standard

Save cropped but context-preserving images in `../evidence/screenshots/`, for example:

- `01-final-topology.png`
- `02-core-svis.png`
- `03-vlan-trunks.png`
- `04-dhcp-lease-sales.png`
- `05-dns-resolution.png`
- `06-internal-web.png`
- `07-nat-translations.png`
- `08-guest-internal-denied.png`
- `09-sales-finance-denied.png`
- `10-ssh-management-success.png` **(do not add or label success until proven)**
- `11-server-egress-acl.png`
- `12-admin-finance-acls.png`

Also export plain-text read-only output, where useful, into `evidence/test-results/` with filenames that correspond to test IDs.

## Commands to consider

```text
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show access-lists
show ip nat translations
show ip nat statistics
show ip ssh
show running-config
```

Packet Tracer support for individual IOS commands depends on device type and simulated image; use equivalent supported commands where necessary.

## Final reviewer checklist

1. Verify a real `.pkt` file opens in Cisco Packet Tracer.
2. Confirm the repository diagram matches that file.
3. Confirm six correct VLANs, gateways and static server IPs.
4. Cross-check each security claim against the deployed ACLs and traffic evidence.
5. Verify **both allowed and denied** flows; a timeout alone can result from many causes.
6. Close the SSH failure in the troubleshooting log, or publish with an explicit known limitation.
7. Ensure confidential data are not in any configuration export or screenshot.
