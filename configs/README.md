# Real device configuration exports (pending)

**No Cisco IOS configuration has been generated or claimed here.** Export your **actual** device settings from the final Packet Tracer `.pkt` before publishing.

## Suggested filenames

```text
configs/
├── core-switch.txt
├── edge-router.txt
├── isp-router.txt
└── access-switches/
    ├── sw-sales.txt
    ├── sw-guest.txt
    ├── sw-it.txt
    ├── sw-administration.txt
    ├── sw-finance.txt
    └── sw-servers.txt
```

On each device, inspect `show running-config` (or the Packet Tracer Config tab). Export the authentic, relevant output to its matching file. In addition, capture `show ip route`, `show vlan brief`, `show interfaces trunk`, `show access-lists`, and NAT information wherever applicable.

**Before commit:** redact passwords, password hashes, enable secrets, keys, personal identifiers, and management credentials. If you redact values, label them `[REDACTED]`. Verify that removing confidential values does not conceal relevant technical details needed to understand the network.

`access-switches/` is intentionally empty except for `.gitkeep` until true exports are available.
