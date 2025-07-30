# Networking - Arrange & Manage Limux Communications Channels

The `ip` command:

- A veritile and powerfull command-line tool for managing and diagnosing network configurations on linux.
- Replaces the older ifconfig, route and netstat commands.
- Commonly used to inspect and modify IP addresses, routes and network interfaces.

Show network config: `ip addr show` or `ip a`
Can also use (older): `ifconfig -a`

### Networking Monitoring - Wireshark

Install Wireshark: `sudo dmf install wireshark -y`
Run Wireshark as Sudo: `sudo wireshark`

---

**OSI Model**

- | DATA |
- | DATA | L4-HEADER|: Segment
- | DATA | L4-HEADER | L3-HEADER |: Packet
- | L2-TRAILER | DATA | L4-HEADER | L3-HEADER | L2-HEADER |: Frame

---

### Pysical Layer - Layer 1 SIGNAL (PLEASE)

The Physical connection through which bits can be sent.

- Physical Medium
  - Copper, firber-optic, Wireless
- Data Transmission
  - Convert digital data to signals
- Signaling
  - Voltage levels, modulation, syncrononization
- Error detection
  - eg: parity bits

Get network devies: `ip a`
Enable a nic: `sudo ip link set <interface> up`
Disable a nic: `sudo ip link set <interface> down`

---

### Data Link Layer (frame) - Layer 2 MAC (DO)

A reliable communication between two devies on the same network

- Units we sent through it are called frames
- An ARP request is sent to find the destination MAC address
- A frame is ususaoly 1005 bites in size
- Devidec into two sub layers:
  - Logical Link Control (LLC) Flow and error detection
  - Media Access Control (MAC) Inique hardware addresses
- Fram encpesulation:
  - Organises into data frames for transmission
  - Add source and destination MAC address
- MAC protocols
  - Ethernet (IEEE 802l.3)
  - WiFi (IEEE 802.11)

**Ehternet**

- Ethernet frame: max 1.5kb - JUMBO frames allow larger frame sizes
- An Ethernet frame contains actually data to transfer
- An Ethernet frame also contains a checksum for verification by the recepiant
- An Ethernet frame contains adderessing information
  - MAC of the source
  - MAC of the destination

**WiFi**

- Contains the same data as an ethnet frame with additonal data for WiFi transmission
- A WAP can convert ethernet into WiFi frames
  Show MAC address: `ip a`

**Hardware**

- Switch
- Bridge
- Wireless Access Point

---

### Network Layer (packet) - Layer 3 IP (NOT)

---

- Here we send packets which can't be routed
- We no longer work with MAC addresses but with IP addresses
- Packets are wraped itnto a frames which can be sent to a router
  Show ip address: `ip a`
  Show Routes: `ip r`

  ***

### How Subnets Enhance Network Efficiecy

---

A subnet is a network inside a network
This allows us to manage more computers and make large networks more efficient.

**Example:**
Address:
IP: 192.168.1.2
Mask: 255.255.255.0

Both will be replaceed as binary:
IP: 11000000.10101000.00000001.00000010
Mask: 1111111.11111111.11111111.00000000

We want to send the packaet to another computer on the LAN
Destination IP: 192.168.1.3(11000000.10101000.00000001.00000011)

Can we send this packet direclty? To check we aplly a logical AND to our IP and the destination.
Local IP:
Applied to source IP: 1111111.11111111.11111111.00000010
Applied to destination IP: 1111111.11111111.11111111.00000011
This can be switched to the computer on the same nework.

External IP Google:
Applied to source IP: 1111111.11111111.11111111.00000010
Applied to 8.8.8.8: 00001000.00001000.00001000.00000000
This needs to be sent to the router

**Shorter Subnet Mask Notation**
255.255.255.0: 1111111.11111111.11111111.00000000
192.168.1.3/24: /24 means 24 1's.

**Network Size**
The subnet mask specifies the size of the network.
192.168.1.0/24: 192.168.1.1 - 192.168.1.255
192.168.1.0: This is the network identifier
192.168.1.255: Reserved for broadcast

**How to Inspect the Subnet Mask**
Show subnet mask `ip a`
Output:

