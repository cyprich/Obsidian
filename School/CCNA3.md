# CCNA3 Questions

Questions from each CCNA3 module from Netacad

## Module 1: Single-Area OSPFv2 Concepts

### 1.1 - OSPF Features and Characteristics

Which of the following OSPF components is associated with the neighbor table?

- ~~Dijkstra's algorithm~~
- ~~Link-State database~~
- ~~Routing protocol messages~~
- Adjacency database
- ~~Forwarding database~~

Which of the following OSP components is responsible for computing the cost of each route?

- Dijkstra's algorithm
- ~~Link-State database~~
- ~~Routing protocol messages~~
- ~~Adjacency database~~
- ~~Forwarding database~~

Which of the following OSPF components is associated with the topology table?

- ~~Dijkstra's algorithm~~
- Link-State database
- ~~Routing protocol messages~~
- ~~Adjacency database~~
- ~~Forwarding database~~

Which of the following OSPF components is associated with the routing table?

- ~~Dijkstra's algorithm~~
- ~~Link-State database~~
- ~~Routing protocol messages~~
- ~~Adjacency database~~
- Forwarding database

Which is the correct order in the steps for Link-State operation?

1. Establish Neighbor Adjacencies
2. Exchange Link-State Advertisements
3. Build the Topology Table
4. Execute the SPF Algorithm
5. Choose the Best Route

### 1.2 OSPF Packets

Which of the following OSPF packets contains an abbreviated list of the LSDB of the sending router?

- ~~Type 1: Hello packet~~
- Type 2: DBD packet
- ~~Type 3: LSR packet~~
- ~~Type 4: LSU packet~~
- ~~Type 5: LSAck packet~~

Which of the following OSPF packets is used by routers to announce new information?

- ~~Type 1: Hello packet~~
- ~~Type 2: DBD packet~~
- ~~Type 3: LSR packet~~
- Type 4: LSU packet
- ~~Type 5: LSAck packet~~

Which of the following OSPF packets is used by routers to request more information?

- ~~Type 1: Hello packet~~
- ~~Type 2: DBD packet~~
- Type 3: LSR packet
- ~~Type 4: LSU packet~~
- ~~Type 5: LSAck packet~~

Which of the following OSPF packets is responsible for establishing and maintaining adjacency with other OSPF routers?

- Type 1: Hello packet
- ~~Type 2: DBD packet~~
- ~~Type 3: LSR packet~~
- ~~Type 4: LSU packet~~
- ~~Type 5: LSAck packet~~

Which of the following OSPF packets is used to confirm receipt of an LSA?

- ~~Type 1: Hello packet~~
- ~~Type 2: DBD packet~~
- ~~Type 3: LSR packet~~
- ~~Type 4: LSU packet~~
- Type 5: LSAck packet

Which of the following is used with the Hello Packet to uniquely identify the originating router?

- ~~Hello interval~~
- Router ID
- ~~Designated Router ID~~
- ~~Network Mask~~
- ~~Dead Interval~~

### 1.3 OSPF Operation

During this OSPF state on multiaccess network, the routers elect a Designated Router (DR) and a Backup Designated Router (BDR)

- ~~Down State~~
- ~~Init State~~
- Two-way State
- ~~ExStart State~~
- ~~Exchange State~~
- ~~Loading State~~
- ~~Full State~~

During this OSPF state, routers send each other DBD packets

- ~~Down State~~
- ~~Init State~~
- ~~Two-way State~~
- ~~ExStart State~~
- Exchange State
- ~~Loading State~~
- ~~Full State~~

An OSPF router enters this state when it has received a Hello packet from a neighbor, containing the sending Router ID

- ~~Down State~~
- Init State
- ~~Two-way State~~
- ~~ExStart State~~
- ~~Exchange State~~
- ~~Loading State~~
- ~~Full State~~

During this OSPF state on point-to-point networks, the router decide which router initializes the exchange of DBD packets

- ~~Down State~~
- ~~Init State~~
- ~~Two-way State~~
- ExStart State
- ~~Exchange State~~
- ~~Loading State~~
- ~~Full State~~

During this OSPF state, routers have converged link-state databases

- ~~Down State~~
- ~~Init State~~
- ~~Two-way State~~
- ~~ExStart State~~
- ~~Exchange State~~
- ~~Loading State~~
- Full State

During this OSPF state, no Hello packets are received

- Down State
- ~~Init State~~
- ~~Two-way State~~
- ~~ExStart State~~
- ~~Exchange State~~
- ~~Loading State~~
- ~~Full State~~

During this OSPF state, routers are processed using the SPF algorithm

- ~~Down State~~
- ~~Init State~~
- ~~Two-way State~~
- ~~ExStart State~~
- ~~Exchange State~~
- ~~Loading State~~
- Full State

### 1.4 Module Quiz

What is a function of OSPF hello packets?

- ~~To send specifically requested link-state records~~
- To discover neighbors and build adjacencies between them
- ~~To ensure database synchronization between routers~~
- ~~To request specific link-state records from neighbor routers~~

Which OSPF packet contains the different types of link-state advertisements?

- ~~Hello~~
- ~~DBD~~
- ~~LSR~~
- LSU
- ~~LSAck~~

Which three statements describe features of the OSPF topology table? (Choose three.)

