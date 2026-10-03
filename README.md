# 🔐 Secure Campus Network — Cisco Packet Tracer

A Cisco Packet Tracer mini project focused on designing and securing a small campus network using Cisco routing, switching, secure remote administration, and Layer 2 security controls.

## 📌 Project Overview

The network connects two IPv4 networks through a Cisco 2911 router and two Cisco 2960 switches.

### Network

- **1 × Cisco 2911 Router** — R1
- **2 × Cisco 2960-24TT Switches** — S1, S2
- **6 × PCs** — PC1 to PC6
- **2 × IPv4 networks**
  - `192.168.10.0/24`
  - `192.168.20.0/24`

The project demonstrates:

- IPv4 addressing and routing
- Cisco IOS configuration
- Secure console and privileged access
- SSH Version 2
- Switch port security
- Sticky MAC learning
- Unauthorized-device detection
- Unused-port shutdown
- Network connectivity verification

---

## 🎯 Objectives

- Build a functional two-network campus topology.
- Configure router and switch management interfaces.
- Enable secure remote administration using SSH.
- Apply basic access and password protection.
- Restrict switch access ports using port security.
- Detect unauthorized MAC addresses.
- Disable unused switch ports.
- Verify connectivity and security controls.

---

## 🗺️ Network Topology

```text
                         ┌───────────────┐
                         │   R1 Router   │
                         │ Cisco 2911    │
                         └──────┬─┬──────┘
                              G0/0 G0/1
                               /     \
                              /       \
                    ┌───────────┐   ┌───────────┐
                    │    S1     │   │    S2     │
                    │ Cisco2960 │   │ Cisco2960 │
                    └─┬───┬───┬─┘   └─┬───┬───┬─┘
                      │   │   │        │   │   │
                     PC1 PC2 PC3      PC4 PC5 PC6
🌐 IP Addressing
Device	Interface	IP Address	Network
R1	G0/0	192.168.10.1/24		192.168.10.0/24
R1	G0/1	192.168.20.1/24		192.168.20.0/24
S1	VLAN 1	192.168.10.2/24		192.168.10.0/24
S2	VLAN 1	192.168.20.2/24		192.168.20.0/24
PC1	—	192.168.10.10/24	192.168.10.0/24
PC2	—	192.168.10.11/24	192.168.10.0/24
PC3	—	192.168.10.12/24	192.168.10.0/24
PC4	—	192.168.20.10/24	192.168.20.0/24
PC5	—	192.168.20.11/24	192.168.20.0/24
PC6	—	192.168.20.12/24	192.168.20.0/24


🔒 Security Controls
SSH Version 2
Secure remote administration was configured on R1, S1, and S2 using SSH Version 2 with local user authentication.
Port Security
User-facing switch ports were protected with:
- Maximum 1 MAC address
- Sticky MAC learning
- Violation action set to shutdown
Unauthorized Device Detection
A port-security violation was intentionally demonstrated by connecting a different device to a protected port. The switch detected the unauthorized MAC address and placed the interface into secure-shutdown.
Unused Ports
Unused FastEthernet ports were disabled to reduce unnecessary network access points.
Access Protection
Console access and privileged EXEC access were secured, and password encryption was enabled.
🧪 Testing & Results
The completed network was tested for:
Test	Result
PC1 → Router	✅ Successful
PC1 → PC4	✅ Successful
SSH → R1	✅ Successful
SSH → S1	✅ Successful
SSH → S2	✅ Successful
Port Security	✅ Verified
Sticky MAC	✅ Verified
Unauthorized MAC Detection	✅ Verified
Unused Ports	✅ Verified


📸 Evidence
The screenshots/ directory contains project evidence covering:
- Network topology
- Test results
- Router configuration
- Switch configuration
- SSH login
- Connectivity testing
- Port-security verification
- Port-security violation
📁 Project Files
secure-campus-network/
│
├── README.md
├── secure-campus-network.pkt
├── Secure_Campus_Network_Project_Report_Sanitized.docx
│
└── screenshots/
    ├── 01-network-topology.png
    ├── 02-test-results.jpeg
    ├── 03-router-configuration.jpeg
    ├── 04-switch-configuration.jpeg
    ├── 05-ssh-login.jpeg
    ├── 06-router-configuration-details.jpeg
    ├── 07-connectivity-test.jpeg
    ├── 08-port-security-verification.jpeg
    └── 09-port-security-violation.png

🧠 Key Learning Outcomes
This project provided practical experience with:
- Cisco IOS
- IPv4 networking
- Router and switch configuration
- SSH-based administration
- Layer 2 port security
- Sticky MAC addresses
- Security violation handling
- Network troubleshooting
- Configuration verification
📄 Documentation
Detailed configuration procedures, testing steps, command references, and project documentation are available in the project report.
👤 Project
Secure Campus Network — Cisco Packet Tracer Mini Project
Author: P. Sukumar