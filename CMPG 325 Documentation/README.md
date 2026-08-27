# CMPG325 — Network Design & Implementation Portfolio

**Student:** Yishmael
**Client:** Lesedi Renewable Energy Research Group (Mahikeng)
**Client ID:** CLI-109
**Project ID:** CMPG325-2026-109
**Assigned IP block:** 172.30.74.0/23
**Assigned technical challenge:** Router-on-a-Stick (sub-interface inter-VLAN routing)

## About this repository

This repository is my individual portfolio of evidence for the CMPG325 network
design and implementation assignment. It documents the full lifecycle of the
project for CLI-109: requirements, design, IP addressing, Packet Tracer build,
configuration, testing, and reflection.

All work here is my own, produced specifically for the CLI-109 scenario.

## Client scenario summary

Lesedi Renewable Energy Research Group is a research organisation that needs
a segmented internal network (research staff, administration, servers/lab
equipment), inter-VLAN routing via Router-on-a-Stick, and **limited** wireless
access for after-hours cleaning/security contractors — all within a
heritage-listed building where no new cabling may be run through external
walls.

## Network diagrams

### Physical topology

![Physical topology](design/physical-topology.png)

Router-on-a-Stick edge router, core switch, two access switches (Research
wing and Admin office), server room devices, and one indoor wireless AP
serving the restricted guest VLAN — connected via existing internal cabling
to respect the heritage building constraint.

### Logical topology

![Logical topology](design/logical_topology.png)

Five VLANs routed through one physical trunk link via router sub-interfaces
(802.1Q), each acting as the default gateway for its VLAN.

## Repository structure

| Folder | Contents |
|---|---|
| `01-requirements/` | Client requirements traceability |
| `02-design/` | Physical and logical topology diagrams, design rationale |
| `03-ip-addressing/` | VLSM IP addressing plan and subnetting workings |
| `04-packet-tracer/` | `.pkt` file(s), versioned as the build progresses |
| `05-configuration/` | Exported device configs (router sub-interfaces, switch VLAN/trunk, AP) |
| `06-testing/` | Connectivity and Router-on-a-Stick verification evidence |
| `07-screenshots/` | Topology and simulation-mode screenshots |
| `08-troubleshooting/` | Issues encountered and how they were resolved |
| `09-reflection/` | Final reflection on the design and challenge |

## Milestone log

- **Milestone 1 — Client Design Review** (this commit): client requirements,
  physical topology, logical topology, IP addressing plan.
  See `01-requirements/`, `02-design/`, `03-ip-addressing/`.
- Milestone 2 — Packet Tracer build & configuration: *pending*
- Milestone 3 — Testing & evidence: *pending*
- Final submission — full portfolio & reflection: *pending*

## Design summary (Milestone 1)

- **VLANs:** 10 (Research), 20 (Admin), 30 (Servers), 40 (Guest Wi-Fi —
  restricted, after-hours contractors), 99 (Management).
- **Routing:** single router, one physical trunk to the core switch, one
  802.1Q sub-interface per VLAN (Router-on-a-Stick).
- **Wireless:** one indoor AP on VLAN 40, fed from existing internal cabling
  to respect the heritage no-external-cabling constraint; restricted via
  router ACL to Internet-only access.
- **Addressing:** VLSM carved from 172.30.74.0/23, with roughly half the
  block reserved for future growth.

Full detail is in the Milestone 1 report under `01-requirements/`,
`02-design/`, and `03-ip-addressing/`.
