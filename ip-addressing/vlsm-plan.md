# IP Addressing Plan — CLI-109

Assigned block: **172.30.74.0/23** (172.30.74.0 – 172.30.75.255, 512 addresses)

Subdivided using VLSM so each VLAN gets a subnet sized to its expected host
count, with spare space kept for future growth rather than allocating the
whole block immediately.

## VLAN plan

| VLAN | Name | Purpose | Subnet | Gateway (router sub-if) | Usable hosts |
|---|---|---|---|---|---|
| 10 | RESEARCH | Researcher PCs, data-collection/lab workstations | 172.30.74.0/25 | 172.30.74.1 | 126 |
| 20 | ADMIN | Administration & management staff | 172.30.74.128/26 | 172.30.74.129 | 62 |
| 30 | SERVERS | File/data server, research application server, lab equipment | 172.30.74.192/27 | 172.30.74.193 | 30 |
| 40 | GUEST-WIFI | After-hours cleaning/security contractors (wireless, restricted) | 172.30.74.224/27 | 172.30.74.225 | 30 |
| 99 | MGMT | Switch and AP management only (no end-user hosts) | 172.30.75.0/28 | 172.30.75.1 | 14 |

## Subnet detail

| Subnet (CIDR) | Network Address | Usable Range | Broadcast | Assigned To |
|---|---|---|---|---|
| 172.30.74.0/25 | 172.30.74.0 | 172.30.74.2 – 172.30.74.126 | 172.30.74.127 | VLAN 10 – Research |
| 172.30.74.128/26 | 172.30.74.128 | 172.30.74.130 – 172.30.74.190 | 172.30.74.191 | VLAN 20 – Admin |
| 172.30.74.192/27 | 172.30.74.192 | 172.30.74.194 – 172.30.74.222 | 172.30.74.223 | VLAN 30 – Servers |
| 172.30.74.224/27 | 172.30.74.224 | 172.30.74.226 – 172.30.74.254 | 172.30.74.255 | VLAN 40 – Guest Wi-Fi |
| 172.30.75.0/28 | 172.30.75.0 | 172.30.75.2 – 172.30.75.14 | 172.30.75.15 | VLAN 99 – Management |
| 172.30.75.16 – 172.30.75.255 (remaining) | — | — | — | Reserved for future growth (not allocated) |

## Sizing rationale

- VLAN 10 (Research) is sized largest (/25, 126 hosts) as the primary
  functional group — researcher workstations and lab/data-collection PCs.
- VLAN 20 (Admin) uses a /26 (62 hosts), adequate for administrative staff
  with headroom.
- VLAN 30 (Servers) and VLAN 40 (Guest Wi-Fi) each use a /27 (30 hosts) —
  small, well-defined populations.
- VLAN 99 (Management) uses a /28 (14 hosts) — infrastructure only, no
  end-user devices.
- Roughly half the /23 block (172.30.75.16 onward) is left unallocated for
  future departments, additional lab equipment, or a wired guest option,
  without needing to renumber the existing VLANs.
