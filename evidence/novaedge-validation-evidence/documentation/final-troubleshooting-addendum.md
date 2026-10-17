# Troubleshooting Addendum — DHCP/DNS and SSH

## Issue A — Sales DHCP lease failed after applying ACLs

**Symptom:** Sales-PC1 showed `DHCP request failed` and had no valid IPv4 address after it was switched from static to DHCP.

**Evidence:** User-provided DHCP Server screenshot showed active SALES pool with gateway `192.168.10.1`, DNS `192.168.10.163`, starting IP `192.168.10.2`, mask `/26` and maximum 61 users.

**Diagnosis:** DHCP discovery uses a `0.0.0.0` source and broadcasts to `255.255.255.255` using UDP 68 → 67. The inbound SALES-SECURITY ACL permitted traffic sourced from `192.168.10.0/26` but lacked an explicit rule for the DHCP discovery's unconfigured source. This was the likely cause of the failure.

**Change applied:** An exception was added to SALES-SECURITY using `permit udp any eq 68 any eq 67` while retaining Sales-to-Finance restrictions.

**Verification:** Sales-PC1 subsequently obtained `192.168.10.3/26`, default gateway `192.168.10.1` and DNS `192.168.10.163` via DHCP (screenshot in `evidence/services/05-dhcp-success.png`).

## Issue B — DNS name lookup failed before DHCP remediation

**Symptom:** `ping intranet.novaedge.test` reported host not found. The internal web page remained reachable by IP.

**Resolution and verification:** After Sales-PC1 obtained the correct DHCP-provided DNS address `192.168.10.163`, the name `intranet.novaedge.test` resolved to `192.168.10.164`, and all four ICMP replies succeeded (screenshot in `evidence/services/06-dns-success.png`). This confirms a successful final name-resolution test; the earlier failure alone did not prove the DNS server was faulty.

## Issue C — SSH refused during initial attempts

**Symptom:** `ssh -l netadmin 192.168.10.97` initially reported connection refused.

**Verification after SSH setup:** `show ip ssh` reported SSH v2 enabled. IT-PC1 successfully authenticated to SW-Core and reached `SW-Core#`, while Sales-PC1's SSH attempt was refused. The resulting tests demonstrate restricted administrative login as configured.

**Security disclosure:** Redact actual credentials and secrets before publishing configuration exports. It is acceptable to show the login prompt and hostname without exposing passwords.

## Issue D — NAT translation table appeared empty during verification

**Symptom:** `show ip nat translations` displayed no active translations during an idle check; `show ip nat statistics` reported 0 active translations.

**Investigation:** R1-Edge running configuration showed correct `ip nat inside` / `ip nat outside` assignments, NAT ACL 1 matching client networks and `ip nat inside source list 1 interface GigabitEthernet0/0 overload`. Reconfiguring NAT was not required.

**Verification:** After generating HTTP traffic from Sales-PC1, R1-Edge displayed two TCP translations from `192.168.10.3` to `203.0.113.2` for destination `198.51.100.10:80`. NAT statistics showed two dynamic translations and nine hits. Screenshot: `evidence/routing/09-nat-pat-verified.png`.

## Resolved issue — Guest DHCP lease

**Symptom:** Earlier Guest Laptop DHCP screenshot displayed `169.254.96.146/16` (APIPA), no gateway and no DNS. This indicates Guest DHCP did not complete at that time.

**Resolution/verification:** After the Guest DHCP ACL exception was applied, the Guest Laptop showed “DHCP request successful” on Wireless0, with address `192.168.10.66/27`, gateway `192.168.10.65` and DNS `8.8.8.8`. Capture: `evidence/services/11-guest-dhcp-success.png`. The observed result verifies successful DHCP assignment. A repeat of Guest internal-block/external-allow tests after the change is optional additional final-state assurance.
