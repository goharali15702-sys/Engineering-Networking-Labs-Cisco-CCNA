Lab 10 – Dynamic NAT with NAT Pool

Objective

Configure and verify Dynamic Network Address Translation (Dynamic NAT) using a NAT pool on a Cisco router.

Dynamic NAT allows inside private IP addresses to be dynamically translated to available global IP addresses from a configured NAT pool.

Topology

```text
PC1 ─┐
PC2 ─┼── Switch ── R0 (NAT Router) ── R1 ── Switch ── Server
PC3 ─┘

Inside LAN:       192.168.10.0/24
R0-R1 Network:    172.165.0.0/16
Server LAN:       192.168.20.0/24
NAT Pool:         203.0.113.10 - 203.0.113.12
```

Addressing Table

| Device | Interface | IP Address | Purpose |
|---|---|---|---|
| R0 | G0/0 | 192.168.10.1/24 | NAT Inside |
| R0 | G0/1 | 172.165.20.1/16 | NAT Outside |
| R1 | G0/0 | 172.165.20.2/16 | Link to R0 |
| R1 | G0/1 | 192.168.20.1/24 | Server Gateway |
| PC1 | NIC | 192.168.10.10/24 | Inside Host |
| PC2 | NIC | 192.168.10.20/24 | Inside Host |
| PC3 | NIC | 192.168.10.30/24 | Inside Host |
| Server | NIC | 192.168.20.10/24 | Destination Server |

NAT Pool

```text
NATPOOL
203.0.113.10
203.0.113.11
203.0.113.12
```

Configuration

R0 is configured as the NAT router.

The inside network `192.168.10.0/24` is identified using standard ACL 1.

The ACL is connected to the NAT pool with:

```cisco
ip nat inside source list 2 pool NAT-POOL
```

R1 contains a return route to the NAT pool network:

```cisco
ip route 203.0.113.0 255.255.255.0 172.165.20.1
```

Verification

The following commands are used to verify the configuration:

```cisco
show ip interface brief
show ip route
show access-lists
show ip nat statistics
show ip nat translations
```

Connectivity is tested from the PCs to the server:

```text
PC1 → 192.168.20.10
PC2 → 192.168.20.10
PC3 → 192.168.20.10
```

Expected NAT Behavior

Private inside addresses are dynamically translated to addresses from the NAT pool.

Example:

```text
192.168.10.10 → 203.0.113.10
192.168.10.20 → 203.0.113.11
192.168.10.30 → 203.0.113.12
```

The exact global address assigned to each PC may vary depending on which host creates the translation first.

Key Learning

Dynamic NAT uses:

```text
Inside Network
      ↓
     ACL
      ↓
  NAT Pool
      ↓
Inside Global Addresses
```

Unlike Static NAT, Dynamic NAT creates translations dynamically when traffic is generated.

Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Git
- GitHub

Status

✅ Completed