- It is a link-state database that represents the network topology - OK
- ~~Its contents are the result of running the SPF algorithm~~
- When converged, all routers in an are have identical topology tables - OK
- ~~The topology table contains feasible successor routes~~
- The table can be viewed via the `show ip ospf database` command
- ~~After convergence, the table only contains the lowest cost route entries for all known networks~~

What does an OSPF area contain?

- ~~Routers that share the same Router ID~~
- ~~Routers whose SPF trees are identical~~
- Routers that have the same link-state information in their LSDBs
- ~~Routers that share the same process ID~~

A router is participating in an OSPFv2 domain. What will always happen if the dead interval expires before the router receives a hello packet from an adjacent DROTHER OSPF router?

- ~~OSPF will run a new DR/DBR election~~
- ~~SPF will run and determine which neighbor router is "down"~~
- ~~A new dead interval timer of 4 times the hello interval will start~~
- OSPF will remove that neighbor from the router link-state database

What is the order of packet types used by an OSPF router to establish convergence?

1. Hello
2. DBD
3. LSR
4. LSU
5. LSAck

What is a feature of the OSPF routing protocol?

- ~~The SPF algorithm chooses the best path based on 30-second updates~~
- ~~OSPF authentication is configured in the same way on IPv4 and IPv6 networks~~
- It scales well in both small and large networks
- ~~Routers can be grouped into autonomous systems to support a hierarchical system~~

What is used to create the OSPF neighbor table?

- Adjacency database
- ~~Link-state database~~
- ~~Forwarding database~~
- ~~Routing table~~

What is identical on all OSPF routers within a single area?

- ~~Routing table~~
- Link-state database
- ~~Neighbor table~~
- ~~Static routes~~

What function is performed by the OSPF designated router?

- ~~Redistribution of external routes into OSPF~~
- Dissemination of LSAs
- ~~Maintaining the link-state database~~
- ~~Summarizing routes between areas~~

What are two reasons for creating an OSPF network with multiple areas? (Choose two.)

- ~~To provide areas in the network for routers that are not running OSPF~~
- ~~To ensure that an area is used to connect the network to the Internet~~
- To reduce SPF calculations
- To reduce use of memory and processor resources
- ~~To simplify configuration~~

At which OSPF state are neighbor routers converged and able to exchange routing updates?

- ~~Two-Way~~
- ~~ExStart~~
- ~~Exchange~~
- Full

The OSPF hello timer has been set to 15 seconds on a router in a point-to-point network. By default, what is the dead interval on this router?

- ~~15 seconds~~
- ~~30 seconds~~
- ~~45 seconds~~
- 60 seconds

What happens immediately after two OSPF routers have exchanged hello packets and have formed a neighbor adjacency?

- ~~They exchange DBD packets in order to advertise parameters such as hello and dead intervals~~
- ~~They negotiate the election process if they are on a multiaccess network~~
- ~~They request more information about their databases~~
- They exchange abbreviated lists of their LSDBs

Which statement is correct about multiarea OSPF?

- ~~OSPF can consolidate a fragmented OSPF area into on large area~~
- ~~All routers are in one area called the backbone area (area 0)~~
- Arranging routers into areas partitions a large autonomous system in order to lighten the load on routers
- ~~OSPF multiarea increases the frequency of SPF calculation~~

## Module 2: Single-Area OSPFv2 Configuration

### 2.1 OSPF Router ID

True or false? In the `router ospf PROCESS-ID` command, the `PROCESS-ID` value, which can be any number between 1 and 65535, is locally significant. It must be the same on all routers in the OSPF area

- ~~True~~
- False

Which of the following applies to the Router ID? (Choose two)

- ~~The Router ID cannot be defined by an administrator~~
- ~~The Router ID is not used to determine the BDR~~
- The Router ID is used to determine the DR
- The Router ID uniquely identifies the router
- ~~The router ID is not required~~

Which of the following is the order of precedence for choosing the Router ID?

1. Router ID that is explicitly configured
2. Highest IPv4 loopback address
3. Highest active configured IPv4 address

### 2.7 Module Quiz

Which criterion is preferred by the router to choose a router ID?

- ~~The IP address of the highest configured loopback interface on the router~~
- ~~The IP address of the highest active interface on the router~~
- The `router-id ID` command
- ~~The IP address of the highest active OSPF-enabled interface~~

Which wildcard mask would be used to advertise the 192.168.5.96/27 network as part of an OSPF configuration

- ~~0.0.0.32~~
- 0.0.0.31
- ~~255.255.255.224~~
- ~~255.255.255.223~~

The following three networks are directly connected to an OSPF router  
192.168.0.0/24, 192.168.1.0/24, 192.168.2.0/24. Which OSPF network command would advertise only the 192.168.1.0 network to neighbors?

- `network 192.168.1.0 0.0.0.255 area 0`
- ~~`network 192.168.0.0 0.0.15.255 area 0`~~
- ~~`network 192.168.1.0 255.255.255.255 area 0`~~
- ~~`network 192.168.1.0 0.0.0.0 area 0`~~

Which three parameters should match in order for a pair of routers to form an adjacency when running OSPFv2? (Choose three.)

- ~~Router ID~~
- ~~OSPFv2 process number~~
- OSPFv2 type of network
- Hello timer
- ~~Interface priority~~
- Subnet mask

What are two features of the OSPF routing protocol? (Choose two.)

