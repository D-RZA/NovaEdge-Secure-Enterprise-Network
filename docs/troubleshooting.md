# Troubleshooting report

## Incident NE-SSH-001 — SSH management connection refused

| Field | Recorded information |
|---|---|
| Status | **OPEN / UNRESOLVED** |
| Affected operation | Remote SSH management |
| Source device | Packet Tracer PC; specific VLAN and IP to be recorded |
| Target address | `192.168.10.97` |
| Target identity | Intended VLAN 30/IT default-gateway SVI on the Layer 3 core, **verify** target ownership in running config |
| Attempted command | `ssh -l netadmin 192.168.10.97` |
| Observed response | `% Connection refused by remote host` |
| Timestamp | Not recorded |
| Cause | **Not yet established** |
| Resolution | **Not performed / not confirmed** |

### Observed CLI record

```text
C:\>ssh -l netadmin 192.168.10.97
% Connection refused by remote host
C:\>
```

### Diagnostic checklist — for the next Packet Tracer session

1. Identify which physical or virtual device owns `192.168.10.97` (`show ip interface brief`). Confirm the source PC's own address and VLAN.
2. Check reachability to that management address; interpret ICMP separately from TCP port 22.
3. On the destination device, check whether the IOS image supports SSH and inspect SSH state (for example, `show ip ssh`).
4. Inspect hostname/domain settings, RSA keys, local user accounts and `line vty` configuration. Confirm authentication and `transport input ssh` where supported.
5. Inspect interface ACLs and any `access-class` restrictions that could block authorized management traffic.
6. Retest from the authorized client. Save a successful session screenshot and sanitized configuration evidence if fixed.
7. Update the **root cause, changes made, outcome, and retest timestamp** in this file. Do not mark this closed merely because a config command was entered.

*Possible causes above are diagnostic hypotheses; none has been confirmed.*

### Resolution record (complete after troubleshooting)

| Field | Entry |
|---|---|
| Root cause | TODO |
| Configuration change | TODO |
| Date/time retested | TODO |
| Source client / VLAN | TODO |
| Successful connection evidence path | TODO |
| Outcome | TODO — PASS or FAIL |
| Preventive lesson | TODO |

## Other historical observations

Earlier connectivity and design issues may be described here **only when substantiated** by screenshots, exported commands, or notes. Do not fabricate incident records from memory.

## Recommended incident format

1. Symptom and error message.
2. Expected vs observed behavior.
3. Environment (devices, interfaces, IPs, VLANs).
4. Diagnostic commands and outputs.
5. Root cause, when known.
6. Change made and why.
7. Retest and evidence.
8. Lesson learned.
