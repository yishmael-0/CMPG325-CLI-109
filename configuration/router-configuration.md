# Router Configuration — Router-Lesedi (Milestone 2)

Router-on-a-Stick implementation of the assigned technical challenge (R3):
one 802.1Q sub-interface per VLAN on FastEthernet0/0, each acting as that
VLAN's default gateway, exactly as planned in `03-ip-addressing/vlsm-plan.md`.

## Sub-interface configuration

```
interface FastEthernet0/0.10
encapsulation dot1Q 10
ip address 172.30.74.1 255.255.255.128

interface FastEthernet0/0.20
encapsulation dot1Q 20
ip address 172.30.74.129 255.255.255.192

interface FastEthernet0/0.30
encapsulation dot1Q 30
ip address 172.30.74.193 255.255.255.224

interface FastEthernet0/0.40
encapsulation dot1Q 40
ip address 172.30.74.225 255.255.255.224

interface FastEthernet0/0.99
encapsulation dot1Q 99
ip address 172.30.75.1 255.255.255.240

interface FastEthernet0/0
no shutdown
```

## Internet-facing link (Server-Internet)

A separate physical interface connects to the Server-Internet node, used to
test that VLAN 40 retains permitted outbound access after the restriction
ACL is applied (see `troubleshooting-log.md` is not relevant here — see
`../testing/testing-results.md` Section 3).

```
interface FastEthernet1/0
ip address 203.0.113.1 255.255.255.252
no shutdown
```

## VLAN 40 restriction ACL (R4)

Applied inbound on the VLAN 40 gateway sub-interface. Denies traffic toward
each internal VLAN's subnet; the final permit line allows everything else,
including the path to Server-Internet.

```
access-list 140 deny ip 172.30.74.224 0.0.0.31 172.30.74.0 0.0.0.127
access-list 140 deny ip 172.30.74.224 0.0.0.31 172.30.74.128 0.0.0.63
access-list 140 deny ip 172.30.74.224 0.0.0.31 172.30.74.192 0.0.0.31
access-list 140 deny ip 172.30.74.224 0.0.0.31 172.30.75.0 0.0.0.15
access-list 140 permit ip any any

interface FastEthernet0/0.40
ip access-group 140 in
```

| ACL line | Effect |
|---|---|
| deny ... 172.30.74.0 0.0.0.127 | Blocks VLAN 40 → VLAN 10 (Research, /25) |
| deny ... 172.30.74.128 0.0.0.63 | Blocks VLAN 40 → VLAN 20 (Admin, /26) |
| deny ... 172.30.74.192 0.0.0.31 | Blocks VLAN 40 → VLAN 30 (Servers, /27) |
| deny ... 172.30.75.0 0.0.0.15 | Blocks VLAN 40 → VLAN 99 (Management, /28) |
| permit ip any any | Allows everything else (Internet-facing link) |

## Saving

```
end
copy running-config startup-config
```

## Verification command used

```
show ip interface brief
show access-lists 140
```

Full output/results are in `../testing/testing-results.md` and the matching
screenshots in `../screenshots/`.