- ~~Automatically summarizes networks at the classful boundaries~~
- ~~Has an administrative distance of 100~~
- Calculates its metric using bandwidth
- Uses Dijkstra's algorithm to build the SPF tree
- ~~Used primarily as an EGP~~

A router with two LAN interfaces, two WAN interface, and one configured loopback interface is operating with OSPF as its routing protocol. What does the router OSPF process use to assign the router ID?

- ~~The IP address of the interface that is configured with priority 0~~
- ~~The OSPF area ID that is configured on the interface with the highest IP address~~
- The loopback interface IP address
- ~~The highest IP address on the LAN interfaces~~
- ~~The highest IP address that is configured on the WAN interfaces~~

Which verification command would identify the specific interfaces on a router that were configured with the `passive-interface` command?

- ~~`show ip eigrp neighbors`~~
- `show ip protocols`
- ~~`show ip interface brief`~~
- ~~`show ip route eigrp`~~

Which command, if applied on an OSPF router, would give a Gigabit Ethernet interface a lower cost than a Fast Ethernet interface?

- ~~`bandwidth 100` on interface~~
- `auto-cost reference-bandwidth 1000` in process
- ~~`ip ospf cost 100` on interface~~
- ~~`ip ospf priority 1` on interface~~

A network administrator has just changed the Router ID on a router that is working in an OSPFv2 environment. What should the administrator do to reset the adjacencies and use the new Router ID?

- ~~Configure the network statements~~
- ~~Change the interface priority~~
- Issue the `clear ip ospf process` privileged mode command
- ~~Change the OSPFv2 process ID~~

Which command can be used to view the OSPF hello and dead time intervals?

- ~~`show ip protocols`~~
- ~~`show ip ospf neighbor`~~
- `show ip ospf interface`
- ~~`show ip ospf route`~~

What does the SPF algorithm consider to be the best path to a network?

- ~~The path with the least number of hops~~
- ~~The path with the smallest delays~~
- The path that includes the fastest cumulative bandwidth links
- ~~The path that includes the fastest single bandwidth link~~

What is one use of the Router ID in OSPF routing?

- ~~The Router ID indicates the router priority value~~
- ~~The Router ID identifies the OSPF area~~
- The Router ID can be used to break a tie in the election process
- ~~The Router ID indicates the highest IPv4 address of all routers that are participating in OSPF routing~~

What is the first criterion used by OSPF routers to elect a DR?

- Highest priority
- ~~Highest IP address~~
- ~~Highest Router ID~~
- ~~Highest MAC address~~

Which command could be used on a router to ensure that an OSPF adjacency is formed with another router?

- ~~`show ip route`~~
- ~~`show ip protocols`~~
- ~~`show ip ospf interface`~~
- ~~`show ip ospf neighbor`~~
- `show ip interface brief`

A router in an OSPF enterprise network has a default static route that has been configured via the interface that connects to the ISP. Which command would the network administrator apply on this router so that other routers in the OSPF network will use this default route?

## Module 3: Network Security Concepts

### 3.1 Current State of Cybersecurity

Which security term is used to describe anything of value to the organization? It includes people, equipment, resources, and data

- ~~Vulnerability~~
- ~~Exploit~~
- Asset
- ~~Risk~~

Which security term is used to describe a weakness in a system, or its design, that could be exploited by a threat?

- Vulnerability
- ~~Exploit~~
- ~~Asset~~
- ~~Risk~~

Which security term is used to describe a potential danger to a company's assets, data, or network functionality?

- ~~Vulnerability~~
- ~~Exploit~~
- Threat
- ~~Risk~~

Which security term is used to describe a mechanism that takes advantage of a vulnerability?

- Exploit
- ~~Threat~~
- ~~Risk~~
- ~~Mitigation~~

Which security term is used to describe the counter-measure for a potential threat or risk?

- ~~Vulnerability~~
- ~~Exploit~~
- ~~Asset~~
- Mitigation

Which security term is used to describe the likelihood of a threat to exploit the vulnerability of an asset, with the aim of negatively affecting an organization?

- ~~Vulnerability~~
- ~~Exploit~~
- ~~Threat~~
- Risk

### 3.2 Threat Actors

Which type of hacker is described in the scenario: After hacking into ATM machines remotely using a laptop, I worked with ATM manufacturers to resolve the security vulnerabilities that I discovered

- ~~White Hat~~
- Gray Hat
- ~~Black Hat~~

Which type of hacker is described in the scenario: From my laptop, I transferred $10 million to my bank account using victim account numbers and PINs after viewing recordings of victims entering the numbers

- ~~White Hat~~
- ~~Gray Hat~~
- Black Hat

Which type of hacker is described in the scenario: My job is to identify weaknesses in my company's network

- White Hat
- ~~Gray Hat~~
- ~~Black Hat~~

Which type of hacker is described in the scenario: I used malware to compromise several corporate systems to steal credit card information. I then sold that information to the highest bidder

- ~~White Hat~~
- ~~Gray Hat~~
- Black Hat

Which type of hacker is described in the scenario: During my research for security exploits, I stumbled across a security vulnerability on a corporate network that I am authorized to access

- White Hat
- ~~Gray Hat~~
- ~~Black Hat~~

Which type of hacker is described in the scenario: It is my job to work with technology companies to fix a flaw with DNS

- White Hat
- ~~Gray Hat~~
- ~~Black Hat~~

### 3.3 Threat Actor Tools

Which penetration testing tool uses algorithm schemas to encode the data, which then prevents access to the data?