```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:30:42:cf brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.125/24 brd 192.168.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 80987sec preferred_lft 80987sec
    inet6 fe80::a00:27ff:fe30:42cf/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:f5:58:22 brd ff:ff:ff:ff:ff:ff

```

### Adress Resolution Protocol - ARP

Communication is done via MAC addresses at layer 2 (ethernet frame).
A computer will send out at ARP request asking which MAC address own's the destination IP.

---

### How to change the IP address

Add an IP to an interface: `sudo ip a add 192.168.1.10/24 dev enp0s3`
Remove an IP to an interface `sudo ip a del 192.168.1.10/24 dev enp0s3`
A device can have multiple IP addresses'
It is better to use DHCP and resever the IP/MAC of the device in the router or DHCP server.

---

### Network Routing - Inspecting Routing Tables & Adding Routes

Show routing table: `ip r`
Output:

```
default via 192.168.1.1 dev enp0s3 proto dhcp src 192.168.1.125 metric 100
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
172.18.0.0/16 dev br-9f11b90782cd proto kernel scope link src 172.18.0.1 linkdown
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.125 metric 100
```

Show a specific route: `ip r get <destination>`
Local route output: `192.168.1.62 dev enp0s3 src 192.168.1.125 uid 1000`
External route output: `8.8.8.8 via 192.168.1.1 dev enp0s3 src 192.168.1.125 uid 1000`

Add a route: `ip r add <destination> via <gateway> dev <interface>`
Remove a route: `ip r del <destination> via <gateway> dev <interface>`

---

### Dynamic Host Control Protocol (DHCP) - Managing IP Adresses on Networks

When a new device connectes to the network, it automatically gets assigned a new IP address by the DHCP.

**DHCP Server**

- Stores IP adress pool
- Manages IP address leases
- Can reserve an IP for a specified MAC address

**DHCH Client**

- Requests IP adress and config
- Renews or releases leases

**DHCP Relay Agent (opptional)**

- Forwards requests between subnets

**Technical Note**

- DHCP helps to manage layer 3, meaning that we can easily obtain the network configuration for a device (IP, Subnet,Ggateway, DNS).
- Technically, DHCP is running on layer 4 (UDP).

**DHCP Process**

1. Discover:
   - Client Brodcasts DHCP discover message to the whole network (255.255.255.255)
   - Other cients will receive but ignore the message.
2. Offer:
   - The DHCPserver will see the request to 255.255.255.255 and responds with an IP offer.
   - The message contains IP address and lease information.
3. Request:
   - Client sends DHCP request message
   - Accepts IP address and lease terms
4. Acknowledge
   - Sever sends DHCP acknowledge message
   - Confirms IP address assignment and lease duration

---

### Inspecting DHCP Logs for IP Issues - systemd-networkd (Debian)

Show logs: `journalctl -u systemd-networkd`

---

### Inspecting DHCP Logs for IP Issues - NetworkManager (RHEL)

Show logs (-b everything after boot): `journalctl -b -u NetworkManager`

---

### Inspection Network Connectivity by IMCP - ping (layer 3)

The protocol is called ICMP (Internet Control Message Protocol)

Ping is used to inspect the connection to another computer.
Ping host: `ping 8.8.8.8`
Ping Domain: `ping google.com`

---

### Exploring Network Routes (latency & routing)- traceroute (layer 3)

Note: Not installed of CentOS by default.

Determins the path taken by data packets from source to destination.

**google.com**
`traceroure google.com`
Output:

- Hope numbers: Indicates the position of the router in the path
- Router IP/Hsotname: Identifies the intermediate router
- RTT: Latency measurments for each packet sent

```
traceroute to google.com (172.217.167.78), 30 hops max, 60 byte packets
 1  unifi.localdomain (192.168.1.1)  3.519 ms  3.449 ms  5.078 ms
 2  loop1591961880.bng.qld.aussiebb.net (159.196.188.1)  9.851 ms  9.825 ms  9.710 ms
 3  10.241.1.48 (10.241.1.48)  122.287 ms  122.259 ms  122.233 ms
 4  10.241.1.184 (10.241.1.184)  125.408 ms  125.529 ms 10.241.1.186 (10.241.1.186)  125.347 ms
 5  be2.lsr2.8gif.nsw.aussiebb.net (180.150.0.160)  125.320 ms  125.428 ms  125.398 ms
 6  10.241.14.241 (10.241.14.241)  123.477 ms  119.211 ms  119.698 ms
 7  10.241.15.29 (10.241.15.29)  119.607 ms  121.491 ms syd15s06-in-f14.1e100.net (172.217.167.78)  116.362 ms
```

