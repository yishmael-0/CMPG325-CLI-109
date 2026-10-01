# Troubleshooting Log (Milestone 2)

Real issues encountered during implementation, documented as part of the
build process rather than omitted.

## 1. Subnet mask truncation while typing router commands

**Symptom:** `ip address 172.30.74.129 255.255.255.192` repeatedly failed
with `% Invalid input detected at '^' marker.`

**Diagnosis:** Checking the exact CLI text character-by-character showed
the subnet mask was actually being entered as `255.255.192` or `255.192` —
one or two octets were silently dropped while typing (likely due to the
router's own `%LINK-3-UPDOWN` / `%LINEPROTO-5-UPDOWN` status messages
interrupting the terminal mid-type).

**Fix:** Typed the command in deliberate pieces and explicitly counted the
four dot-separated octets of the mask before pressing Enter each time.

## 2. VLAN 10 sub-interface missing after a CLI timeout

**Symptom:** After Packet Tracer's CLI timed out from inactivity (session
dropped back to `Router>` user mode), a later `show ip interface brief`
showed sub-interfaces .20, .30, .40, .99 but not .10.

**Diagnosis:** The sub-interface was likely lost when the device session
reset/timed out before the configuration had been saved to startup-config.

**Fix:** Re-entered `interface FastEthernet0/0.10` with its encapsulation
and IP address commands again; confirmed present on the next
`show ip interface brief`. Saved immediately afterward with
`copy running-config startup-config` to prevent a repeat.

## 3. PC/server IP addresses defaulting to 0.0.0.0

**Symptom:** Initial ping test from PC-Admin1 to its gateway failed
completely.

**Diagnosis:** `ipconfig` showed IPv4 Address `0.0.0.0` — the device had
never actually been given a static IP matching its VLAN.

**Fix:** Set IP Configuration to Static on every end device (both research
PCs, the admin PC, both servers, the contractor laptop) with the correct
address/mask/gateway for its VLAN.

## 4. Wireless NIC and DHCP mismatch on Laptop-Contractor

**Symptom:** The AP's SSID (`Lesedi-Guest`) was not visible from
Laptop-Contractor's PC Wireless tool; separately, IP Configuration showed
an auto-assigned 169.254.x.x (APIPA) address.

**Diagnosis:** The laptop had no wireless interface module installed, and
its IP Configuration was set to DHCP, which failed (no DHCP server
configured in this topology) and fell back to APIPA.

**Fix:**
- Powered off the laptop, added a WPC300N wireless module via the Physical
  tab, powered it back on, then connected to the `Lesedi-Guest` SSID.
- Switched IP Configuration from DHCP to Static and entered the correct
  VLAN 40 address (172.30.74.226 / 255.255.255.224, gateway 172.30.74.225).