- ~~Packet Sniffers~~
- Encryption Tools
- ~~Vulnerability Exploration Tools~~
- ~~Forensic Tools~~
- ~~Debuggers~~

Which penetration testing tool is used by black hats to reverse engineer binary files when writing exploits? They are also used by white hats when analyzing malware.

- ~~Packet Crafting Tools~~
- ~~Rootkit Detectors~~
- ~~Vulnerability Exploitation Tools~~
- ~~Forensic Tools~~
- Debuggers

Which penetration testing tool is used to probe and test a firewall's robustness?

- Packet Crafting Tools
- ~~Encryption Tools~~
- ~~Rootkit Detectors~~
- ~~Forensic Tools~~
- ~~Debuggers~~

Which penetration testing tool is used by white hat hackers to sniff out any trace of evidence existing in a computer?

- ~~Fuzzers to Search Vulnerabilities~~
- ~~Encryption Tools~~
- ~~Packet Sniffers~~
- Forensic Tools
- ~~Debuggers~~

Which penetration testing tool identifies whether a remote host is susceptible to a security attack?

- ~~Packet Sniffers~~
- ~~Encryption Tools~~
- Vulnerability Exploitation Tools
- ~~Forensic Tools~~
- ~~Debuggers~~

### 3.4 Malware

Which malware executes arbitrary code and install copies of itself in the memory of the infected computer? The main purpose of this malware is to automatically replicate from system to system across the network

- ~~Adware~~
- ~~Rootkit~~
- ~~Spyware~~
- ~~Virus~~
- Worm

Which malware is non-self-replicating type of malware? It often contains malicious code that is designed to look like something else, such as a legitimate application or file. It attacks the device from withing

- ~~Adware~~
- ~~Rootkit~~
- ~~Spyware~~
- Trojan Horse
- ~~Worm~~

Which malware is used to gather information about a user and then, without the user's consent, sends the information to another entity?

- ~~Adware~~
- ~~Rootkit~~
- Spyware
- ~~Virus~~
- ~~Worm~~

Which malware typically displays annoying pop-ups to generate revenue for its author?

- Adware
- ~~Rootkit~~
- ~~Spyware~~
- ~~Virus~~
- ~~Worm~~

Which malware is installed on a compromised system and provides privileged access to the threat actor?

- ~~Adware~~
- ~~Virus~~
- ~~Spyware~~
- Rootkit
- ~~Worm~~

Which malware denies access to the infected computer system and demands payment before the restriction is removed?

- ~~Adware~~
- ~~Rootkit~~
- ~~Spyware~~
- ~~Virus~~
- Ransomware

### 3.5 Common Network Attacks

What type of attack is tailgating?

- ~~Reconnaissance~~
- ~~Access~~
- ~~DoS~~
- Social Engineering

What type of attack is a password attack?

- ~~Reconnaissance~~
- Access
- ~~DoS~~
- ~~Social Engineering~~

What type of attack is port scanning?

- Reconnaissance
- ~~Access~~
- ~~DoS~~
- ~~Social Engineering~~

What type of attack is man-in-the-middle?

- ~~Reconnaissance~~
- Access
- ~~DoS~~
- ~~Social Engineering~~

What type of attack is address spoofing?

- ~~Reconnaissance~~
- Access
- ~~DoS~~
- ~~Social Engineering~~

### 3.6 IP Vulnerability and Threats

Which attack is being used when threat actors position themselves between a source and destination to transparently monitor, capture, and control the communication?

- ~~Address Spoofing Attack~~
- ~~Amplification and Reflection Attacks~~
- ~~ICPM attack~~
- MiTM Attack
- ~~Session Hijacking~~

Which attack is being used when threat actors gain access to the physical network, and them use an MiTM attack to capture and manipulate legitimate user's traffic?

- ~~Address Spoofing Attack~~
- ~~Amplification and Reflection Attacks~~
- ~~ICPM attack~~
- ~~MiTM Attack~~
- Session Hijacking

Which attack is being used when threat actors initiate a simultaneous, coordinated attack from multiple source machines?

- ~~Address Spoofing Attack~~
- Amplification and Reflection Attacks
- ~~ICPM attack~~
- ~~MiTM Attack~~
- ~~Session Hijacking~~

Which attack is being used when threat actors use ping to discover subnets and hosts on a protected network, to generate flood attacks, and to alter host routing tables?

- ~~Address Spoofing Attack~~
- ~~Amplification and Reflection Attacks~~
- ICPM attack
- ~~MiTM Attack~~
- ~~Session Hijacking~~

Which attack is being used when a threat actor creates packets with false source IP address information to either hide the identity of the sender, or to pose as another legitimate user?

- Address Spoofing Attack
- ~~Amplification and Reflection Attacks~~
- ~~ICPM attack~~
- ~~MiTM Attack~~
- ~~Session Hijacking~~

### 3.7 TCP and UDP Vulnerabilities

Which attack exploits the three-way handshake?

- ~~TCP reset attack~~
- ~~UDP flood attack~~
- TCP SYN Flood attack
- ~~DoS attack~~
- ~~TCP session hijacking~~

Two hosts have established TCP connection and are exchanging data. A threat actor sends a TCP segment with the RST bit to both hosts informing them to immediately stop using the TCP connection. Which attack is this?

- TCP reset attack
- ~~UDP flood attack~~
- ~~TCP SYN Flood attack~~
- ~~DoS attack~~
- ~~TCP session hijacking~~