Note: Routes can take different paths depending on the status of the trversed networks.

**helsinki.fi**
Set max hops to 50: `traceroute -m 50 helsinki.fi `
output:

```
raceroute to helsinki.fi (128.214.222.50), 50 hops max, 60 byte packets
 1  unifi.localdomain (192.168.1.1)  3.581 ms  3.661 ms  4.091 ms
 2  loop1591961880.bng.qld.aussiebb.net (159.196.188.1)  17.352 ms  17.235 ms  17.119 ms
 3  10.241.1.48 (10.241.1.48)  111.120 ms 10.241.1.50 (10.241.1.50)  110.760 ms  110.640 ms
 4  10.241.1.186 (10.241.1.186)  115.200 ms 10.241.1.184 (10.241.1.184)  114.955 ms 10.241.1.186 (10.241.1.186)  114.833 ms
 5  be2.lsr2.8gif.nsw.aussiebb.net (180.150.0.160)  111.798 ms  111.685 ms  114.300 ms
 6  be20.lsr2.200bou.nsw.aussiebb.net (159.196.252.106)  114.189 ms  111.719 ms  110.782 ms
 7  10.241.16.231 (10.241.16.231)  110.662 ms  112.577 ms  112.429 ms
 8  be21.ier1.26aye.sin.aussiebb.net (180.150.2.63)  109.035 ms  111.554 ms  111.390 ms
 9  * kbn-bb5-link.ip.twelve99.net (62.115.143.33)  275.433 ms  275.293 ms
10  kbn-b4-link.ip.twelve99.net (62.115.136.231)  277.421 ms kbn-b4-link.ip.twelve99.net (62.115.134.81)  277.165 ms kbn-b4-link.ip.twelve99.net (62.115.136.231)  277.482 ms
11  nordunet-ic-358161.ip.twelve99-cust.net (62.115.11.78)  279.814 ms  279.478 ms  279.206 ms
12  se-sthb1.nordu.net (109.105.97.131)  280.976 ms  280.863 ms  280.522 ms
13  ndn-gw.funet.fi (109.105.102.1)  280.362 ms  280.455 ms  281.771 ms
14  turku4-et-0-0-8-1.ip.funet.fi (86.50.254.250)  293.022 ms  294.303 ms  294.125 ms
15  espoo4-et-0-0-0-1.ip.funet.fi (86.50.254.254)  292.324 ms  292.339 ms  292.046 ms
16  helsinki5-et-0-0-0-1.ip.funet.fi (86.50.254.253)  296.853 ms  296.709 ms  296.486 ms
17  helsinki1.ip.funet.fi (86.50.255.104)  291.240 ms  291.099 ms  294.929 ms
18  r1.helsinki.kampus.funet.fi (193.167.244.245)  294.995 ms  294.648 ms  291.115 ms
19  kumpula1-r1.fe.helsinki.fi (128.214.173.242)  297.206 ms  298.093 ms  297.945 ms
20  * * *
{removed 21 * * * to 49 * * *}
50  * * *
```

---

### Mapping Internet Packet Paths and TTL - traceroute (layer 3)

We send out an IP packet with a TTL of 1 to the destination.
Our router will decrease the TTL bt 1. It's now 0.
Because it is now 0, it will discard the packet and reply with a ICMP "Time Exceeded" packet.

Set max hops to 1: `traceroute -m 1 helsinki.fi `

With this technique wil can slowy trace the path to the destination.

---

### Transport Layer (Layer 4 OSI)

Often packets just get lost:

- A router may be overloaded and just drop packets
- UDP is a great solution for applications like VOIP and video streaming.
- TCP ensures all packets are recived by requester if packets that don't arrive.

**TCP: Transmission Contrrol Protocol**

- A connection oriented protocol
- Provides reliable, ordered and error-checked data transmission
- Operates are the transport layer (layer 4)
- It also ensure we utilze the bandwidth that the reciver can handel / our conntion can hendel.
  - Flow Control: what the reciver can handel
  - Congestion control: what the connection can handle

