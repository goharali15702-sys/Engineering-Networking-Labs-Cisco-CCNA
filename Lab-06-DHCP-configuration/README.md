📘 Lab 06: DHCP Configuration

🎯 Objective

To configure a Cisco router as a DHCP server and automatically assign IP addressing,Default Gatway and DNS  information to end devices.

---

🧱 Topology

* 1 Router
* 1 Switch
* 3 PCs
* Router configured as DHCP Server

---

🌐 Network Design

| Network      | Prefix | Default Gateway |
| ------------ | ------ | --------------- |
| 192.168.10.0 | /24    | 192.168.10.1    |

---

⚙️ Configuration

Router

The router interface was configured with the default gateway address:

```bash
interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
```

A DHCP pool was created to automatically assign IP addresses to the PCs:

```bash
ip dhcp pool LAN
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
```

The router's address was excluded from the DHCP pool:

```bash
ip dhcp excluded-address 192.168.10.1
```

---

🖥️ PC Configuration

All three PCs were configured to obtain their IP addresses automatically using DHCP.

Example:

```text
PC1 → DHCP
PC2 → DHCP
PC3 → DHCP
```

---

🖥️ Verification

DHCP pool:

```bash
show ip dhcp pool
```

DHCP address assignments:

```bash
show ip dhcp binding
```

Interface status:

```bash
show ip interface brief
```

Connectivity was tested using `ping` between the PCs.

---

🧠 Key Concepts

* DHCP
* DHCP Server
* DHCP Pool
* DHCP Excluded Addresses
* Dynamic IP Address Assignment
* Default Gateway
* DNS Server
* IP Address Management

---

🎯 Result

The Cisco router successfully operated as a DHCP server.

The PCs automatically received IP addresses, subnet masks, default gateways, and DNS information from the DHCP server.

Connectivity between the network devices was successfully verified.