Which attack is being used when the threat actor spoof the IP address of one host, predicts the next sequence number, and sends an ACK to the other host?

- ~~TCP reset attack~~
- ~~UDP flood attack~~
- ~~TCP SYN Flood attack~~
- ~~DoS attack~~
- TCP session hijacking

A program sends a flood of UDP packets from a spoofed host to a server on the subnet sweeping though all the known UPD ports lookin for closed ports. This will cause the server to reply with an ICMP port unreachable message. Which attack is this?

- ~~TCP reset attack~~
- UDP flood attack
- ~~TCP SYN Flood attack~~
- ~~DoS attack~~
- ~~TCP session hijacking~~

### 3.9 Network Security Best Practices

Which network security device ensures that internal traffic can go out and come back, but external traffic cannot initiate connections to inside hosts?

- ~~VPN~~
- ASA Firewall
- ~~IPS~~
- ~~ESA/WSA~~
- ~~AAA Server~~

Which network security device contains a secure database of who is authorized to access and manage network devices?

- ~~VPN~~
- ~~ASA Firewall~~
- ~~IPS~~
- ~~ESA/WSA~~
- AAA Server

Which network security device filters known and suspicious internet malware sites?

- ~~VPN~~
- ~~ASA Firewall~~
- ~~IPS~~
- ESA/WSA
- ~~AAA Server~~

Which network security device is used to provide secure services with corporate sites and remote access support for remote users using secure encrypted tunnels?

- VPN
- ~~ASA Firewall~~
- ~~IPS~~
- ~~ESA/WSA~~
- ~~AAA Server~~

Which network security device monitors incoming and outgoing traffic looking for malware, network attack signatures, and if it recognizes a threat, it can immediately stop it?

- ~~VPN~~
- ~~ASA Firewall~~
- IPS
- ~~ESA/WSA~~
- ~~AAA Server~~

### 3.10 Cryptography

Which encryption method repeats an algorithm process three times and is considered very trustworthy when implemented using very short key lifetimes?

- ~~Rivest Cipher~~
- Triple DES
- ~~Block Cipher~~
- ~~Data Encryption Standard~~
- ~~Stream Cipher~~

Which encryption method encrypts plaintext one byte or one bit at a time?

- ~~Rivest Cipher~~
- ~~Triple DES~~
- ~~Block Cipher~~
- ~~Data Encryption Standard~~
- Stream Cipher

Which encryption method uses the same key to encrypt and decrypt data?

- ~~Triple DES~~
- Symmetric
- ~~Block Cipher~~
- ~~Data Encryption Standard~~
- ~~Asymmetric~~

Which encryption method is a stream cipher and is used to secure web traffic in SSL and TLS?

- Rivest Cipher
- ~~Triple DES~~
- ~~Symmetric~~
- ~~Block Cipher~~
- ~~Data Encryption Standard~~

### 3.11 Module Quiz

The IT department is reporting that a company web server is receiving an abnormally high number of web requests from different locations simultaneously. Which type of security attack is occurring?

- ~~Adware~~
- DDoS
- ~~Phishing~~
- ~~Social engineering~~
- ~~Spyware~~

What causes a buffer overflow?

- ~~Launch a security countermeasure to mitigate a Trojan horse~~
- ~~Downloading and installing too many software updates at one time~~
- Attempting to write more data to a memory location than that location can hold
- ~~Sending too much information to two or more interfaces of the same device, thereby causing dropping packets~~
- ~~Sending repeated connections such as Telnet to a particular device, thus denying other data sources~~

Which objective of secure communications is achieved by encrypting data?

- ~~Authentication~~
- ~~Availability~~
- Confidentiality
- ~~Integrity~~

What type of malware has the primary objective of spreading across the network?

- Worm
- ~~Virus~~
- ~~Trojan horse~~
- ~~Botnet~~

What three items are components of the CIA triad? (Choose three.)

- ~~Access~~
- Integrity
- ~~Scalability~~
- Availability
- Confidentiality
- ~~Intervention~~

Which cyber attack involves coordinated attack from a botnet of zombie computers?

- DDoS
- ~~MiTM~~
- ~~ICMP redirect~~
- ~~Address spoofing~~

What specialized network device is responsible for enforcing access control policies between networks?

- ~~Switch~~
- ~~IDS~~
- ~~Bridge~~
- Firewall

To which category of security attack does man-in-the-middle belong?

- ~~DoS~~
- Access
- ~~Reconnaissance~~
- ~~Social engineering~~

What is the role of an IPS?

- To detect patterns of malicious traffic by the use of signature files
- ~~To enforce access control policies based on packet content~~
- ~~To filter traffic based on defined rules and connection context~~
- ~~To filter traffic based on Layer 7 information~~

Which type of DNS attack involves the cybercriminal compromising a parent domain and creating multiple subdomains to be used during the attacks?

- ~~Cache poisoning~~
- ~~Amplification and reflection~~
- ~~Tunneling~~
- Shadowing

Which two types of hackers are typically classified as grey hat hackers? (Choose two.)

- ~~State-sponsored hackers~~
- Hacktivists
- ~~Script kiddies~~
- ~~Cyber criminals~~
- Vulnerability brokers

What is a significant characteristic of virus malware?

