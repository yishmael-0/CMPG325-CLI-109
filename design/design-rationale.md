# Design Rationale — CLI-109

> Diagrams `physical-topology.png` and `logical-topology.png` belong in this
> folder alongside this file. See the repository README for how they're
> referenced.

## Physical design summary

| Device | Role | Notes |
|---|---|---|
| 1x Router | Router-on-a-Stick — inter-VLAN routing | Single physical link (Gig0/0) to core switch, configured with 5 sub-interfaces (.10 .20 .30 .40 .99), each an 802.1Q encapsulated gateway. |
| 1x Core Switch | Trunk aggregation | Trunk port to router; trunk/access ports to access switches, server room devices and AP. |
| 2x Access Switches | Access layer | Switch A (Research wing): VLAN 10 access ports + uplink for the wireless AP. Switch B (Admin office): VLAN 20 access ports. |
| 1x Wireless Access Point | Guest/contractor wireless | Connected to an existing internal access-switch port (VLAN 40 access port). No new cable run through external/heritage walls — reuses existing internal cabling. |
| End devices | Researcher PCs, Admin PCs, Servers, contractor laptop/phone | Distributed across VLANs 10, 20, 30 and 40 as per the logical topology. |

### Heritage building constraint (R5)

The building is heritage-listed, so no new cabling may be run through
external walls. This is addressed in two ways:

- The wireless AP for the restricted contractor/guest VLAN is mounted
  indoors and connected to an existing internal access-switch port using
  existing internal cabling — wireless coverage extends access into the
  building without any new cable penetrating an external wall.
- All other new connectivity (router–switch trunk, inter-switch links) is
  likewise routed through existing internal risers/conduits rather than
  through external walls.

## Logical design summary

Five VLANs separate traffic by function. All inter-VLAN traffic is routed
by the single router over one physical trunk link (Router-on-a-Stick),
using one 802.1Q sub-interface per VLAN as the default gateway for that
VLAN. Full VLAN/subnet table is in `03-ip-addressing/vlsm-plan.md`.

### Why Router-on-a-Stick is appropriate here

- The client is a single-site research group with a modest number of VLANs
  (five) and moderate traffic volumes — a single routed trunk link
  comfortably carries inter-VLAN traffic without needing a multilayer
  switch.
- It satisfies the assigned technical challenge directly: one router
  interface, sub-divided into logical sub-interfaces, each acting as the
  gateway for one VLAN.
- It keeps the design inexpensive and simple to verify in Packet Tracer,
  while still meeting the segmentation (R2) and inter-VLAN routing
  requirements.

### How restricted guest access will be enforced

VLAN 40 (Guest-Wi-Fi) hosts obtain the Gig0/0.40 sub-interface
(172.30.74.225) as their gateway like any other VLAN, but the router will
carry an access control list on that sub-interface that permits traffic
out to the Internet/ISP only and denies traffic destined for VLAN 10, 20,
30 or 99. This will be configured and verified in Milestone 2; it is noted
here because it directly shapes the logical topology and IP plan (VLAN 40
is a distinct, isolated subnet for exactly this reason).