---

### TCP Ports

**Ports in TCP**

- 16 bit numbers assgined to specific processes and sevices
- Rannge: 0-65535
- Differentiate between multiple connections on a single device

**Types of TCP Ports**

- Well know ports: 0-1023
- Reserved for standard services and protocols
- Examples: HTTP(80), HTTPS(443), FTP(21), SSH(22)

**Registed Ports**

- Range: 1024-49151
- Assigned to specific applications by IANA
- Exampls: MySQL(3306), PostgresSQL(5432), VNC(5900)

**Dynamic Ports**

- Range: 49152-65535
- No controlled by IANA
- Avaible for any application to use

Note: We can change the default port of most standard or prot specified application.

**Port Comminication**
Source Port

- Randomly generated from dynamic/private port range
- Identifies the sneding application on the client

Destination Port

- Identifies the reciving application on the server
- Typically, this is a well-known or registered port number

Port Combination

- Unique combination of source IP, source port, destination IP and destination port
- Differentiates multiple connections between the same devices

---

### Esstntial TCP & UDP Ports

**Common TCP ports**

- HTTP (80): Web
- HTTPS (443): Secure Web
- FTP (20, 21): File Transfer
- SSH (22): Secure Shell
- Telnet (23): Remote Shell (unencrypted)
- SMTP (25): Simpple Mail
- IMAP (143): Internet Message Access Protocol
- POP3 (110): Post Office Protocol V3

**Commonm UDP ports**

- DNS (53): Domain Name System
- DHCP (67, 68): Dynamic Host Config Protocol
- SNMP (161, 162): Simple Network Management Protocol
- TFTP (69): Trivial File Transfer Protocol
- NTP (123): Network Time Protocol
- RTP (5004, 5005): Real-Time Transport Protocol (audio/video streaming)

---

### The TCP Handshake Process (layer 4)

**The Goal**

- Both computers need to know that the other side responds
- Both will later need to know how much data has already been recived by the other side

**The 3 Way Handshake**

- Thee source computer sends a SYN packet to the destination computer
- The destination computer sends a SYN-ACK pack to the source computer
- The source computer send a ACK packet - The connection is established now

```
Computer (Client)                         Server
      |                                      |
      | ------ SYN:1,ACK:0,ISN:1234 -------> |
      |                                      |
      | <----- SYN:1,ACK:1,ISN:100000 ------ |
      |        ACK Number:1234               |
      |                                      |
      | ------ SYN:0,ACK:1,Sequence:12345 -> |
      |        ACK Number: 10000             |
      |                                      |
```

Connection Established ✔

---

### Port Scanning - nmap

**Port Scanning**

- Try to connect through all possible ports - Sending SYN packets
- If a port is open, we will get a message back from the server

**Nmap**

- Network discovery and security auditing
- Can single ports or an entire network
- By default it will scan the most common 1000 ports

**Namp Commands**
Default scan: `nmap <hostname.IP>`
Specify port(s): `nmap -p <port> <hostname.IP>` or `nmap -p <port> <port> <hostname.IP>`
Scan all ports: `nmap -p <hostname.IP>`
Scan a whole network: `nmap -p 192.168.1-100`

**Net Cat**
Net cat can also be used to check a port: `nc -zv 192.168.1.10 80`

**Telnet**
Telnet can also be used to check a port: `telnet 192.168.1.10 80`

---

### Network Address Translation - NAT

Translate private IP network traffic to public IP traffice and in reverse.
The local router will remeber the translation mapping when the public reply returns to the LAN.

An ISP might also combine multiple customer networks to a single public IP.

Port forwarding allows an outside source to reach a destination computer in the LAN.

Example: Forward port 80 to 192.168.1.200 on port 8080

### OSI Layer 5/7 - The Session Layer

Interhost comminication.
Note: Handeled by layer 7

HTTP3 uses UDP to communicaite between servers.

**Session Layer**

- Establishes, maintains and terminates connections
- Supports coms between applications

**Exmaples**

- Network File System (NFS)
- Remote Proceedure Call (RPC)
- Session Control Protocol (SCP)

---

### OSI Layer 6/7 - The Presentation Layer

