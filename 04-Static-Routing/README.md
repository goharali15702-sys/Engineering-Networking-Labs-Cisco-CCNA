📘 Lab 04: Static Routing

🎯 Objective

To configure static routing between two routers and enable communication between different networks.

---

🧱 Topology

* 2 Routers (R1, R2)
* 2 Switches
* 2 PCs

---

🌐 IP Addressing

| Device  | IP           |
| ------- | ------------ |
| R1 G0/0 | 192.168.1.1  |
| R1 G0/1 | 10.0.0.1     |
| R2 G0/0 | 192.168.2.1  |
| R2 G0/1 | 10.0.0.2     |
| PC1     | 192.168.1.10 |
| PC2     | 192.168.2.10 |

---

 ⚙️ Configuration

R1

```bash
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface g0/1
ip address 10.0.0.1 255.255.255.0
no shutdown

ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

---

 R2

```bash
interface g0/0
ip address 192.168.2.1 255.255.255.0
no shutdown

interface g0/1
ip address 10.0.0.2 255.255.255.0
no shutdown

ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

---

✅ Verification

```bash
ping 192.168.2.10
```

---

🧠 Key Concepts

* Static Routing
* Next Hop
* Multi-router Communication

---

🎯 Result

Devices from different networks communicate successfully using static routing.
