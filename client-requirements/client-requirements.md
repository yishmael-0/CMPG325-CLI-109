# Client Requirements : CLI-109 (Lesedi Renewable Energy Research Group)

Requirements below are drawn directly from the CMPG325 assignment brief for
CLI-109. Each is traced back to where it comes from so nothing is added
beyond what the brief specifies.

| Ref | Requirement | Source / Justification |
|---|---|---|
| R1 | Network must use the assigned address block 172.30.74.0/23 for all internal addressing. | Assignment brief — fixed allocation for CLI-109. |
| R2 | Traffic must be logically segmented by function (research staff, administration, servers/lab equipment) to contain broadcast domains and separate research data from general office traffic. | Inferred from client being a research organisation with distinct staff and lab/server functions; standard practice for a research network handling project data. |
| R3 | Inter-VLAN communication must be routed through a single router using sub-interfaces (Router-on-a-Stick), one sub-interface per VLAN on an 802.1Q trunk. | Assigned technical challenge, mandatory, intermediate difficulty. |
| R4 | After-hours cleaning and security contractors require wireless network access, but that access must be limited (not full access to research/admin/server resources). | Client change request — explicit wording: "limited wireless access", so scope is restricted to what this phrase requires. |
| R5 | No new cabling may be run through the building's external walls. | Heritage-listed building constraint. Any new connectivity (e.g. for the guest wireless AP) must use existing internal cable paths/risers or be delivered wirelessly from an AP fed by existing internal cabling. |
| R6 | The design must be fully implementable and testable in Cisco Packet Tracer, producing a working .pkt file. | Assignment deliverable requirement. |
| R7 | End-to-end connectivity must be demonstrable: intra-VLAN connectivity for all staff, inter-VLAN connectivity where appropriate (e.g. staff to servers), and correctly restricted connectivity for the guest VLAN. | Assignment testing requirement + R4 (limited access implies some paths must fail by design). |

## Note on assumptions

Host-count sizing behind the IP addressing plan (see `03-ip-addressing/`) was
reasoned from the brief and typical research-group scale, not confirmed by
the client directly, the brief gives no explicit user counts. This is
flagged here rather than presented as a confirmed figure.