Data representation and Encryption.
Note: Handeled by layer 7

** Presentation Layer**

- Deals with data representation
- Ensure data compatability and security

**Function**

- Data Conversion
- Transforms data formats: (ASCII, EBCDIC, Unicode)
- Encryption and decryption: (SSL/TLS)
- Data compression
- Reduce data size (deflat, brotil compression)

**Examples**

- SSL/TLS: (Secure Socket Layer/Transport Layer Security)
  - Provides secure comms over the network
  - Technically it is operating at level 6 and 7
- MIME: (Multipurpose Internet Mail Extensions)
  - Defines how E-Mails support attachments, character encoinds and other feautres.

---

### OSI Layer 6/7 - The Application Layer

Network process to application.
The application layer are the protocols that the application can use

**Example**

- HTTP/HTTPS: Websies
- IMAP: Accessing emails on a remote server
- SSH/SCP: Accessing a remote shell/secure file copy
- POP3: Downloading emails
- Proprietary protocols: Custom VOIP implimentations

---

### Domain Name Systems - DNS Protocol

Application layer.
Translaye IP to domain names.

**Process**

1. Broswer local cache
2. Browser will ask the OS which also has a local cache
3. The OS will check the local host file
4. The registed DNS resolver will be contacted
5. The DNS resolver will have a cache
6. The DNS resolver will contact one of the 13 root name servers at random
   - They are labeled A to M: i.root-servers.net
   - The rquest is to get TLD.com (Top Level Domain)
   - The root name server does not know google.com, but it knows which server does know about .com domains.
7. The root name server will give a name server that has the TLD.com
   - e.gtld-servers.net at 192.12.94.30
8. e.gtld-servers.net is contacted for the name servers for google.com
   - It does not have it but gives the server: ns1.google.com / 216.239.32.10
9. Connect to ns1.google.com who has the information and the DNS is resolved to the google.com IP.
10. DNS resolver > OS > web browser

Note:

- Linux Domain servers are configured in /etc/resolv.conf.
- Local (static) domain mapping is done in /etc/hosts.

### DNS Records & the host command

**Common DNS Records**
A: Maps a domain to an IPv4 address
AAAA: Maps a domain to an IPv6 address
CNAME: Provides an alias for another domain name
MX: Specifies mail servers for a domain
NS: Lists authoritative name severs for a domain

List all the recived DNS entires for a given domain: `host -a <domain>`