- A virus is triggered by an event on the host system
- ~~Once installed on a host system, a virus will automatically propagate itself to other systems~~
- ~~A virus can execute independently of the host system~~
- ~~Virus malware is only distributed over the Internet~~

A cleaner attempts to enter a computer lab but is denied entry by the receptionist because there is no scheduled cleaning for that day. What type of attack was just prevented?

- ~~Trojan~~
- ~~Shoulder surfing~~
- ~~War driving~~
- Social engineering
- ~~Phishing~~

## Module 4: ACL Concepts

### 4.1 Purpose of ACLs

### 4.2 Wildcard Masks in ACLs

### 4.3 Guidelines for ACL Creation

### 4.4 Types of IPv4 ACLs

### 4.5 Module Quiz

## Module 5: ACLs for IPv4 Configuration

## Module 6: NAT for IPv4

## Module 7: WAN Concepts

### 7.1 Purpose of WANs

Which two options describe a WAN (Choose two.)

- ~~A WAN is owned and managed by an organization or home user~~
- A WAN provides networking services over large geographical
- WAN services are provided for a fee
- ~~WANs providers offer low bandwidth speeds over short-distances~~
- ~~WANs guarantee security between the endpoints~~

Which topology type describes the virtual connection between source to destination?

- ~~Cabling topology~~
- ~~Physical topology~~
- Logical topology
- ~~Wired topology~~

Which type of WAN network design is the most fault-tolerant?

- ~~Dual-homed topology~~
- Fully meshed topology
- ~~Hub-and-spoke topology~~
- ~~Partially meshed topology~~
- ~~Point-to-point topology~~

Which is a type of WAN carrier connection that provides redundancy?

- Dual-carrier WAN connection
- ~~Single-carrier WAN connection~~

### 7.2 WAN Operations

Which two statements about the WAN OSI Layer 1 are true? (Choose two.)

- ~~It describes how data will be encapsulated into a frame~~
- It describes the electrical, mechanical, and operational components needed to transmit bits
- ~~It includes protocols such as PPP, HDLC, and Ethernet~~
- It includes protocols such as SDH, SONET, and DWDM

Which WAN term defines the point where the subscriber connects to the service providers network

- ~~Customer Premises Equipment (CPE)~~
- ~~Data Communications Equipment (DCE)~~
- ~~Demarcation point~~
- ~~Local Loop~~
- Point-of-Presence (POP)

Which two devices operate in a similar manner to the voiceband modem but use higher broadband frequencies and transmission speeds? (Choose two.)

- Cable Modem
- ~~CSU/DSU~~
- DSL Modem
- ~~Optical Converter~~
- ~~Voiceband Modem~~

Which communication method is used in all WAN connections?

- ~~Circuit-Switched~~
- ~~Packet-Switched~~
- ~~Parallel~~
- Serial

Which two WAN connectivity options are circuit-switched technologies? (Choose two.)

- ~~ATM~~
- ~~Ethernet WAN~~
- ~~Frame Relay~~
- ISDN
- PSTN

Which two WAN connectivity options are packet-switched technologies? (Choose two.)

- Ethernet WAN
- Frame Relay
- ~~ISDN~~
- ~~PSTN~~

Which service provider fiber-optic technology increases the data-carrying capacity using different wavelengths?

- DWDM
- ~~SDH~~
- ~~SONET~~

### 7.3 Traditional WAN Connectivity

Which traditional WAN connectivity option uses T-Carrier or E-carrier lines?

- ~~ATM~~
- ~~Frame Relay~~
- ~~ISDN~~
- Leased lines
- ~~PSTN~~

Which two traditional WAN connectivity options are circuit-switched? (Choose two.)

- ~~ATM~~
- ~~Frame Relay~~
- ISDN
- ~~Leased Lines~~
- PSTN

Which two traditional WAN connectivity options are packet-switched? (Choose two.)

- ATM
- Frame Relay
- ~~ISDN~~
- ~~Leased Lines~~
- ~~PSTN~~

### 7.4 Modern WAN Connectivity

Which WAN connectivity option is based on Ethernet LAN technology?

- ~~ATM~~
- ~~Cable~~
- ~~DSL~~
- Metro Ethernet
- ~~MPLS~~

Which is a service provider WAN solution that uses labels to direct the flow of packets through the provider network?

- ~~ATM~~
- ~~Cable~~
- ~~DSL~~
- ~~Metro Ethernet~~
- MPLS

### 7.6 Module Quiz

A company is expanding its business to other countries. All branch offices must remain connected to corporate headquarters at all times. Which network technology is required to support this scenario?

- ~~LAN~~
- ~~MAN~~
- WAN
- ~~WLAN~~

What is the recommended technology to use over a public WAN infrastructure when a branch office is connected to the corporate site?

- ~~ATM~~
- ~~ISDN~~
- ~~Municipal Wi-Fi~~
- VPN

Which medium do service providers use to transmit data over WAN technologies with SONET, SDH, DWDM?

- Fiber optic
- ~~Satellite~~
- ~~Wi-Fi~~
- ~~Copper~~

Which statement describes a characteristic of a WAN?

- ~~A WAN operates within the same geographic scope of a LAN, but has serial links~~
- WAN networks are owned by service providers
- ~~All serial links are considered WAN connections~~
- ~~A WAN provides end-user network connectivity to the campus backbone~~

Which type of network would be used by a company to connect locations across the country?

- WAN
- ~~LAN~~
- ~~WLAN~~
- ~~SAN~~

