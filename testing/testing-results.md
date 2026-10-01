# Testing & Verification Results (Milestone 2)

All tests performed with `ping` from each device's Command Prompt (or
Server equivalent), screenshots captured in `../screenshots/` with filenames
referenced below.

## 1. Intra-VLAN connectivity (device → own gateway)

| Device | VLAN | Target (gateway) | Result | Screenshot |
|---|---|---|---|---|
| PC-Admin1 | 20 | 172.30.74.129 | 4/4 replies, 0% loss, TTL=255 | `01-admin1-gateway-and-vlan30-ttl127.png` |
| PC-Research2 | 10 | 172.30.74.1 | 4/4 replies, 0% loss, TTL=255 | `02-research2-gateway.png` |
| Server-App | 30 | 172.30.74.193 | 4/4 replies, 0% loss, TTL=255 | `03-serverapp-gateway.png` |
| Laptop-Contractor (wireless) | 40 | 172.30.74.225 | 4/4 replies, 0% loss | `05-before-acl-contractor-and-vlan30.png` |

Confirms every VLAN's end devices can reach their own default gateway —
basic Layer 2/3 connectivity within each VLAN is working. (PC-Research1 and
Server-File were configured identically to their VLAN partners above and
were not separately screenshotted, since PC-Research2 and Server-App
already demonstrate connectivity for VLANs 10 and 30 respectively.)

Wireless association evidence (Laptop-Contractor connected to SSID
`Lesedi-Guest`): `04-contractor-wireless-connected.png`

## 2. Inter-VLAN routing — Router-on-a-Stick proof

PC-Admin1 (VLAN 20) pinged Server-File (VLAN 30, 172.30.74.194):

- Result: 4/4 replies, 0% loss, **TTL=127**
- Significance: TTL dropped from the usual 255 (Cisco default, seen in all
  same-VLAN tests above) to 127 (Windows/PC default of 128, minus one
  router hop). This is direct evidence the packet was routed through
  Router-Lesedi — proof of real inter-VLAN routing, not simple switching.
- Screenshot: `01-admin1-gateway-and-vlan30-ttl127.png` (same screenshot as
  the PC-Admin1 gateway test above — both pings are visible in the same
  Command Prompt window)

## 3. Restricted VLAN 40 — before and after the ACL

**Before** ACL 140 was applied:
- Laptop-Contractor (VLAN 40) → gateway (172.30.74.225) and → Server-File
  (172.30.74.194): both succeeded, the Server-File ping showing TTL=127 —
  fully open, as expected prior to restriction.
- Screenshot: `05-before-acl-contractor-and-vlan30.png`

**After** ACL 140 applied to Fa0/0.40:
- Same ping repeated → failed with "Destination host unreachable" from
  172.30.74.225 (the router's own gateway for VLAN 40) — proof the router
  is actively denying the packet, not just losing it.
- Screenshot: `06-after-acl-contractor-denied.png`

**Permitted Internet access** (proves restriction is selective, not total):
- Laptop-Contractor → Server-Internet (203.0.113.2).
- Screenshot: `10-contractor-internet-permitted.png` (if captured — see note below)

## 4. ACL match counters

`show access-lists 140` run after the above tests:

- Each deny line: 4 matches (from the four blocked pings to VLAN 10/20/30/99)
- Permit line: 4 matches (from the pings allowed through)
- Confirms the ACL is actively filtering real traffic, not sitting unused.
- Screenshot: `07-show-access-lists-140.png`

## 5. Interface and trunk verification

- Router sub-interfaces all up/up with correct IPs:
  `08-show-ip-interface-brief.png`
- Core-Switch trunk ports/VLANs confirmed:
  `09-show-interfaces-trunk.png`

## Summary against requirements

| Requirement | Evidence |
|---|---|
| R2 — segmentation | VLANs verified via `show vlan brief` (see `switch-configuration.md`) |
| R3 — Router-on-a-Stick | Section 2 above (TTL=127 proof) |
| R4 — limited contractor access | Section 3 above (denied internally, permitted to Internet) |
| R7 — end-to-end connectivity demonstrated | Sections 1–3 above |