Note: host -a tries to perform a zone transfer (AXFR), which most public DNS servers (like Google's 8.8.8.8) block for security reasons.

### Manually Resolve DNS - dig

This is a learning experience and not how DNS should be resolved in general.
Quert a root name server: `dig @a.root-servers.net com NS`

```
; <<>> DiG 9.16.23-RH <<>> dig @a.root-servers.net com NS
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 54169
;; flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;dig.				IN	A

;; AUTHORITY SECTION:
.			86400	IN	SOA	a.root-servers.net. nstld.verisign-grs.com. 2025072902 1800 900 604800 86400

;; Query time: 115 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Wed Jul 30 13:44:41 AEST 2025
;; MSG SIZE  rcvd: 107

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 18898
;; flags: qr rd ra; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;com.				IN	NS

;; ANSWER SECTION:
com.			162141	IN	NS	a.gtld-servers.net.
com.			162141	IN	NS	b.gtld-servers.net.
com.			162141	IN	NS	c.gtld-servers.net.
com.			162141	IN	NS	d.gtld-servers.net.
com.			162141	IN	NS	e.gtld-servers.net.
com.			162141	IN	NS	f.gtld-servers.net.
com.			162141	IN	NS	g.gtld-servers.net.
com.			162141	IN	NS	h.gtld-servers.net.
com.			162141	IN	NS	i.gtld-servers.net.
com.			162141	IN	NS	j.gtld-servers.net.
com.			162141	IN	NS	k.gtld-servers.net.
com.			162141	IN	NS	l.gtld-servers.net.
com.			162141	IN	NS	m.gtld-servers.net.

;; Query time: 6 msec
;; SERVER: 192.168.1.1#53(192.168.1.1)
;; WHEN: Wed Jul 30 13:44:41 AEST 2025
;; MSG SIZE  rcvd: 256
```

Quert a name server `dig @b.gtld-servers.net google.com NS`

```
; <<>> DiG 9.16.23-RH <<>> @b.gtld-servers.net google.com NS
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 61862
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 4, ADDITIONAL: 9
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;google.com.			IN	NS

;; AUTHORITY SECTION:
google.com.		172800	IN	NS	ns2.google.com.
google.com.		172800	IN	NS	ns1.google.com.
google.com.		172800	IN	NS	ns3.google.com.
google.com.		172800	IN	NS	ns4.google.com.

;; ADDITIONAL SECTION:
ns2.google.com.		172800	IN	AAAA	2001:4860:4802:34::a
ns2.google.com.		172800	IN	A	216.239.34.10
ns1.google.com.		172800	IN	AAAA	2001:4860:4802:32::a
ns1.google.com.		172800	IN	A	216.239.32.10
ns3.google.com.		172800	IN	AAAA	2001:4860:4802:36::a
ns3.google.com.		172800	IN	A	216.239.36.10
ns4.google.com.		172800	IN	AAAA	2001:4860:4802:38::a
ns4.google.com.		172800	IN	A	216.239.38.10

;; Query time: 211 msec
;; SERVER: 192.33.14.30#53(192.33.14.30)
;; WHEN: Wed Jul 30 13:48:29 AEST 2025
;; MSG SIZE  rcvd: 287
```

Query the google name server: `dig @ns2.google.com google.com NS`

```
; <<>> DiG 9.16.23-RH <<>> @ns2.google.com google.com any
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46789
;; flags: qr aa rd; QUERY: 1, ANSWER: 22, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;google.com.			IN	ANY

;; ANSWER SECTION:
google.com.		300	IN	A	172.217.167.78
google.com.		300	IN	AAAA	2404:6800:4006:80a::200e
google.com.		3600	IN	TXT	"v=spf1 include:_spf.google.com ~all"
google.com.		3600	IN	TXT	"apple-domain-verification=30afIBcvSuDV2PLX"
google.com.		86400	IN	CAA	0 issue "pki.goog"
google.com.		300	IN	MX	10 smtp.google.com.
google.com.		3600	IN	TXT	"cisco-ci-domain-verification=47c38bc8c4b74b7233e9053220c1bbe76bcc1cd33c7acf7acd36cd6a5332004b"
google.com.		3600	IN	TXT	"onetrust-domain-verification=de01ed21f2fa4d8781cbc3ffb89cf4ef"
google.com.		3600	IN	TXT	"MS=E4A68B9AB2BB9670BCE15412F62916164C0B20BB"
google.com.		3600	IN	TXT	"docusign=05958488-4752-4ef2-95eb-aa7ba8a3bd0e"
google.com.		345600	IN	NS	ns1.google.com.
google.com.		3600	IN	TXT	"globalsign-smime-dv=CDYX+XFHUw2wml6/Gb8+59BsH31KzUr6c1l2BPvqKX8="
google.com.		3600	IN	TXT	"docusign=1b0a6754-49b1-4db5-8540-d2c12664b289"
google.com.		3600	IN	TXT	"google-site-verification=TV9-DBe4R80X4v0M4U_bd_J9cpOJM0nikft0jAgjmsQ"
google.com.		60	IN	SOA	ns1.google.com. dns-admin.google.com. 788377477 900 900 1800 60
google.com.		345600	IN	NS	ns4.google.com.
google.com.		345600	IN	NS	ns2.google.com.
google.com.		3600	IN	TXT	"facebook-domain-verification=22rm551cu4k0ab0bxsw536tlds4h95"
google.com.		3600	IN	TXT	"google-site-verification=4ibFUgB-wXLQ_S7vsXVomSTVamuOXBiVAzpR5IZ87D0"
google.com.		21600	IN	HTTPS	1 . alpn="h2,h3"
google.com.		3600	IN	TXT	"google-site-verification=wD8N7i1JTNTkezJ49swvWW48f8_9xveREV4oB-0Hf5o"
google.com.		345600	IN	NS	ns3.google.com.

;; Query time: 21 msec
;; SERVER: 216.239.34.10#53(216.239.34.10)
;; WHEN: Wed Jul 30 13:51:33 AEST 2025
;; MSG SIZE  rcvd: 1121
```
