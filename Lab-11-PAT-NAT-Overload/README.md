Lab 11 – PAT (NAT Overload)

Objective
Configure Port Address Translation (PAT), also called NAT Overload, so that multiple inside hosts can share a single outside/global IP address.

Topology

```text
PC1 192.168.10.10 ─┐
PC2 192.168.10.20 ─┤
PC3 192.168.10.30 ─┤
                   SW0
                    |
             Router0 (NAT)
          G0/0: 192.168.10.1
          G0/1: 172.165.20.1
                    |
             Router1 (ISP)
          G0/0: 172.165.20.2
          G0/1: 192.168.20.1
                    |
             Server: 192.168.20.10
```

Addressing

| Device | Interface | IP Address | Role |
|---|---|---|---|
| PC1 | NIC | 192.168.10.10/24 | Inside host |
| PC2 | NIC | 192.168.10.20/24 | Inside host |
| PC3 | NIC | 192.168.10.30/24 | Inside host |
| Router0 | G0/0 | 192.168.10.1/24 | NAT inside |
| Router0 | G0/1 | 172.165.20.1/16 | NAT outside |
| Router1 | G0/0 | 172.165.20.2/16 | ISP side |
| Router1 | G0/1 | 192.168.20.1/24 | Server gateway |
| Server | NIC | 192.168.20.10/24 | External destination |

PAT Design

ACL 2 identifies the inside network:

```cisco
access-list 2 permit 192.168.10.0 0.0.0.255
```

PAT uses Router0's G0/1 address:

```cisco
ip nat inside source list 2 interface GigabitEthernet0/1 overload
```

Therefore, PC1, PC2, and PC3 can share `172.165.20.1` when communicating with the outside network.

  Lab 10 → Lab 11

Lab 10 used Dynamic NAT with a pool:

```text
203.0.113.10 – 203.0.113.12
```

Lab 11 removes the NAT pool and uses PAT/NAT Overload with the outside interface address:

```text
172.165.20.1
```

  Tools
- Cisco Packet Tracer
- Cisco IOS CLI
- Git
- GitHub

Screenshots
Recommended screenshots for this lab:

- `topology.png`
- `pat-configuration.png`
- `pat-translations.png`
- `connectivity-test.png`
