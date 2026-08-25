📘 Lab 07: Standard ACL

🎯 Objective

To configure and verify a Standard IPv4 ACL to control network traffic based on the source IP address.

---

🧱 Topology

* 1 Router
* 2 Switch
* 2 PCs
* 1 Server
* 2 Networks

---

🌐 Network Design

| Device      | IP Address    | Network         | Default Gateway |
| ----------- | ------------- | --------------- | --------------- |
| HR PC       | 192.168.10.10 | 192.168.10.0/24 | 192.168.10.1    |
| IT PC       | 192.168.10.20 | 192.168.10.0/24 | 192.168.10.1    |
| Router G0/0 | 192.168.10.1  | 192.168.10.0/24 | —               |
| Router G0/1 | 192.168.20.1  | 192.168.20.0/24 | —               |
| Server      | 192.168.20.10 | 192.168.20.0/24 | 192.168.20.1    |

---

⚙️ Configuration

Router Interface Configuration

```bash
interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface g0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

Standard ACL Configuration

HR was denied access based on its source IP address, while all other sources were permitted.

```bash
access-list 10 deny host 192.168.10.10
access-list 10 permit any
```

Apply ACL

The Standard ACL was applied outbound on the interface toward the server network.

```bash
interface g0/1
ip access-group 10 out
```

---

🖥️ Verification

Verify the ACL:

```bash
show access-lists
```

Verify the ACL applied to the interface:

```bash
show ip interface g0/1
```

Verify the configured ACL:

```bash
show running-config | include access-list
```

Connectivity Tests

Before applying the ACL:

```text
HR → Server    Successful
IT → Server    Successful
```

After applying the ACL:

```text
HR → Server    Blocked
IT → Server    Successful
```

---

🧠 Key Concepts

* Standard ACL
* Source IP Address
* ACL Numbering
* Permit and Deny
* Implicit Deny
* ACL Placement
* Inbound vs Outbound
* `ip access-group`
* ACL Verification

---

## 🎯 Result

The Standard ACL successfully blocked the HR PC from reaching the server while allowing the IT PC to communicate with the server.

This demonstrated how Standard ACLs control traffic based on the source IP address.
