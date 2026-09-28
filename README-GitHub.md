# 🔐 Secure Campus Network --- Cisco Packet Tracer

A Cisco Packet Tracer mini project that demonstrates the design,
configuration, testing, and basic security hardening of a small campus
network.

## 📌 Project Overview

This project implements a secure campus network using:

-   **1 × Cisco 2911 Router** --- R1
-   **2 × Cisco 2960-24TT Switches** --- S1 and S2
-   **6 × PCs** --- PC1 to PC6
-   Copper straight-through connections

The router connects two IPv4 networks:

-   `192.168.10.0/24`
-   `192.168.20.0/24`

The project focuses on:

-   Router and switch configuration
-   Console security
-   Privileged EXEC (`enable secret`) security
-   Password encryption
-   SSH version 2
-   Switch port security
-   Sticky MAC learning
-   Port-security violation detection
-   Unused-port shutdown
-   Connectivity and configuration verification

## 🎯 Objectives

-   Configure R1 interfaces and inter-network connectivity.
-   Configure management IP addresses on S1 and S2.
-   Secure console and privileged EXEC access.
-   Enable password encryption.
-   Configure SSH v2 for secure remote administration.
-   Restrict user-facing switch ports to one MAC address.
-   Use sticky MAC learning.
-   Shut down a port when a security violation occurs.
-   Disable unused switch ports.
-   Test connectivity and SSH access.
-   Save the final configurations.

## 🗺️ Network Topology

``` text
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
```

### Physical Connections

  Device   Port    Connected To
  -------- ------- --------------
  R1       G0/0    S1 G0/1
  R1       G0/1    S2 G0/1
  S1       Fa0/1   PC1
  S1       Fa0/2   PC2
  S1       Fa0/3   PC3
  S2       Fa0/1   PC4
  S2       Fa0/2   PC5
  S2       Fa0/3   PC6

## 🌐 IP Addressing

  Device   Interface   IP Address        Subnet Mask       Gateway
  -------- ----------- ----------------- ----------------- ----------------
  R1       G0/0        `192.168.10.1`    `255.255.255.0`   ---
  R1       G0/1        `192.168.20.1`    `255.255.255.0`   ---
  S1       VLAN 1      `192.168.10.2`    `255.255.255.0`   `192.168.10.1`
  S2       VLAN 1      `192.168.20.2`    `255.255.255.0`   `192.168.20.1`
  PC1      NIC         `192.168.10.10`   `255.255.255.0`   `192.168.10.1`
  PC2      NIC         `192.168.10.11`   `255.255.255.0`   `192.168.10.1`
  PC3      NIC         `192.168.10.12`   `255.255.255.0`   `192.168.10.1`
  PC4      NIC         `192.168.20.10`   `255.255.255.0`   `192.168.20.1`
  PC5      NIC         `192.168.20.11`   `255.255.255.0`   `192.168.20.1`
  PC6      NIC         `192.168.20.12`   `255.255.255.0`   `192.168.20.1`

------------------------------------------------------------------------

# ⚙️ Configuration

## 1. Router R1

### Interface Configuration

``` cisco
enable
configure terminal
hostname R1

interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

show ip interface brief
```

### Console and Privileged EXEC Security

``` cisco
line console 0
password <CONSOLE_PASSWORD>
login
exit

enable secret <ENABLE_SECRET>
```

### Password Encryption

``` cisco
service password-encryption
show running-config
```

> `service password-encryption` obfuscates line passwords in the
> configuration. It is not a replacement for `enable secret`.
>
> **GitHub security note:** Credentials are represented by placeholders in this public README. The actual Packet Tracer credentials are intentionally not published here.

### SSH Configuration

``` cisco
ip domain-name campus.local
username admin privilege 15 secret <SSH_ADMIN_PASSWORD>
crypto key generate rsa
ip ssh version 2

line vty 0 4
login local
transport input ssh
exit

show ip ssh
```

