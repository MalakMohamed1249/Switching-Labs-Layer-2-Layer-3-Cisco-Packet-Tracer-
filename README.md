# Switching Labs — Cisco Packet Tracer

Two versions of the same campus network lab: first built with **Layer 2 switching**, then rebuilt with **Layer 3 switching and routing**. Same topology, same security baseline, different design.

![Topology](project1/WhatsApp%20Image%202026-10-01%20at%202.40.41%20AM.jpeg)

**Topology:** 2 distribution switches (D1, D2) · 3 access switches (A1–A3) · 6 PCs in 2 VLANs

## Projects

| Project | What it covers |
|---|---|
| [Project 1 — Layer 2 Switching](./project1) | VLANs, trunking (DTP), LACP EtherChannel, STP root placement, PortFast + BPDU Guard |
| [Project 2 — Layer 3 Switching](./project2) | Routed ports, Layer 3 EtherChannel, OSPF, SVIs as default gateways |

## Layer 2 vs Layer 3 at a glance

| | Project 1 (L2) | Project 2 (L3) |
|---|---|---|
| Inter-switch links | 802.1Q trunks (DTP) | Routed ports |
| Loop prevention | STP, root primary/secondary per VLAN | No Layer 2 loops between switches |
| D1–D2 EtherChannel | LACP, Layer 2 trunk | Layer 3 Port-channel with an IP address |
| Routing | None (Layer 2 only) | OSPF, point-to-point |
| Default gateways | None | SVIs on the access switches |
| Management IP | 10.0.x.x /8 | Same numbers on Loopback0 /32 |

## Common baseline (both projects)

- `enable secret` and a local user `admin`
- Lab credentials (training lab only): enable secret `cisco`, local user `admin` / secret `cisco`
- Domain name `CCNA.Lab`
- VLAN 10 `front` and VLAN 20 `back`
- SSH enabled and enforced on the VTY lines (`transport input ssh`) — Telnet is not enabled
- PortFast + BPDU Guard on PC-facing ports

## Skills practiced

VLANs · 802.1Q trunking · DTP · LACP · STP · SVIs · routed ports · OSPF · SSH hardening · CLI verification