A small company with 10 employees uses a single LAN to share information between computers. Which type of connection to the Internet would be appropriate for this company?

- ~~A dialup connection that is supplied by their local telephone service provider~~
- ~~Virtual Private Networks that would enable the company to connect easily and securely with employees~~
- ~~Private dedicated lines through their local service provider~~
- A broadband service, such as DSL, through their local service provider

Which two layers of the OSI model do WAN technologies provide services? (Choose two.)

- ~~Network layer~~
- ~~Session layer~~
- Physical layer
- ~~Transport layer~~
- Data link layer
- ~~Presentation layer~~

A customer need a metropolitan area WAN connection that provides high-speed, dedicated bandwidth between two sites. Which type of WAN connection would best fulfill this need?

- ~~Circuit-switched network~~
- Ethernet WAN
- ~~MPLS~~
- ~~Packet-switched network~~

An intercity bus company wants to offer constant Internet connectivity to the users travelling on the buses. Which two types of WAN infrastructure would meet the requirements? (Choose two.)

- ~~Private infrastructure~~
- Public infrastructure
- ~~Dedicated~~
- ~~Circuit-switched~~
- Cellular

An enterprise has four branches. The headquarters needs full connectivity to all branches. The branches do not need to be connected directly to each other. Which WAN topology is most suitable?

- ~~Bus~~
- ~~Full mesh~~
- Hub and spoke
- ~~Mesh~~
- ~~Point-to-point~~

What is a characteristic of a WAN?

- ~~A WAN is typically owned by an enterprise which want to interconnect its LANs~~
- WAN service providers include carriers such as a telephone network or satellite service
- ~~WANs always use physical cables to connect LANs~~
- ~~A WAN operates inside the geographic scope of a LAN~~

What are two common types of circuit-switched WAN technologies? (Choose two.)

- ~~Frame Relay~~
- ~~DSL~~
- PSTN
- ~~ATM~~
- ISDN

A new corporation needs a data network that must meet certain requirements. The network must provide a low const connection to sales people dispersed over a large geographical area. Which two types of WAN infrastructure would meet the requirements? (Choose two.)

- Public infrastructure
- ~~Private infrastructure~~
- Internet
- ~~Dedicated~~
- ~~Satellite~~

## Module 8: VPN and IPsec Concepts

## Module 9: QoS Concepts

### 9.1 Network Transmission Quality

What is the variable amount of time it takes for a frame to traverse the links between the source and destination?

- ~~Serialization delay~~
- Propagation delay
- ~~Code delay~~

What happens when congestion occurs?

- Packet loss
- ~~Jitter~~
- ~~Code delay~~

What is the fixed amount of time it takes to transmit a frame from the NIC to the wire?

- Serialization delay
- ~~Jitter~~
- ~~Code delay~~

What is caused by variation in delay?

- ~~Congestion~~
- ~~Packet loss~~
- Jitter

### 9.2 Traffic Characteristics

Which type of traffic tends to consume a large portion of network capacity?

- ~~Voice~~
- ~~Video~~
- Data

Which type of traffic requires at least 384 Kbs of bandwidth?

- ~~Voice~~
- Video
- ~~Data~~

Which type of traffic in unpredictable, inconsistent, and burst?

- ~~Voice~~
- Video
- ~~Data~~

Which type of traffic can be predictable and smooth?

- Voice
- ~~Video~~
- ~~Data~~

Which type of traffic cannot be retransmitted if lost?

- Voice
- ~~Video~~
- ~~Data~~

Which type of traffic must receive a higher UDP priority?

- Voice
- ~~Video~~
- ~~Data~~

### 9.3 Queuing algorithms

Which queuing algorithm simultaneously schedules interactive traffic to the front of a queue to reduce response time?

- ~~FIFO~~
- WFQ
- ~~CBWWFQ~~
- ~~LLQ~~

Which queuing algorithm provides support for user-defined traffic classes? 

- ~~FIFO~~
- ~~WFQ~~
- CBWFQ
- ~~LLQ~~

Which queuing algorithm is effective for large links that have little delay and minimal congestion? 

- FIFO
- ~~WFQ~~
- ~~CBWFQ~~
- ~~LLQ~~

Which queuing algorithm classifies traffic into different flows based on packet header addressing? 

- ~~FIFO~~
- WFQ
- ~~CBWFQ~~
- ~~LLQ~~

Which queuing algorithm allows delay-sensitive data such as voice to be sent before packets in other queues? 

- ~~FIFO~~
- ~~WFQ~~
- ~~CBWFQ~~
- LLQ

Which queuing algorithm applies to priority, or weights, to identify traffic and classify it? 

- ~~FIFO~~
- WFQ
- ~~CBWFQ~~
- ~~LLQ~~

### 9.4 QoS Models


Which QoS model provides per-request policy admission control? 

- ~~Best effort~~
- Integrated services
- ~~Differential services~~

Which QoS model requires no special QoS mechanisms? 

- Best effort
- ~~Integrated services~~
- ~~Differential services~~

Which QoS model provides many different levels of quality? 

- ~~Best effort~~
- ~~Integrated services~~
- Differential services

Which QoS model uses explicit end-to-end resource admission control? 

- ~~Best effort~~
- Integrated services
- ~~Differential services~~

Which QoS model is the most scalable? 

- ~~Best effort~~
- ~~Integrated services~~
- Differential services

