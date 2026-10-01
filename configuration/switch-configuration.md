# Switch Configuration (Milestone 2)

Implements the VLAN/trunk/access plan from `../02-design/design-rationale.md`
across the three switches.

## Core-Switch

- VLANs created: 10, 20, 30, 40, 99
- Trunk uplink to Router-Lesedi (802.1Q, all five VLANs)
- Trunk links down to SwitchA-Research and SwitchB-Admin
- Access ports (VLAN 30) for Server-File and Server-App

## SwitchA-Research

- VLAN 10 access ports — PC-Research1, PC-Research2
- VLAN 40 access port — uplink to AP-GUEST (wireless, restricted contractor VLAN)
- Trunk link up to Core-Switch

## SwitchB-Admin

- VLAN 20 access port — PC-Admin1 (confirmed on Fa1/1 via `show vlan brief`)
- VLANs 10, 30, 40, 99 correctly show no ports on this switch — none of
  those end devices are physically connected here
- Trunk link up to Core-Switch

## AP-GUEST (wireless)

- SSID: `Lesedi-Guest`
- Port 1 (Radio): Enabled
- Authentication: Disabled (open) — access restriction is enforced at
  Layer 3 by the router ACL (`router-configuration.md`), not by Wi-Fi
  security, which is sufficient for this assignment's scope
- Connected to SwitchA-Research's VLAN 40 access port using existing
  internal cabling (heritage building constraint, R5 — no new external-wall
  cabling)

## Verification commands used

```
show vlan brief
show interfaces trunk
```

Outputs captured as screenshots in `../screenshots/`.
