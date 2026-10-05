# Project 2 — Layer 3 Switching Lab

The same topology and security baseline as [Project 1](../project1), rebuilt on **Layer 3 switches**: every inter-switch link is a routed port, the D1–D2 EtherChannel is a Layer 3 Port-channel, and OSPF handles routing.

![Topology](WhatsApp%20Image%202026-10-05%20at%202.31.47%20AM.jpeg)

## Design

- **Routed links:** all inter-switch links use `no switchport` with /30 point-to-point subnets (`10.1.x.0` for D1 links, `10.2.x.0` for D2 links). No STP is needed between switches.
- **Layer 3 EtherChannel:** the two Gigabit links between D1 and D2 form a routed Port-channel with its own IP address.
- **OSPF:** point-to-point network type on all routed links.
- **Gateways:** each access switch has an SVI per VLAN that acts as the default gateway for its PCs.
- **One subnet per VLAN per access switch:** VLANs are local to each access switch, so there are no trunks.
- **Management:** the same management numbers as Project 1, now on `Loopback0` with a /32 mask.
- **Access ports:** Fa0/1 → VLAN 10, Fa0/2 → VLAN 20, with PortFast + BPDU Guard.

## Addressing

| Access switch | VLAN 10 (front) | VLAN 20 (back) | PCs |
|---|---|---|---|
| A1 | 192.168.11.0/24 | 192.168.21.0/24 | User 1-1, User 1-2 |
| A2 | 192.168.12.0/24 | 192.168.22.0/24 | User 2-1, User 2-2 |
| A3 | 192.168.13.0/24 | 192.168.23.0/24 | User 3-1, User 3-2 |

Default gateway = the access switch SVI (`.1` of each subnet).

| Switch | Loopback0 (management) |
|---|---|
| D1 | 10.0.2.1/32 |
| D2 | 10.0.2.2/32 |
| A1 | 10.0.1.1/32 |
| A2 | 10.0.1.2/32 |
| A3 | 10.0.1.3/32 |

## Common configuration (all switches)

- `enable secret` and local user `admin`
- Lab credentials (training lab only): enable secret `cisco`, local user `admin` / secret `cisco`
- Domain name `CCNA.Lab`
- VLAN 10 `front` and VLAN 20 `back`
- SSH enabled and enforced (`transport input ssh`) — Telnet is not enabled

## Build order

1. Per-switch config first: VLANs, SVIs, IP addresses, routed ports
2. Enable `ip routing`
3. Configure OSPF on the routed links
4. Test PC-to-PC connectivity

## Verification

```
show ip interface brief
show etherchannel summary
show ip ospf neighbor
show ip route ospf
show ip ssh
```

On the EtherChannel, `Po1(RU)` means Layer 3 and in use. Ping between PCs in different VLANs and on different access switches.

## Why this design

- No dependency on STP between switches
- OSPF converges and fails over using routing instead of spanning tree
- Each access switch can use both of its uplinks
- Smaller broadcast domains: one subnet per VLAN per access switch

## Files

- `project2.pkt` — Packet Tracer file
- `WhatsApp Image 2026-10-05 at 2.31.47 AM.jpeg` — topology screenshot
