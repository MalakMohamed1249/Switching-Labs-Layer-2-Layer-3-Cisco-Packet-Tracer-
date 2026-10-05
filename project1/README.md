# Project 1 — Layer 2 Switching Lab

A campus network built with Layer 2 switches in Cisco Packet Tracer: VLANs, trunking, EtherChannel, spanning tree, and secure remote access.

![Topology](WhatsApp%20Image%202026-10-01%20at%202.40.41%20AM.jpeg)

## Topology

- 2 distribution switches: **D1**, **D2**
- 3 access switches: **A1**, **A2**, **A3**
- 6 PCs, 2 per access switch (one in VLAN 10, one in VLAN 20)
- Every access switch connects to both D1 and D2 (redundant uplinks)

## Common configuration (all switches)

- `enable secret` set
- Local user `admin` with a secret, used for login on the VTY lines
- Lab credentials (training lab only): enable secret `cisco`, local user `admin` / secret `cisco`
- Domain name `CCNA.Lab`
- VLAN 10 `front` and VLAN 20 `back`
- SSH enabled and enforced (`transport input ssh`) — Telnet is not enabled

## Design

### Distribution switches

| | D1 | D2 |
|---|---|---|
| Management IP | 10.0.2.1/8 | 10.0.2.2/8 |
| STP root | Primary: VLANs 1, 10 · Secondary: VLAN 20 | Primary: VLAN 20 · Secondary: VLANs 1, 10 |
| D1–D2 link | Gig0/1–0/2, LACP **active** | Gig0/1–0/2, LACP **passive** |

The D1–D2 bundle is also configured as a static trunk.

### Access switches

| | A1 | A2 | A3 |
|---|---|---|---|
| Management IP | 10.0.1.1/8 | 10.0.1.2/8 | 10.0.1.3/8 |

Access ports: **Fa0/1 → VLAN 10**, **Fa0/2 → VLAN 20**, both with **PortFast + BPDU Guard**.

### Uplinks (trunks)

| Distribution port | Access port | Mode (distribution / access) |
|---|---|---|
| D1 Fa0/21 | A1 Fa0/23 | desirable / auto |
| D1 Fa0/22 | A2 Fa0/23 | desirable / auto |
| D1 Fa0/23 | A3 Fa0/23 | desirable / auto |
| D2 Fa0/21 | A1 Fa0/24 | desirable / auto |
| D2 Fa0/22 | A2 Fa0/24 | desirable / auto |
| D2 Fa0/23 | A3 Fa0/24 | desirable / auto |

## PC addressing

| PC | VLAN | IP address |
|---|---|---|
| User 1-1 | 10 | 192.168.10.1/24 |
| User 1-2 | 20 | 192.168.20.1/24 |
| User 2-1 | 10 | 192.168.10.2/24 |
| User 2-2 | 20 | 192.168.20.2/24 |
| User 3-1 | 10 | 192.168.10.3/24 |
| User 3-2 | 20 | 192.168.20.3/24 |

## Verification

```
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree vlan 10
show ip ssh
```

Ping between PCs in the same VLAN across different access switches.

## Notes

This lab is Layer 2 only: PCs communicate within their own VLAN. Inter-VLAN routing is added in [Project 2](../project2).

## Files

- `project1.pkt` — Packet Tracer file
- `WhatsApp Image 2026-10-01 at 2.40.41 AM.jpeg` — topology screenshot
