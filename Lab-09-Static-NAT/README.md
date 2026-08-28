📘 Lab 09: Static NAT

🎯 Objective

To configure and verify Static Network Address Translation (NAT) on a Cisco router.

This lab demonstrates how a private inside IP address can be permanently mapped to a public/global IP address using a one-to-one Static NAT mapping.

---

🏢 Scenario

A company has an internal server using a private IP address.

The company wants the server to be reachable from an external network using a public IP address.

Static NAT is used to create a permanent one-to-one mapping between the private and public addresses.

Static NAT Mapping

```text
Inside Local                 Inside Global
192.168.10.10       →       203.0.113.10
Private IP                   Public/Global IP

🧱 Topology
2 Cisco 2911 Routers
1 Cisco 2960 Switch
1 Server
1 External PC

Server
192.168.10.10
    │
 Switch
    │
 R1 — NAT Router
    │
    │ 203.0.113.0/30
    │
 R2 — ISP Router
    │
    │ 192.168.20.0/24
    │
External PC
192.168.20.10
The External PC is connected directly to R2.

🌐 Network Design
| Device      | Interface | IP Address      | Network           | Default Gateway |
| ----------- | --------- | --------------- | ----------------- | --------------- |
| Server      | —         | `192.168.10.10` | `192.168.10.0/24` | `192.168.10.1`  |
| R1          | G0/0      | `192.168.10.1`  | `192.168.10.0/24` | —               |
| R1          | G0/1      | `203.0.113.1`   | `203.0.113.0/30`  | —               |
| R2          | G0/0      | `203.0.113.2`   | `203.0.113.0/30`  | —               |
| R2          | G0/1      | `192.168.20.1`  | `192.168.20.0/24` | —               |
| External PC | —         | `192.168.20.10` | `192.168.20.0/24` | `192.168.20.1`  |

⚙️ R1 Interface Configuration
interface gigabitEthernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface gigabitEthernet 0/1
 ip address 203.0.113.1 255.255.255.252
 no shutdown

 ⚙️ R2 Interface Configuration
 interface gigabitEthernet 0/0
 ip address 203.0.113.2 255.255.255.252
 no shutdown

interface gigabitEthernet 0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown

 🛣️ Routing Configuration
 R1
 ip route 192.168.20.0 255.255.255.0 203.0.113.2
 R2
 ip route 192.168.10.0 255.255.255.0 203.0.113.1

 🔄 Static NAT Configuration

On R1, configure the internal interface:
interface gigabitEthernet 0/0
 ip nat inside

Configure the external interface:
interface gigabitEthernet 0/1
 ip nat outside

 Create the Static NAT mapping:
 ip nat inside source static 192.168.10.10 203.0.113.10

 This creates a permanent one-to-one mapping:
 192.168.10.10 → 203.0.113.10
 
 🧠 NAT Terminology
 | Term           | Address         | Description                                     |
| -------------- | --------------- | ----------------------------------------------- |
| Inside Local   | `192.168.10.10` | Private address of the internal server          |
| Inside Global  | `203.0.113.10`  | Public address representing the internal server |
| Outside Local  | `192.168.20.10` | External PC address                             |
| Outside Global | `192.168.20.10` | Actual external PC address                      |

🧪 Verification
Verify NAT Translation
show ip nat translations

Expected mapping:
203.0.113.10 → 192.168.10.10

Verify NAT Statistics: 
show ip nat statistics

📊 Final Results
| Test                         | Result       |
| ---------------------------- | ------------ |
| Static NAT mapping           | ✅ Verified   |
| NAT inside interface         | ✅ Configured |
| NAT outside interface        | ✅ Configured |
| NAT statistics               | ✅ Verified   |
| External PC → `203.0.113.10` | ✅ Successful |
| Server → External Network    | ✅ Connected  |

🧠 Key Concepts
Network Address Translation (NAT)
Static NAT
One-to-one address translation
Inside Local
Inside Global
Outside Local
Outside Global
NAT inside interface
NAT outside interface
NAT translation table
NAT statistics
Private IP addresses
Public/global IP addresses
Static routing

🎯 Result

Static NAT was successfully configured and verified.

The internal server's private IP address 192.168.10.10 was permanently mapped to the public/global address 203.0.113.10.

The External PC successfully reached the internal server using the public NAT address.

This lab demonstrates the basic operation of one-to-one Static NAT in a Cisco network.