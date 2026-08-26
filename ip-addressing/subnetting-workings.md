# Subnetting Workings — VLSM derivation from 172.30.74.0/23

Shows the actual working, not just the final table, in case this needs to
be explained or defended.

## Starting point

Assigned block: `172.30.74.0/23`
- Binary mask: `11111111.11111111.11111110.00000000`
- Range: `172.30.74.0` – `172.30.75.255`
- Total addresses: 2^9 = 512

VLSM approach: allocate largest-need subnets first, carving down from the
top of the range, so each subnet's boundary aligns correctly on a power-of-2
address block.

## Step 1 — VLAN 10 (Research), needs ~126 usable hosts

Nearest power of 2 ≥ 126 usable hosts → borrow so that 2^h − 2 ≥ 126 →
h = 7 (2^7 − 2 = 126 usable). That's a `/25` (32 − 7 = 25).

- Subnet: `172.30.74.0/25`
- Range: `172.30.74.0` – `172.30.74.127`
- Usable: `172.30.74.1` – `172.30.74.126` (gateway = `.1`)
- Broadcast: `172.30.74.127`

Remaining space starts at `172.30.74.128`.

## Step 2 — VLAN 20 (Admin), needs ~62 usable hosts

2^6 − 2 = 62 usable → `/26` (32 − 6 = 26).

- Subnet: `172.30.74.128/26`
- Range: `172.30.74.128` – `172.30.74.191`
- Usable: `172.30.74.129` – `172.30.74.190` (gateway = `.129`)
- Broadcast: `172.30.74.191`

Remaining space starts at `172.30.74.192`.

## Step 3 — VLAN 30 (Servers), needs ~30 usable hosts

2^5 − 2 = 30 usable → `/27` (32 − 5 = 27).

- Subnet: `172.30.74.192/27`
- Range: `172.30.74.192` – `172.30.74.223`
- Usable: `172.30.74.194` – `172.30.74.222` (gateway = `.193`)
- Broadcast: `172.30.74.223`

Remaining space starts at `172.30.74.224`.

## Step 4 — VLAN 40 (Guest Wi-Fi), needs ~30 usable hosts

Same size as Step 3 → `/27`.

- Subnet: `172.30.74.224/27`
- Range: `172.30.74.224` – `172.30.74.255`
- Usable: `172.30.74.226` – `172.30.74.254` (gateway = `.225`)
- Broadcast: `172.30.74.255`

This exactly fills the first half of the /23 block (`172.30.74.0/24`).
Remaining space starts at `172.30.75.0`.

## Step 5 — VLAN 99 (Management), needs ~10-14 usable hosts (infra only)

2^4 − 2 = 14 usable → `/28` (32 − 4 = 28).

- Subnet: `172.30.75.0/28`
- Range: `172.30.75.0` – `172.30.75.15`
- Usable: `172.30.75.2` – `172.30.75.14` (gateway = `.1`)
- Broadcast: `172.30.75.15`

## Remaining space

`172.30.75.16` through `172.30.75.255` (240 addresses) is left
unallocated — reserved for future departments, additional lab equipment,
or expansion, without needing to renumber any existing VLAN.

## Check — no overlaps, everything inside the assigned block

| Subnet | Falls within 172.30.74.0/23? |
|---|---|
| 172.30.74.0/25 | Yes |
| 172.30.74.128/26 | Yes |
| 172.30.74.192/27 | Yes |
| 172.30.74.224/27 | Yes |
| 172.30.75.0/28 | Yes |

All five subnets are contiguous, non-overlapping, and fall entirely within
the assigned 172.30.74.0/23 block.