------------------------------------------------------------------------

# 🔀 Switch S1

## Basic Configuration

``` cisco
enable
configure terminal
hostname S1
enable secret <ENABLE_SECRET>
service password-encryption

interface vlan 1
ip address 192.168.10.2 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.10.1
```

## Console and SSH

``` cisco
line console 0
password <SWITCH_CONSOLE_PASSWORD>
login
exit

ip domain-name campus.local
username admin privilege 15 secret <SSH_ADMIN_PASSWORD>
crypto key generate rsa
ip ssh version 2

line vty 0 4
login local
transport input ssh
exit
```

## Port Security --- Fa0/1 to Fa0/3

``` cisco
interface fastEthernet 0/1
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

interface fastEthernet 0/2
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

interface fastEthernet 0/3
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit
```

------------------------------------------------------------------------

# 🔀 Switch S2

## Basic Configuration

``` cisco
enable
configure terminal
hostname S2
enable secret <ENABLE_SECRET>
service password-encryption

interface vlan 1
ip address 192.168.20.2 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.20.1
```

## Console and SSH

``` cisco
line console 0
password <SWITCH_CONSOLE_PASSWORD>
login
exit

ip domain-name campus.local
username admin privilege 15 secret <SSH_ADMIN_PASSWORD>
crypto key generate rsa
ip ssh version 2

line vty 0 4
login local
transport input ssh
exit
```

## Port Security --- Fa0/1 to Fa0/3

``` cisco
interface range fastEthernet 0/1-3
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit
```

------------------------------------------------------------------------

# 💻 PC Configuration

Configure the following values through:

**Desktop → IP Configuration**

  PC    IP Address        Subnet Mask       Default Gateway
  ----- ----------------- ----------------- -----------------
  PC1   `192.168.10.10`   `255.255.255.0`   `192.168.10.1`
  PC2   `192.168.10.11`   `255.255.255.0`   `192.168.10.1`
  PC3   `192.168.10.12`   `255.255.255.0`   `192.168.10.1`
  PC4   `192.168.20.10`   `255.255.255.0`   `192.168.20.1`
  PC5   `192.168.20.11`   `255.255.255.0`   `192.168.20.1`
  PC6   `192.168.20.12`   `255.255.255.0`   `192.168.20.1`

------------------------------------------------------------------------

# 🧪 Testing

## Connectivity Testing

From PC1:

``` text
ping 192.168.10.1
ping 192.168.20.10
ping 192.168.20.11
ping 192.168.20.12
```

### Result

All tested destinations returned successful replies.

This verifies:

-   PC1 → R1 connectivity
-   Routing from `192.168.10.0/24` to `192.168.20.0/24`
-   PC1 → PC4/PC5/PC6 connectivity

------------------------------------------------------------------------

# 🔑 SSH Testing

From PC1:

``` text
ssh -l admin 192.168.10.2
ssh -l admin 192.168.10.1
```

From PC4:

``` text
ssh -l admin 192.168.20.2
```

SSH was successfully tested against:

-   R1 --- `192.168.10.1`
-   S1 --- `192.168.10.2`
-   S2 --- `192.168.20.2`

------------------------------------------------------------------------

# 🛡️ Port Security

## Verification Commands

``` cisco
show port-security
show port-security address
show port-security interface fastEthernet 0/1
show mac address-table
```

The configuration verifies:

-   Port security: **Enabled**
-   Maximum secure MAC addresses: **1**
-   Sticky MAC learning: **Enabled**
-   Violation action: **Shutdown**

## Port-Security Violation Demonstration

The test procedure was:

1.  Connect PC1 to S1 Fa0/1.
2.  Generate traffic so the switch learns PC1's MAC address.
3.  Disconnect PC1.
4.  Connect another PC to Fa0/1.
5.  Generate traffic.
6.  Verify the port-security status.

Verification:

