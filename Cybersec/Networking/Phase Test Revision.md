# Lecture 1
## To Form a Network
### Endpoints
- Sender
- Receiver

### Intermediary Devices
- Routers, switches, firewalls, etc.

### Medium
- Wire (copper, fiber optics, coaxial)
- Wireless (radio waves, Bluetooth, satellite)

### Network Interface Cards
To connect to the medium, network interface cards are needed (NIC) which are:
- Ethernet interfaces
- Serial Interfaces
- Wireless Antennas
- optical interfaces

### Network Protocols
![](https://i.imgur.com/IABELJs.png)

Messages are transformed into bits for transmission which can be done by encoding these bits into patterns of
- Electric Pulses
- Light
- Microwave Signals

Messages are encapsulated before transmissions denoting the proper source/destination addresses over several layers.

Long messages must be broken into smaller sizes for transmission and each piece sent separately then the receiver will reconstruct the original sent message

Message timing
- Access control | when to begin sending and when to stop
- Flow control | avoid overwhelming the receiver
- Response timeout | how long to wait to receive

Delivery options consist of 
- Unicast
- Multicast
- Broadcast

### Standards
used as measure, norm, or model in comparative evaluations
- Open standards
- internet standards
- electronics and comms. standard org.

### Network Applications

## Open Systems Interconnection Model (OSI Model)
![](https://i.imgur.com/Fq5imo3.jpg)


![](https://i.imgur.com/1m2q1ye.png)

## TCP/IP Model

![](https://i.imgur.com/SwlcmOp.png)


#### IPv4
32 bits
2^32 different addresses = 4294967296
32 bits (4 bytes separated by a dot)
#### IPv6
IPv4 128 Bits


MAC Address 48 Bits (24 for OUI and 24 for Vendor ID)


# Lecture 2

## Securing the Edge Router

### Perimeter Implementations

![](https://i.imgur.com/5IhT7Od.png)

### Areas of Router Security
![](https://i.imgur.com/txMYuSN.png)

### Securing Administrative Access
![](https://i.imgur.com/J9px5wm.png)

### Virtual Login Security
![](https://i.imgur.com/ZG6RbCA.png)


# Lecture 3
## SNMP Versions
### SNMPv1 & SNMPv2
- Provide only simple authentication and do not address encryption
- SNMPv2c is a revised version of v1 introducing community string
- SNMPv2c should only be used in private networks where security is not a major concern

### SNMPv3
- **Message Integrity** - Ensures that a packet has not been tampered with in transit
- **Authentication** - Determines that the message is from a valid source
- **Encryption** - Scrambles the contents of a packet to prevent it from being seen by unauthorized source

## Security Levels
### noAuth
- Authenticates a packet by a string match of the username or community string
### Auth
- Authenticates a packet by using either the Hashed Message Authentication Code (HMAC) with Message Digest 5 (MD5) method or Secure Hash Algorithms (SHA) method. 
### Priv
- Authenticates a packet by using either the HMAC MD5 or HMAC SHA algorithms and encrypts the packet using the Data Encryption Standard (DES), Triple DES (3DES), or Advanced Encryption Standard (AES) algorithms.

## Ensure a device is secure

![](https://i.imgur.com/rwEP01P.png)


# Lecture 4
## TACACS+ And RADIUS Comparison
![](https://i.imgur.com/KLKzjKu.png)

## TACACS+ AUTH Process
![](https://i.imgur.com/kmtNV9G.png)

## RADIUS AUTH Process
![](https://i.imgur.com/hDFzief.png)

## AAA AUTH Overview
![](https://i.imgur.com/uMtawTS.png)

# Lecture 5
## MAC Spoofing & MAC Table Overflow Mitigation

![](https://i.imgur.com/ccETI6P.png)

## Port Security Violation Actions

![](https://i.imgur.com/cHlccdJ.png)


## Layer 2 Security
### Layer 2 Basics
- Layer 2 refers to the Data Link layer in the OSI model
- It involves comm. between devices using MAC addresses

## Layer 2 Attacks
### MAC Address Spoofing
- **What is it?** Impersonating another device by using a fake MAC address. an attacker change their device's MAC to match the MAC of another legit device
- **Risk?** Unauthorized access to the network
- **Mitigation?** Use port security to limit the number of MAC addresses on a port. 

### MAC Address Table Overflow
- **What is it?** Overloading the switch's MAC table to cause a denial of service. 
- **Risk?** Disruption of network services.
- **Mitigation?** Configure port security and limit the number of allowed MAC addresses.

### STP (Spanning Tree Protocol) Manipulation
- **What is it?** Exploiting vulnerabilities in STP to disrupt network topology
- **Risk?** Networking instability or redirection of traffic
- **Mitigation?** Use BPDU (Bridge Protocol Data Units) Guard to prevent unauthorized changes in the STP topology. if port receives BPDU after mitigation, port goes into **root-inconsistent state** and use Root Guard, because it protects the network by preventing switches with lower Bridge Priority values from becoming the root bridge 

### VLAN Attacks
#### Double Tagging
- **What is it?** Attacker sends a double-tagged frame, tricking the switch into forwarding it to a different VLAN
- **Risk?** Unauthorized access to a different VLAN
- **Mitigation?** Change the default native VLAN 

```
Switch3(config)# interface gigabitEthernet 0/14
Switch3(config-if)# switchport trunk native vlan 900
```

#### Switch Spoofing
- **What is it?** An attacker sets up a rogue switch to intercept and forward traffic from multiple VLANs (forces a connection to a rogue switch using a trunk port)
- **Risk?** Unauthorized access and interception of VLAN traffic
- **Mitigation?** Use port security, disable unused ports, and deploy dynamic ARP inspection 

#### Spoofing DTP (Dynamic Trunking Protocol) Messages 
- **What is it?** Attacker sends fake DTP messages to negotiate a trunk list, gaining access to multiple VLANs
- **Risk?** Unauthorized trunking and access to multiple VLANs
- **Mitigation?** configure non-trunk ports as **access ports**, and disable DTP on a **trunk port**

#### Mitigating VLAN Attacks
1. Disable trunking on all access ports
2. Disable auto trunking and manually enable trunking
3. Be sure that the native VLAN is used only for trunk lines and no where else

### DHCP Server Spoofing
- **What is it?** An Attacker sets up a rogue DHCP server to distribute false IP configurations to network clients
- **Risk?** Unauthorized IP assignment, potential for MITM attacks, could direct users to malicious server
- **Mitigation?** Use DHCP Snooping

![](https://i.imgur.com/l5ZKbUO.png)

#### DHCP Snooping (Mitigation)
- **What is it?** A security feature that validates DHCP messages, allowing only legit DHCP servers to assign IPs
- **Role?** Prevents DHCP-based attacks like server spoofing and ensures DHCP traffic integrity.
- **Mitigation?** Enable DHCP snooping on network switches and define trusted DHCP servers 

![](https://i.imgur.com/A4w9SyF.png)

### DHCP Starvation Attack
- **What is it?** overloading a DHCP server by sending a large number of DHCP requests, exhausting available IPs. Broadcasting DHCP requests with **spoofed MAC addresses**
- **Risk?** DoS for legitimate users, network disruption, Preventing anyone else from obtaining an IP address
- **Mitigation?** Implement rate limiting on DHCP requests, config DHCP snooping [[#DHCP Snooping (Mitigation)]], use static IP assignments for critical devices and configure port security

### ARP Spoofing (Poisoning) Attack
- **What is it?** sending false ARP messages to associate an attacker's MAC address with the IP address of another device 
- **Risk?** MITM attacks (Map router IP to attacker MAC address) , traffic interception, network disruption
- **Mitigation?** [[#Dynamic ARP Inspection (DAI)]]

![](https://i.imgur.com/LzLzCOQ.png)

### Gratuitous ARP 
- **What is it?** Legitimate ARP message where a device announces its own IP-to-MAC mapping
- **Use Cases?** Updating ARP caches, detect IP conflicts, inform switches of the MAC of the machine on a given switch port, preload the ARP tables of all other local hosts, can be used in HSRP (Hot Standby Router Protocol) is an IP routing redundancy protocol designed to allow for transparent failover at the first-hop IP router
- **Security Implications?** Can be exploited for ARP spoofing attacks if used maliciously
- **Mitigation?** Monitor and filter gratuitous ARP messages, use DAI [[#Dynamic ARP Inspection (DAI)]], and maintain secure ARP configs. 

### Dynamic ARP Inspection (DAI)
- **Purpose?** DAI is designed to protect a network against ARP spoofing and other ARP-based attacks
- **How it works?**
	- **ARP Inspection**: DAI monitors and inspects ARP packets within the network
	- **Verification**: DAI validates ARP packets to ensure that the mapping between IP and MAC is legit
	- **Filtering**: Any ARP packets that are deemed sus or unauthorized are filtered or blocked by the network switch. 
	- Determines the validity of packets by performing an IP-to-MAC address binding inspection stored in a trusted database, **(DHCP snooping binding database)** [[#DHCP Snooping (Mitigation)]] before forwarding the packet to the appropriate destination. 
- Prevent ARP Spoofing? [[#ARP Spoofing (Poisoning) Attack]]
- **DAI in DHCP Envir.**
	- DAI associates a trust state with each interface on the switch
	- When DAI is enabled, all ports will be untrusted
	- Packets arriving on trusted interfaces bypass all dynamic ARP inspection validation checks, and those arriving on untrusted interfaces undergo the dynamic ARP inspection validation process.
- **DAI in NON-DHCP Envir.**
	- No DHCP snooping binding database
	- DAI can validate ARP packets against a **user defined ARP ACL** to map hosts with a statically configured IP address to their MAC address.

![](https://i.imgur.com/C55vqtG.png)

## Cisco Switched Port Analyzer
### Traffic Analysis
- A SPAN port mirrors traffic to another port where a monitoring device is connected
- Without this, it can be difficult to track hackers after they have entered the network
- SPAN is configured and applies to a single switch 

- [[#Cisco Switched Port Analyzer]] a method for analyzing **multiple switches**
- The destination is on a switch that is remote to the other source ports, which are on different switches.
- An [[#RSPAN]] port mirrors traffic to another port on another switch where a sniffer or IDS sensor is connected.
- This allows more switches to be monitored with a single sniffer or IDS.uy

### RSPAN
![](https://i.imgur.com/QPCj96C.png)

## Layer 2 Guideline
- Use secure management (SSH, access-class)
- Disable unneeded services.
- Administratively shut down any inactive ports.
- Limit management access to administrators only.
- Use SNMP v3 if network management is desired because information transmitted is encrypted.
- Don’t send user data over a native VLAN using trunk ports.
- Engage Spanning-Tree Protocol protection features such as BPDU Guard and Root Guard. [[#STP (Spanning Tree Protocol) Manipulation]]
- Turn on DHCP snooping and DAI for protection against man-in-the-middle type attacks. [[#Dynamic ARP Inspection (DAI)]] and [[#DHCP Snooping (Mitigation)]]

## VLAN Practices
- Always use a dedicated, unused native VLAN ID for trunk ports
- Do not use VLAN 1 for anything, because it is the default VLAN on switches which means if you have any enabled and unconfigured ports on a switch, someone can plug in with immediate access to VLAN 1
- Disable all unused ports and put them in an unused VLAN
- Manually configure all trunk ports and disable DTP on trunk ports [[#Spoofing DTP (Dynamic Trunking Protocol) Messages]]
- Configure all non-trunking ports with switchport mode access
	- Don't use dynamic ports

## Port Security Summary
### Protect Against
- MAC Spoofing
- DCHP Starvation
- Double Tagging (port security must be configured on access links) 
- ARP Spoofing
- Switch Spoofing
- MAC Address Table Overflow (maximum no. of MAC addresses per port)

### Configure Switch Security Summary
- [[#DHCP Snooping (Mitigation)]]
- [[#Dynamic ARP Inspection (DAI)]]
- Port Security
- BPDU Guard and Root Guard [[#STP (Spanning Tree Protocol) Manipulation]]
- [[#Cisco Switched Port Analyzer]]

# Lecture 6
## Well Known Ports Addresses
![](https://i.imgur.com/30NWB1K.png)

## Access Control Lists (ACLs)
### Overview
#### Definition
- ACLs are sets of rules used to control network traffic by permitting denying data packets based on defined criteria.

#### Configuration
- One ACL per protocol
- One ACL per direction
- One ACL per interface

### Types of ACLs
#### Standard ACLs
- Based on source IP
```
(Guard: Where are you coming from?
 Guy: From the lake Town. 
 Guard: STOP!)
```
- Generally used to permit or deny entire protocols
##### Configuration Mode
###### Numbered
- 1 till 99
###### Named
- No spaces and punctuation
##### Operation
- Should be located as close to the destination as possible
#### Extended ACLs
- Consider both source and destination IP and port address
- Provides more detailed control by looking at the type of data and specific ports
```
Guard: Where are you coming from? Where are you going? What are you going to do there?)
Guy: From Lake Town, To Wood Side, For Hunting
Guard: STOP!
```
##### Configuration Mode
###### Numbered
- 100 till 199
###### Named
- No spaces and punctuation
##### Operation
- Should be located as close as possible to the source of the traffic to be filtered
### Configuration Structure
#### Standard
##### Numbered
```
access-list 1-99 deny/permit <source> <wildcard>
access-list 1 permit 192.168.1.0 0.0.0.255
```
##### Named
```
ip access-list standard <name> deny/permit <source> <wildcard>
ip access-list standard myList permit 192.168.1.0 0.0.0.255
```
#### Extended
##### Numbered
```
access-list 100-199 deny/permit <protocol> <network> <wildcard> <network> <wildcard> eq <port>
access-list 100 deny tcp 192.168.1.1 0.0.0.0 192.168.2.0 0.0.0.255 eq 80
```
##### Named
```
ip access-list extended <name> deny/permit <protocol> <network> <wildcard> <network> <wildcard> eq <port>
ip access-list extended myList deny tcp 192.168.1.1 0.0.0.0 192.168.2.0 0.0.0.255 eq 80
```
### Applications
#### Traffic Filtering
- ACLs are commonly used to filter traffic entering or leaving a network based on specific criteria
#### Security Policies
- ACLs help implement and enforce security policies by controlling network access
## Firewalls
### Overview
- Definition: A firewall is a network security device that monitors and controls incoming and outgoing network traffic based on predetermined security rules (accepts, rejects, drops)
- Hardware (appliance-based) or software (host-based)
	- Appliance-based or network-based firewall might have two or more Network Interface Cards (NICs)
	- Accepts: allow traffic
	- Reject: block traffic but reply with "unreachable error"
	- Drop: block traffic no reply
- it establishes a barrier between secured internal networks and outside untrusted networks (the internet)
- Before firewalls, ACLs were performed residing on routers but ACLs cannot determine the nature of the packet it is blocking and doesn't have the capacity to keep threats out of the network, hence firewall was introduced

### How Firewall Works
- Firewall match the traffic against the rule set defined
- If matched, associate action is applied to the traffic
- It's always better to set a rule on outgoing traffic in order to achieve more security

### Generation of Firewall
#### First Generation - Packet Filtering Firewall
- Examines individual data packets by isolation
- Allows or blocks packets based on predefined rules (source and destination IP, protocols, ports)
- It analyses traffic at the transport protocol layer (mainly uses first 3 layers)
- basic and efficient
- lacks awareness of the state of connections

#### Second Generation - Stateful Inspection Firewall
##### How it Works
- Tracks the state of active connections such as TCP streams
- Makes decisions based on the context of the traffic

##### Advantages
- Smarter decision-making
- Understands the state of connections (e.g., established, related)

#### Third Generation - Application Layer Firewall
##### How it Works
- Analyzes data at any OSI model layer up to application layer (layer 7)
- Understands specific applications and their protocols

##### Capabilities
- Provide more detailed control
- Can filter based on application type (e.g., allowing or blocking specific applications)
- Also recognizes when certain application and protocols (e.g., HTTP, FTP) are being misused

#### Next-Generation Firewall (NGFW)
##### How it Works
- Combines traditional firewall features with advanced capabilities
- Includes IPS, deep packet inspection, application inspection, SSL/SSH inspection, and many more

##### Advanced Features
- Content filtering, antivirus, VPN support
- Focuses on threat intel. and advanced security features. 

#### Zone-Based Firewall
##### How it Works
- Divides the network into security zones.
- DMZ which is a subnetwork that works between private networks and public.
	- It is a barrier between trusted and untrusted networks of org. private and public.
	- it acts as a protective layer that prevents external users from accessing company data
- Controls traffic between zones based on policies

##### Detailed Control
- Allows for more subtle control between different segments of network
- Enhances security by isolating zones


# Lecture 7
## IDS & IPS
![](https://i.imgur.com/ffepJiw.png)


# Lecture 8

## Issues with Layer 1 Redundancy
- MAC Database Instability
- Multiple Frame Transmission
- Broadcast Storms
## Port Roles 
![](https://i.imgur.com/emrh7yW.png)

![](https://i.imgur.com/3hId192.png)

![](https://i.imgur.com/uJDiNWQ.png)

## Link Aggregation Constraints

![](https://i.imgur.com/oo0zXS2.png)

## Router Redundancy

![](https://i.imgur.com/cEhXEOF.png)

# Lecture 10
## WLAN Concepts

### Types of Wireless Networks
#### Wireless Personal-Area Network (WPAN)
- Low power and short-range (20-30ft or 6-9 meters)
- Based on IEEE 802.15 standard and 2.4 GHz frequency
- Bluetooth and ZigBee are WPAN examples

#### Wireless Local Area Network (WLAN)
- Medium sized networks up to 300 feet.
- Based on IEEE 802.11 standard and 2.4 or 5.0 GHz frequency

#### Wireless Metropolitan Area Network (WMAN)
- Large geographic area such as city or district
- uses specific licensed frequencies

#### Wireless Wide Area Network (WWAN)
- Extensive geographic area for national or global comm. 
- uses specific licensed frequencies

### Wireless Technologies
- WiMAX (Worldwide Interoperability for Microwave Access) - alternative broadband wired internet connections. IEEE 802.16 WLAN standard for up to 30 miles
- Bluetooth
	- Bluetooth Low Energy (BLE)
	- Bluetooth Basic Rate/Enhanced Rate (BR/EDR)
- Cellular Broadband
	- Global System of Mobile (GSM)
	- Code Division Multiple Access (CDMA)
- Satellite Broadband

### 802.11 Standards
- 802.11 - 2.4 GHz - Data rate up to 2 Mb/s
- 802.11a - 5 GHz - Data rate up to 54 Mb/s and Not interoperable with 802.11b or 802.11g ^cad941
- 802.11b - 2.4 GHz - Data rate up to 11 Mb/s and longer range than [[#^cad941]] 802.11a and better able to penetrate building structures
- 802.11g - 2.4 GHz - Data rate up to 54 Mb/s and backward compatible with 802.11b
- 802.11n - 2.4 and 5 GHz - Data rate up to 150 - 600 Mb/s and require multiple antennas with MIMO (Multiple Input Multiple Output) technology
- 802.11ac - 5 GHz - Data rate up to 450 Mb/s - 1.3 Gb/s and supports up to eight antennas
- 802.11ax - 2.4 and 5 GHz - High - Efficiency Wireless (HEW) and Capable of using 1 GHz and 7 GHz frequencies

### WLAN Components
- Wireless NICs
- Wireless Home Router
- Wireless Access Point
	- **Autonomous APs** - Standalone devices configured through a CLI or GUI 
	- **Controller-based APs** - Also known as lightweight APs (LAPs). Use Lightweight Access Point Protocol (LWAPP) to communicate with a LWAN controller (WLC). 
- CSMA/CA is Carrier Sense Multiple Access with Collision Avoidance. WLANS are half-duplex and client cannot hear while it is sending so it makes it impossible to detect collision. CSMA/CA determines how and when to send data. 

### Channel Management
#### Frequency Channel Saturation Mitigation
- Direct-Sequence Spread Spectrum (DSSS)
- Frequency-Hopping Spread Spectrum
- Orthogonal Frequency-Division Multiplexing

#### Channel Selection
- The 2.4 GHz band is subdivided into multiple channels each allotted 22 MHz bandwidth, and separated from the next channel by 5 MHz
- A best practice for 802.11b/g/n WLANs requiring multiple APs is to use non-overlapping channels such as 1, 6, and 11.

![](https://i.imgur.com/hEBFC7i.png)


- For the 5GHz standards 802.11a/n/ac, there are 24 channels. Each channel is separated from the next channel by 20 MHz
- Non-overlapping channels are 36, 48, and 60.

![](https://i.imgur.com/Tg85OZI.png)

### Wireless Security Overview
#### Threats
- Interception of data
- Wireless intruders
- DoS attacks
- rogue APs

#### Secure WLANs
- SSID Cloaking
- MAC Address Filtering

##### 802.11 Original Authentication Methods
- Open System Authentication
- Shared Key Authentication
	- Wired Equivalent Privacy (WEP)
	- Wi-Fi Protected Access (WPA)
	- WPA2
	- WPA3
- WPA and WPA2 include two encryption protocols 
	- Temporal Key Integrity Protocol (TKIP)
	- Advanced Encryption Standard (AES)
# Topics to Be Well-Revised

![](https://i.imgur.com/mikH4N2.png)

# Questions
ACL config - 1 question
wireless devices - 2 question
how to secure network - 1 question
redundancy - 1 question
unknown - 1 question

---
#networking #phasetest #test #revision 