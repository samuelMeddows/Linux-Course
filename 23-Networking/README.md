# Networking - Arrange & Manage Limux Communications Channels

The `ip` command:

- A veritile and powerfull command-line tool for managing and diagnosing network configurations on linux.
- Replaces the older ifconfig, route and netstat commands.
- Commonly used to inspect and modify IP addresses, routes and network interfaces.

Show network config: `ip addr show` or `ip a`
Can also use (older): `ifconfig -a`

# Networking Monitoring - Wireshark

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

- Here we send packets which can't be routed
- We no longer work with MAC addresses but with IP addresses
- Packets are wraped itnto a frames which can be sent to a router
  Show ip address: `ip a`
  Show Routes: `ip r`

  ***

### How Subnets Enhance Network Efficiecy

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

---

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
