# Scope Notes — CLI-109

The client change request is worded specifically as **"limited wireless
access"** for after-hours contractors — not general guest Wi-Fi, not staff
mobility, and not a public hotspot. The design is deliberately scoped to
that wording, per the brief's instruction not to introduce additional scope
unnecessarily.

## In scope (per brief wording)

- VLAN segmentation (research / admin / servers)
- Router-on-a-Stick inter-VLAN routing
- A wireless AP delivering restricted guest/contractor access
- IP addressing from 172.30.74.0/23
- Evidence and testing of the above

## Explicitly out of scope for this project

- Additional VLANs or additional sites
- Full NAC / 802.1X guest portal
- Redundancy / HA routing (HSRP/VRRP)
- Dynamic routing protocols
- Any wired guest network

None of the above is asked for in the brief and each is intentionally
excluded to avoid unnecessary scope creep.
