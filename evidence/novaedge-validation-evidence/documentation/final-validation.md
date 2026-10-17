# NovaEdge — Final Validation Record

**Scope:** Cisco Packet Tracer secure enterprise network (Cisco 3560 multilayer switch, six Cisco 2960 access switches, edge router, simulated ISP).  The findings below reflect actual user-provided command outputs and screenshots, not simulated or invented results.

## Core architecture

| VLAN | Role | Network | Gateway |
|---|---|---|---|
| 10 | Sales | 192.168.10.0/26 | 192.168.10.1 |
| 20 | Guest | 192.168.10.64/27 | 192.168.10.65 |
| 30 | IT | 192.168.10.96/27 | 192.168.10.97 |
| 40 | Administration | 192.168.10.128/28 | 192.168.10.129 |
| 50 | Finance | 192.168.10.144/28 | 192.168.10.145 |
| 60 | Servers | 192.168.10.160/28 | 192.168.10.161 |

## Validation matrix

| Control or service | Actual observation | Assessment | Evidence |
|---|---|---|---|
| VLANs | VLANs 10–60 active in `show vlan brief` | Passed | Previous core evidence package: `evidence/vlans/01-core-vlans.png` |
| 802.1Q trunking | Core Fa0/2–Fa0/7 forwarding, one VLAN per uplink | Passed | Previous core evidence package: `evidence/vlans/02-core-trunks.png` |
| Layer 3 routing | Six connected VLAN routes, 192.168.10.176/30 routed uplink, default via 192.168.10.178 | Passed | Previous core evidence package: `evidence/routing/03-core-routing-table.png` |
| DHCP Guest | Guest Laptop Wireless0 showed “DHCP request successful” and was assigned 192.168.10.66/27, gateway 192.168.10.65, DNS 8.8.8.8 | Passed after remediation | `evidence/services/11-guest-dhcp-success.png` |
| DHCP Sales | Sales-PC1 DHCP selected; leased 192.168.10.3/26, gateway 192.168.10.1 and DNS 192.168.10.163, reconfirmed on new screenshot | Passed | `evidence/services/05-dhcp-success.png`, `evidence/services/10-sales-dhcp-reconfirmed.png` |
| DNS | `intranet.novaedge.test` resolved to 192.168.10.164; 4/4 replies; screenshot includes historical unsuccessful requests | Passed after remediation | `evidence/services/06-dns-success.png` |
| Internal HTTP | Packet Tracer webpage loaded via http://192.168.10.164 | Passed | `evidence/services/07-internal-web.png` |
| SSH authorized | IT-PC1 pinged 192.168.10.97 and authenticated as netadmin, reaching `SW-Core#` | Passed | `evidence/ssh/ssh-it-success.png` |
| SSH unauthorized | Sales-PC1 SSH to 192.168.10.97 refused | Passed | `evidence/ssh/ssh-sales-blocked.png` |
| Simulated external routing | Sales-PC1 pinged 198.51.100.10 with 4/4 replies on repeated test | Passed | `evidence/routing/08-external-ping-working.png` |
| NAT/PAT | Fresh R1-Edge capture: two TCP translations from inside local 192.168.10.3 to inside global 203.0.113.2 when accessing 198.51.100.10:80; NAT statistics report 2 dynamic translations and 9 hits | Passed (verified again) | `evidence/routing/09-nat-pat-verified.png` |
| Guest isolation | Guest blocked from internal web while external address allowed; ACL deny/permit match counters previously increased | Passed in earlier session; final screenshot recommended | `show access-lists` + paired ping/browser screenshots |
| Sales → Finance ACL | Sales failed to reach Finance; deny counter increased | Passed in earlier session; final screenshot recommended | `show access-lists` + endpoint test |
| Administration → Finance ACL | Blocked; Administration → internal web allowed; deny/permit counters observed | Passed in earlier session; final screenshot recommended | ACL output and paired tests |
| Finance → File ACL | Blocked; Finance → DNS allowed; deny/permit counters observed | Passed in earlier session; final screenshot recommended | ACL output and paired tests |
| Server egress | Web server reached 192.168.10.161 and 192.168.10.1, but could not reach external 198.51.100.10; deny counter 4 | Passed in earlier session; final screenshot recommended | ACL and paired tests |

## Remaining before publication

1. **Guest DHCP verified:** earlier Guest Laptop screenshot had APIPA address `169.254.96.146`, but the later screenshot shows `DHCP request successful`, IPv4 `192.168.10.66/27`, gateway `192.168.10.65`, and DNS `8.8.8.8`. Guest security isolation and simulated Internet access were tested earlier, but may be retested after the DHCP change for final-state assurance.
2. Make sure the current topology diagram has final subnet/gateway labels instead of outdated design annotations.
3. Export **redacted** device configs from SW-Core, six access switches, R1-Edge, and ISP-Router. Do not publish passwords or actual secret values.
4. Save final `.pkt` file and verify it opens successfully.
5. Include final negative/positive ACL screenshots where available; update README claims to distinguish verified results from remaining checks.

> **Simulator scope:** All external IP addresses `203.0.113.0/30` and `198.51.100.0/24` are documentation-only networks used inside Packet Tracer; this lab is not connected to the public Internet.
