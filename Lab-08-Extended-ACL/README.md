📘 Lab 08: Extended ACL

🎯 Objective

To configure and verify an Extended IPv4 ACL to control network traffic based on the source IP address, destination IP address, protocol, and port number.

---

🏢 Scenario

A company wants to control access to an internal web server.

The security policy is:

- HR can access the Server using HTTP.
- Sales cannot access the Server using HTTP.
- Both HR and Sales can ping the Server.

This lab demonstrates how Extended ACLs provide more granular traffic control than Standard ACLs.

---

🧱 Topology

- 1 Cisco 2911 Router
- 1 Cisco 2960 Switch
- 2 PCs
- 1 Server

Network Topology

```text
HR PC ─────┐
           │
           ├── Switch ─── Router ─── Server
           │
Sales PC ──┘

🌐 Network Design

| Device   | Interface | IP Address      | Network           | Default Gateway |
| -------- | --------- | --------------- | ----------------- | --------------- |
| HR PC    | —         | `192.168.10.10` | `192.168.10.0/24` | `192.168.10.1`  |
| Sales PC | —         | `192.168.10.20` | `192.168.10.0/24` | `192.168.10.1`  |
| Router   | G0/0      | `192.168.10.1`  | `192.168.10.0/24` | —               |
| Router   | G0/1      | `192.168.20.1`  | `192.168.20.0/24` | —               |
| Server   | —         | `192.168.20.10` | `192.168.20.0/24` | `192.168.20.1`  |


⚙️ Router Configuration

Interface Configuration

interface g0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface g0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown

⚙️ Extended ACL Configuration

ACL 100 is used to block HTTP traffic from the Sales PC to the Server.

access-list 100 deny tcp host 192.168.10.20 host 192.168.20.10 eq 80
access-list 100 permit icmp any any
access-list 100 permit ip any any

Apply the ACL

The ACL is applied inbound on G0/1:

interface g0/1
 ip access-group 100 in

🖥️ Server Configuration

The Server was configured with:

IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1

The HTTP service was enabled on the Server.

🧪 Verification

Verify ACL Configuration

show access-lists

Verify ACL Applied to Interface

show ip interface gigabitEthernet 0/1

Verify Running Configuration

show running-config | include access-list

🔎 Connectivity Testing

HR → Server

HTTP:

http://192.168.20.10

Result:

✅ HTTP Access Allowed

Ping:

ping 192.168.20.10

Result:

✅ Ping Successful

Sales → Server

HTTP:

http://192.168.20.10

Result:

❌ HTTP Access Blocked

Ping:

ping 192.168.20.10

Result:

✅ Ping Successful


📊 Final Results

| Traffic             | Result    |
| ------------------- | --------- |
| HR → Server HTTP    | ✅ Allowed |
| HR → Server Ping    | ✅ Allowed |
| Sales → Server HTTP | ❌ Blocked |
| Sales → Server Ping | ✅ Allowed |


🧠 Key Concepts

Extended ACL

Source IP address

Destination IP address

TCP

ICMP

TCP port 80

Permit and Deny

ACL processing order

Implicit Deny

Inbound and Outbound ACLs

ACL placement

ip access-group

ACL verification

🔐 Standard ACL vs Extended ACL

This lab builds on Lab 07 — Standard ACL.

| Feature         | Standard ACL | Extended ACL |
| --------------- | ------------ | ------------ |
| Source IP       | ✅           | ✅          |
| Destination IP  | ❌           | ✅          |
| Protocol        | ❌           | ✅          |
| Port number     | ❌           | ✅          |
| Traffic control | Basic        | Granular     |

Standard ACL: Controls traffic primarily based on source IP.

Extended ACL: Can control traffic based on source, destination, protocol, and port.



🎯 Result

The Extended ACL was successfully configured and verified.
Sales was prevented from accessing the Server using HTTP, while HR retained HTTP access and both departments maintained ICMP connectivity.
This lab demonstrated how Extended ACLs can provide granular traffic control in a Cisco network.