### 9.5 QoS Implementation Techniques

Which detects when traffic rates reach a configured maximum rate and drops excess traffic? 

- Traffic policing 
- ~~Traffic shaping~~
- ~~Classification~~

Which determines what class of traffic packets or frames belong to? 

- ~~Marking~~ 
- Classification
- ~~Traffic shaping~~ 

Which adds a value to the packet header? 

- Marking
- ~~Classification~~
- ~~802.1Q~~

Which provides buffer management and allows TCP traffic to throttle back before buffers are exhausted? 

- ~~Traffic policing~~
- ~~802.1Q~~
- WRED 

Which retains excess packets in a queue and then schedules the excess for later transmission over increments of time?

- ~~WRED~~
- ~~Traffic policing~~
- Traffic shaping

### 9.6 Module Quiz

What is the term used to indicate a variation of delay? 

- ~~Latency~~
- ~~Serialization delay~~
- ~~Speed mismatch~~
- Jitter

A network engineer performs a ping test and receives a value that shows the time if takes for a packet to travel from a source to a destination device and return. Which term describes the value? 

- ~~Jitter~~ 
- Latency
- ~~Priority~~
- ~~Bandwidth~~

What role do network devices play in the IntServ QoS model?

- Network devices ensure that resources are available before traffic is allowed to be sent by a host through the network
- ~~Network devices provide a best-effort approach to forward traffic~~
- ~~Network devices are configured to service multiple classes of traffic and handle traffic as it may arrive~~
- ~~Network devices use QoS on a hop-by-hop basis to provide excellent scalability~~

Which device would be classified as a trusted endpoint? 

- ~~Switch~~ 
- ~~Router~~
- ~~Firewall~~
- IP phone 

Under which condition does congestion occur on a converged network with voice, video, and data traffic? 

- If the request for bandwidth exceeds the amount of bandwidth available
- ~~If video traffic requests more bandwidth than voice traffic requests~~
- ~~If voice traffic latency begins to decrease across the network~~
- ~~If a user downloads a file that exceeds the file limitation that is set on the server~~ 

Which type of traffic does Cisco recommend be placed in the strict priority queue (PQ) when low latency queuing (LLQ) is being used? 

- ~~Video~~
- ~~Data~~
- Voice
- ~~Management~~ 

Which model is the only QoS model with no mechanism to classify packets? 

- ~~IntServ~~
- ~~DiffServ~~
- Best-effort
- ~~Hard QoS~~

What happens when the memory queue of a device fills up and new network traffic is received? 

- ~~The network device sends the received traffic immediately~~ 
- The network device will drop the arriving packets
- ~~The network device drops all traffic in the queue~~
- ~~The network device queues the received traffic while sending previously received traffic~~

What are two characteristics of voice traffic? (Choose two.)

- It is delay sensitive
- ~~It is bursty~~
- ~~It can tolerate latency up to 400 ms~~
- It consumes few network resources
- ~~It is insensitive to packet loss~~ 

Which QoS model is very resource intensive and provides the highest guarantee of QoS? 

- ~~DiffServ~~
- ~~Best-effort~~
- IntServ
- ~~Soft QoS~~

What happens when an edge router using IntServ QoS determines that the data pathway cannot support the level of QoS requested? 

- ~~Data is forwarded along the pathway using a best-effort approach~~
- ~~Data is forwarded along the pathway using DiffServ~~
- Data is not forwarded along the pathway
- ~~Data is forwarded along the pathway using IntServ but not provided preferential treatment~~ 

In QoS models, which type of traffic is commonly provided the most preferential treatment over all other application traffic? 

- Voice traffic
- ~~Email~~
- ~~Web traffic~~
- ~~File transfers~~ 

Which queuing mechanism supports user-defined traffic classes? 

- ~~FIFO~~
- CBWFQ
- ~~WFQ~~
- ~~FCFS~~

What mechanism compensates for jitter in an audio stream by buffering packets and then replaying them outbound in a steady stream? 

- ~~Digital signal processor~~
- Playout delay buffer
- ~~Voice codec~~
- ~~WFQ~~

## Module 10: Network Management

### 10.2 Compare CPD and LLDP

Which protocol is used to gather information about Cisco devices which share the same data link? 

- CDP 
- ~~LLDP~~

Which protocol works with network devices, such as routers, switches, and wireless LAN access points across multiple manufacturers' devices? 

- ~~CDP~~ 
- LLDP

### 10.4 SNMP Versions 

Which SNMP version authenticates the source of management messages?

- ~~Version 2~~
- Version 3
- ~~Both~~

Which SNMP version provides services for security models? 

- ~~Version 2~~
- ~~Version 3~~
- Both

Which SNMP version does not provide encrypted management messages? 

- Version 2
- ~~Version 3~~
- ~~Both~~

Which SNMP version is supported by Cisco IOS software? 

- ~~Version 2~~
- ~~Version 3~~
- Both

Which SNMP version includes expanded error codes with types? 

- Version 2
- ~~Version 3~~
- ~~Both~~

Which SNMP version uses community-based forms of security? 

- Version 2
- ~~Version 3~~
- ~~Both~~

Which SNMP version is used for interoperability and includes messages integrity reporting? 

- ~~Version 2~~
- Version 3
- ~~Both~~


## Module 11: Network Design

## Module 12: Network Troubleshooting

## Module 13: Network Virtualization

## Module 14: Network Automation