``` cisco
show port-security interface fastEthernet 0/1
```

The captured result showed:

``` text
Port Security              : Enabled
Port Status                : Secure-shutdown
Violation Mode             : Shutdown
Maximum MAC Addresses      : 1
Sticky MAC Addresses       : 1
Security Violation Count   : 1
```

This demonstrates that the switch detected an unauthorized MAC address
and placed the port into secure-shutdown.

## Recovering the Port

``` cisco
configure terminal
interface fastEthernet 0/1
shutdown
no shutdown
exit
```

------------------------------------------------------------------------

# 🚫 Unused Ports

Unused FastEthernet ports `Fa0/4` through `Fa0/24` were disabled on both
switches:

``` cisco
interface range fastEthernet 0/4-24
shutdown
exit
```

This prevents unused active ports from being available for unauthorized
device connections.

------------------------------------------------------------------------

# 🔍 Verification Commands

The following commands were used during the project:

``` cisco
show running-config
show ip interface brief
show ip ssh
show port-security
show port-security address
show port-security interface fastEthernet 0/1
show mac address-table
```

------------------------------------------------------------------------

# 💾 Saving the Configuration

The final configuration was saved on R1, S1, and S2:

``` cisco
copy running-config startup-config
```

------------------------------------------------------------------------

# 📊 Final Test Results

  Test               Expected Result      Result
  ------------------ -------------------- ---------------
  PC1 → Router       Successful ping      ✅ Successful
  PC1 → PC4          Successful ping      ✅ Successful
  SSH → R1           Successful login     ✅ Successful
  SSH → S1           Successful login     ✅ Successful
  SSH → S2           Successful login     ✅ Successful
  Port Security      Enabled              ✅ Verified
  Sticky MAC         MAC learned          ✅ Verified
  Unauthorized MAC   Violation detected   ✅ Verified
  Unused Ports       Shutdown             ✅ Verified

------------------------------------------------------------------------

# 🔐 Security Controls Implemented

  Security Control           Implementation
  -------------------------- -------------------------------
  Console Security           Console passwords + `login`
  Privileged Access          `enable secret`
  Password Obfuscation       `service password-encryption`
  Remote Administration      SSH v2
  Local SSH Authentication   Local `admin` account
  Port Security              Maximum 1 MAC
  MAC Learning               Sticky MAC
  Violation Response         Shutdown
  Unused Port Protection     Fa0/4--Fa0/24 shutdown

------------------------------------------------------------------------

# 📁 Project Deliverables

-   Cisco Packet Tracer `.pkt` file
-   Network topology screenshot
-   IP addressing table
-   Router configuration screenshots
-   Switch configuration screenshots
-   SSH login screenshot
-   Port-security verification screenshot
-   Port-security violation screenshot
-   Connectivity test screenshots
-   Project report

------------------------------------------------------------------------

# 🧠 Key Learning Outcomes

This project provided practical experience with:

-   Cisco IOS CLI
-   IPv4 addressing
-   Router interface configuration
-   Switch management interfaces
-   Default gateways
-   Console security
-   Privileged EXEC security
-   SSH v2
-   RSA key generation
-   Local user authentication
-   Port security
-   Sticky MAC addresses
-   Security violation handling
-   Unused-port hardening
-   Network troubleshooting
-   Configuration verification and backup

------------------------------------------------------------------------

# 📌 Conclusion

The Secure Campus Network was successfully implemented in Cisco Packet
Tracer. The final network provides connectivity between two IPv4
networks while applying multiple basic security controls.

The project demonstrates secure management through console protection
and SSH, password protection, switch port security with sticky MAC
learning, unauthorized-device detection, and shutdown of unused ports.
Connectivity, SSH access, and the port-security violation scenario were
successfully tested.

## 👤 Project

**Secure Campus Network --- Cisco Packet Tracer Mini Project**

**Author:** P. Sukumar
