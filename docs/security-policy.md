# Security policy and ACL documentation

## Important status distinction

The project owner reports several ACL-based controls as **implemented and tested**, but this scaffold does not contain the actual ACL statements, application points, or screenshots. Treat the descriptions below as **intended or reported controls** until configuration evidence and test results are archived. SSH management is **unfinished**.

## Security objectives and verification matrix

| Source | Destination | Security objective | Reported implementation | Evidence needed |
|---|---|---|---|---|
| Guest VLAN 20 | Enterprise internal VLANs | Deny guest lateral access (allow any explicitly necessary exceptions only if documented) | Guest isolation reported tested | Guest-to-internal negative tests; applied ACL lines |
| Guest VLAN 20 | Simulated external web server / Internet | Permit intended guest external traffic | Reported tested as part of isolation/NAT | External test result and NAT evidence |
| Sales VLAN 10 | Finance VLAN 50 | Deny cross-department traffic | Restriction reported tested | Source/destination test, relevant ACL |
| Servers VLAN 60 | Outbound destinations covered by server egress policy | Restrict outbound server traffic | Restrictions reported tested | Actual destination/port scope, applied ACL, negative/positive tests |
| Administration VLAN 40 | Protected networks/services specified by configured ACL | Enforce Administration policy | ACLs reported tested | Exact ACL statements and traffic tests |
| Finance VLAN 50 | Protected networks/services specified by configured ACL | Enforce Finance policy | ACLs reported tested | Exact ACL statements and traffic tests |
| IT management host(s) | Network device SSH service | Permit authenticated SSH management from authorized sources only | **Unresolved** | Successful SSH test, remote auth and VTY settings |

## Avoid overclaiming

The following specifics are **not yet documented** and must not be inferred:

- Exact ACL command syntax, ACL names/numbers, permit/deny order, and protocol/port matches.
- The actual interface and `in`/`out` direction where each ACL is attached.
- Which Administration/Finance flows are explicitly permitted or denied.
- Which server egress destinations/protocols are blocked, and which remain permitted.
- Any default-deny policy, logging, switch port security, or successful SSH hardening beyond what real configs show.

## ACL evidence template

Complete one entry for **each deployed ACL**:

| Field | Value |
|---|---|
| Device | TODO |
| ACL name/number | TODO |
| Purpose | TODO |
| Applied interface | TODO |
| Direction | TODO (`in` / `out`) |
| Exact sanitized ACEs | TODO |
| Positive test (should work) | TODO + evidence path |
| Negative test (should fail) | TODO + evidence path |
| Notes/known limitations | TODO |

Useful commands (depending on Packet Tracer IOS support):

```text
show access-lists
show running-config
show ip interface
show ip nat statistics
show ip nat translations
```

Ensure exported configs and screenshots are reviewed for passwords, credentials and personal data before committing them.
