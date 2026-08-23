📘 Lab 05: VLSM + VLANs + Inter-VLAN Routing

🎯 Objective

To design and implement a VLSM-based network using multiple VLANs and configure inter-VLAN routing using Router-on-a-Stick.

---

🧱 Topology

* 1 Router
* 1 Switch
* 4 PCs
* 4 VLANs
* Router-on-a-Stick

---

🌐 VLSM Design

| Department | Required Hosts | VLAN    | Network            | Prefix | Gateway         |
| ---------- | -------------- | ------- | ------------------ | ------ | --------------- |
| HR         | 50             | VLAN 10 | 192.168.100.0/26   | /26    | 192.168.100.1   |
| IT         | 25             | VLAN 20 | 192.168.100.64/27  | /27    | 192.168.100.65  |
| Sales      | 10             | VLAN 30 | 192.168.100.96/28  | /28    | 192.168.100.97  |
| Management | 2              | VLAN 40 | 192.168.100.112/30 | /30    | 192.168.100.113 |

---

⚙️ Configuration

Router

```bash
interface g0/0
no shutdown

interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.100.1 255.255.255.192

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.100.65 255.255.255.224

interface g0/0.30
encapsulation dot1Q 30
ip address 192.168.100.97 255.255.255.240

interface g0/0.40
encapsulation dot1Q 40
ip address 192.168.100.113 255.255.255.252
```

---

Switch

* VLAN 10 — HR
* VLAN 20 — IT
* VLAN 30 — SALES
* VLAN 40 — MANAGEMENT
* PC ports configured as access ports
* **Fa0/5 configured as trunk**

---

🖥️ PC Addressing

| PC         | IP Address      | Subnet Mask     | Default Gateway |
| ---------- | --------------- | --------------- | --------------- |
| HR         | 192.168.100.2   | 255.255.255.192 | 192.168.100.1   |
| IT         | 192.168.100.66  | 255.255.255.224 | 192.168.100.65  |
| Sales      | 192.168.100.98  | 255.255.255.240 | 192.168.100.97  |
| Management | 192.168.100.114 | 255.255.255.252 | 192.168.100.113 |

---

🖥️ Verification

```bash
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
```

Test inter-VLAN connectivity:

```bash
ping 192.168.100.66
ping 192.168.100.98
ping 192.168.100.114
```

---

🧠 Key Concepts

* VLSM
* Subnetting
* VLANs
* Access Ports
* Trunking
* 802.1Q
* Router-on-a-Stick
* Inter-VLAN Routing
* Default Gateway
* Network Addressing

---

🎯 Result

The network was successfully divided into four VLSM-based subnets and four VLANs.

Router-on-a-Stick was configured using router subinterfaces, and devices in different VLANs successfully communicated through the router.
