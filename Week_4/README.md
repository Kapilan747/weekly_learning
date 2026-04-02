# Networking Basics

A network is a group of computers/devices connected together to share data and resources.

Example:

Your phone + laptop + WiFi router = a small network

Office computers connected via LAN = a network


## Types of Networks

### 1. LAN (Local Area Network)

Covers a small area (home, office, college)

Fast and reliable

 #### Example:

Your home WiFi network

Office systems connected via switch

### 2. WAN (Wide Area Network)

Covers large geographical areas

Connects multiple LANs

 #### Example:

The Internet is the biggest WAN

Company branches in different cities connected

### 3. MAN (Metropolitan Area Network)

Covers a city or large campus

 #### Example:

Internet service across a city

University campus network

# OSI Model

| Layer | Name         | Purpose           | Real Example     |
| ----- | ------------ | ----------------- | ---------------- |
| 7     | Application  | User interaction  | Browser (Chrome) |
| 6     | Presentation | Encryption/format | HTTPS encryption |
| 5     | Session      | Session control   | Login session    |
| 4     | Transport    | Data delivery     | TCP/UDP          |
| 3     | Network      | Routing (IP)      | Router           |
| 2     | Data Link    | MAC address       | Switch           |
| 1     | Physical     | Cables/signals    | Ethernet cable   |


## Physical Layer

Layer 1 (Physical) specifications define the transmission and reception of RAW BIT STREAMS between a device and a SHARED physical medium. It defines things like voltage levels, timing, rates, distances, modulation and connectors


```

      [ GAME ]                                                   [ GAME ]
   +------------+                                             +------------+
   |   Laptop   |      +-----+                   +-----+      |   Laptop   |
   | (Player 1) |------| NIC |-------------------| NIC |------| (Player 2) |
   +------------+      +-----+                   +-----+      +------------+

          |               |          Binary         |                |
          |               |       Data Stream       |                |
          +---------------+-------------------------+----------------+
                         0 1 0 1 0 1 0 1 0 1 0 1 0


```

## Data Link

Devices at L2 have a unique hardware (MAC) address e.g 3:22:fb:b9:5b:75. 48 bits, in hex, 24 bits for manufacturer.
Frames can be addressed to a destination or broadcast (ALL F's)

```

+----------------+-------------+------------+---------+------------------+------+
|    PREAMBLE    | DESTINATION |   SOURCE   | ETHER-  |                  |      |
|    56 Bits     |     MAC     |    MAC     |  TYPE   |     PAYLOAD      | FCS  |
|      ---       |   ADDRESS   |  ADDRESS   | 16 Bits | 46 - 1500 BYTES  |  32  |
|   SFD 8 Bits   |             |            |         |                  | Bits |
+----------------+-------------+------------+---------+------------------+------+
 <------------------MAC HEADER----------------------->
```

## Network
```
+----------------------------------------------------------------------------------+
|                                   NETWORK                                        |
+----------------------------------------------------------------------------------+
| Purpose : Logical addressing and routing                                         |
|----------------------------------------------------------------------------------|
| Data Unit : Packet                                                               |
|----------------------------------------------------------------------------------|
| Responsibilities:                                                                |
|  - IP Addressing (192.168.1.1)                                                   |
|  - Routing between networks                                                      |
|  - Path determination                                                            |
|----------------------------------------------------------------------------------|
| Devices : Router                                                                 |
+----------------------------------------------------------------------------------+
```

#### Packet Structure:

```
+------------+-------------+------------------+
| IP HEADER  | DATA        |                  |
+------------+-------------+------------------+
```

## Transport

```
+----------------------------------------------------------------------------------+
|                                  TRANSPORT                                       |
+----------------------------------------------------------------------------------+
| Purpose : End-to-end communication                                               |
|----------------------------------------------------------------------------------|
| Data Unit : Segment (TCP) / Datagram (UDP)                                       |
|----------------------------------------------------------------------------------|
| Responsibilities:                                                                |
|  - Reliable delivery (TCP)                                                       |
|  - Fast delivery (UDP)                                                           |
|  - Port numbers (80, 443)                                                        |
|  - Flow control                                                                  |
|----------------------------------------------------------------------------------|
| Protocols : TCP, UDP                                                             |
+----------------------------------------------------------------------------------+
```

#### Segment Structure:
```
+------------+------------+------------------+
| SRC PORT   | DEST PORT  | DATA             |
+------------+------------+------------------+
```

Layer 5 – Session
+----------------------------------------------------------------------------------+
|                                   SESSION                                        |
+----------------------------------------------------------------------------------+
| Purpose : Manage sessions between applications                                   |
|----------------------------------------------------------------------------------|
| Data Unit : Data                                                                 |
|----------------------------------------------------------------------------------|
| Responsibilities:                                                                |
|  - Session establishment                                                         |
|  - Session maintenance                                                           |
|  - Session termination                                                           |
|----------------------------------------------------------------------------------|
| Example : Login session, video call session                                      |
+----------------------------------------------------------------------------------+
Layer 6 – Presentation
+----------------------------------------------------------------------------------+
|                                PRESENTATION                                      |
+----------------------------------------------------------------------------------+
| Purpose : Data translation, encryption, compression                              |
|----------------------------------------------------------------------------------|
| Data Unit : Data                                                                 |
|----------------------------------------------------------------------------------|
| Responsibilities:                                                                |
|  - Encryption (SSL/TLS)                                                          |
|  - Data formatting (JSON, XML)                                                   |
|  - Compression                                                                   |
|----------------------------------------------------------------------------------|
| Example : HTTPS encryption                                                       |
+----------------------------------------------------------------------------------+
Layer 7 – Application
+----------------------------------------------------------------------------------+
|                                 APPLICATION                                      |
+----------------------------------------------------------------------------------+
| Purpose : Interface between user and network                                     |
|----------------------------------------------------------------------------------|
| Data Unit : Data                                                                 |
|----------------------------------------------------------------------------------|
| Responsibilities:                                                                |
|  - User interaction                                                              |
|  - Network services                                                              |
|----------------------------------------------------------------------------------|
| Protocols : HTTP, FTP, SMTP, DNS                                                 |
+----------------------------------------------------------------------------------+

User → Browser → Internet

## Full Flow (Bottom → Top)
```
PHYSICAL  → DATA LINK → NETWORK → TRANSPORT → SESSION → PRESENTATION → APPLICATION
   Bits        Frames      Packets      Segments         Data             User
```



## NOTE
   Not all seven layers do not take part in every single piece of communication.

   OSI Model is a theoritical framework.

   Hubs and Cables (Layer 1): These only deal with raw bits and electrical signals; they have no concept of IP addresses or applications.

   Switches (Layer 2): Standard switches only process data up to the Data Link layer to read MAC addresses.

   Routers (Layer 3): Routers go up to the Network layer to read IP addresses and decide the best path for data. 

## ARP: The "Phonebook" of the Local Network 
ARP is a helper protocol. It does not carry application data like a message or a file. Instead, it translates the IP address (logical) into a MAC address (physical) so your computer knows which physical network card to send bits to. 

How it works: Your computer shouts a "Who has this IP?" broadcast to everyone on the local network. The device with that IP shouts back, "I do, and here is my MAC address".

Relationship: Both TCP and UDP rely on ARP to function in a local network. Without ARP, your computer wouldn't know the physical destination for its packets.


# Types of Networks

```
       _________________________________
      /              WAN                \
     /         (100km - 1000km)          \
    /     _________________________       \
   /     /           MAN           \       \
  /     /          (< 10km)         \       \

 |     |     _________________       |       |
 |     |    /       LAN       \      |       |
 |     |   /    (10m - 1km)    \     |       |
 |     |  |     ___________     |    |       |
 |     |  |    /    PAN    \    |    |       |
 |     |  |   |   (< 10m)   |   |    |       |
 |_____|__|___|_____________|___|____|_______|

```


| Feature     | MAC Address (Media Access Control)                        | IP Address (Internet Protocol)                                |
| ----------- | --------------------------------------------------------- | ------------------------------------------------------------- |
| Common Name | Physical Address                                          | Logical Address                                               |
| OSI Layer   | Layer 2 (Data Link Layer)                                 | Layer 3 (Network Layer)                                       |
| Permanence  | Permanent: "Burned into" the hardware by the manufacturer | Dynamic: Assigned by your network (ISP/Router) and may change |
| Scope       | Used for communication within a local network (LAN)       | Used for communication between different networks (Internet)  |
| Format      | 48-bit Hexadecimal (e.g., 00:1A:2B:3C:4D:5E)              | 32-bit (IPv4) or 128-bit (IPv6)                               |


## Ports

      “Different doors in the same house for different services”
#### Example:

Port 80 → HTTP (web browsing)

Port 22 → SSH (remote login)
<br>
<br>
<br>

# Troubleshooting Tools

## ping & tracert
|Command| Usage |
|--|--|
|`ping google.com` |Tests if a host is reachable using ICMP (Internet Control Message Protocol)|
|`tracert www.netflix.com`||

## ipconfig

`ipconfig`

## netstat

`netstat`

`netstat -an`

## nslookup

`nslookup google.com`

## curl

`curl https://google.com`

## telnet

`telnet google.com 80`

| Layer | Problem Type    | Tool                 |
| ----- | --------------- | -------------------- |
| L7    | App/API issue   | `curl`               |
| L4    | Port/connection | `telnet`, `netstat`  |
| L3    | Routing/IP      | `ping`, `traceroute` |
| L2    | Local network   | `arp`, `ipconfig`    |
| L1    | Physical        | Cable/WiFi           |

<br>
<br>
<br>
<br>

# Types of DNS Records

## 1. A Record
Maps domain → IPv4 address

#### Example:

google.com → 142.250.183.14
## 2. AAAA Record
Maps domain → IPv6 address
## 3. CNAME Record
Maps one domain → another domain

#### Example:

www.google.com → google.com
## 4. MX Record
Used for email servers

#### Example:

Gmail mail routing
## 5. TXT Record
Stores verification/security info

#### Example:

Domain verification (Google, AWS)

## DNS Troubleshooting
`nslookup google.com`

<br>
<br>
<br>
<br>

# HTTP Headers
HTTP headers are key-value pairs sent between:

 -> Client (browser)

 -> Server

### Types of Headers
   1. Request Headers (Client → Server)
   2. Response Headers (Server → Client)

<br>
<br>
<br>
<br>

# Protocols
A protocol is a set of rules that defines how devices communicate over a network.<br>
`Protocol = Language computers use to talk`

<br>
<br>
<br>
<br>
<br>


# Types of Physical Network Topologies
`Bus Topology: `All nodes are connected to a single main cable (the backbone).

`Star Topology:` All nodes are connected to a central hub or switch.

`Ring Topology:` Each node connects to exactly two other nodes, forming a continuous pathway for signals through each node.

`Mesh Topology:` Every node is connected to every other node (Full Mesh) or some other nodes (Partial Mesh).

`Tree Topology:` A hybrid topology in which star networks are fitted onto a bus.

`Hybrid Topology:` A combination of two or more different topologies.

# Internet Protocol (IP)
IP is responsible for addressing and routing packets across networks

Works at Layer 3 (Network Layer)
## Types:
IPv4 → 32-bit (e.g., 192.168.0.1)

IPv6 → 128-bit (modern internet)

### Important:
IP is connectionless (no guarantee of delivery)

# TCP (Transmission Control Protocol)
Works at Layer 4 (Transport Layer)

Provides reliable communication

## Features:
Connection-oriented (3-way handshake)

Guarantees delivery

Error checking & retransmission

Maintains order of data

## 3-Way Handshake:
SYN → Request<br>
SYN-ACK → Acknowledge<br>
ACK → Connection established
## Use Cases:
Web browsing (HTTP/HTTPS)<br>
File transfer (FTP)<br>
Emails (SMTP)<br>

# UDP (User Datagram Protocol)
Also works at Layer 4 (Transport Layer)

Provides fast but unreliable communication

## Features:
No connection setup

No guarantee of delivery

No ordering

Very fast

## Use Cases:
Video streaming<br>
Online gaming<br>
VoIP (calls)



## Subnetting & CIDR

- Subnetting is the process of dividing a network into smaller networks  
- CIDR (Classless Inter-Domain Routing) uses notation like `/24`, `/16`  

### Example:
- 192.168.1.0/24 → 256 IP addresses  

### Why Important:
- Used in cloud networking (AWS, Azure, GCP)  
- Helps in efficient IP management and security  



## Private vs Public IP

- **Private IP** → Used within local networks  
- **Public IP** → Used on the internet  

### Private IP Ranges:
- 10.0.0.0 – 10.255.255.255  
- 172.16.0.0 – 172.31.255.255  
- 192.168.0.0 – 192.168.255.255  

### NAT (Network Address Translation):
- Converts private IP → public IP  
- Used by routers to allow internet access  


## DNS Deep Dive

- DNS translates domain names into IP addresses  

### DNS Resolution Flow:
1. Browser → DNS Resolver  
2. Resolver → Root Server  
3. Root → TLD Server (.com, .org)  
4. TLD → Authoritative Server  
5. Returns IP address  

### Concepts:
- Recursive Query  
- Iterative Query  
- DNS Caching  



## HTTP vs HTTPS

- **HTTP** → Not secure  
- **HTTPS** → Secure (uses SSL/TLS encryption)  

### Ports:
- HTTP → 80  
- HTTPS → 443  

### HTTPS Features:
- Encryption  
- Data integrity  
- Authentication  



## Common Network Protocols

| Protocol | Port | Use Case |
|----------|------|---------|
| HTTP     | 80   | Web browsing |
| HTTPS    | 443  | Secure web |
| FTP      | 21   | File transfer |
| SSH      | 22   | Remote login |
| DNS      | 53   | Domain resolution |
| SMTP     | 25   | Email sending |



## Firewall & Security Basics

- Firewall is a system that filters network traffic  

### Works Based On:
- IP address  
- Port number  
- Protocol  

### Types:
- Stateless Firewall  
- Stateful Firewall  

### Additional Concepts:
- IDS (Intrusion Detection System)  
- IPS (Intrusion Prevention System)  


## Load Balancing

- Distributes incoming traffic across multiple servers  

### Types:
- Round Robin  
- Least Connections  

### Benefits:
- High availability  
- Fault tolerance  
- Better performance  

---

## Proxy & Reverse Proxy

- **Forward Proxy** → Acts on behalf of client  
- **Reverse Proxy** → Acts on behalf of server  

### Example:
- Reverse Proxy: Nginx  

### Uses:
- Security  
- Load balancing  
- Caching  



## Basic Linux Networking Commands

```
ip a
ifconfig
netstat -tuln
ss -tuln
curl
wget
```

## Notes:
- ss is faster and modern alternative to netstat
- curl is used for API testing


## Ports:
- 80 (HTTP): Standard, unencrypted web browsing.
- 443 (HTTPS): Secure, encrypted web browsing.
- 22 (SSH): Secure remote access to another computer.
- 25 (SMTP): Used for sending emails.
- 53 (DNS): Translates website names (like google.com) into IP addresses.

|Command|Usage|
|-------|------|
|`netstat -a`|to See Active Ports |
|`curl -v https://google.com`| To perform a verbose request to Google's servers |
|`netstat -tuln`| To perform a verbose request to Google's servers |
| **CURL Commands** ||
|-v	|Verbose: Shows the full conversation (handshake and headers).|
|-L	|Location: Follows redirects (301, 302).|
|-I	|Head: Fetches only the headers, no body.|
|-k	|Insecure: Ignores SSL/Certificate errors (useful for local testing).|
|-u	|User: For sites requiring a username and password.|

| Code | Meaning      |
| ---- | ------------ |
| 200  | Success      |
| 301  | Redirect     |
| 400  | Bad request  |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not found    |
| 500  | Server error |

| Protocol   | Port |
| ---------- | ---- |
| HTTP       | 80   |
| HTTPS      | 443  |
| SSH        | 22   |
| FTP        | 21   |
| DNS        | 53   |
| MySQL      | 3306 |
| PostgreSQL | 5432 |



##  to see "live" hex traffic:

|Command|Usage|
|-------|------|
|`sudo apt update && sudo apt install tcpdump -y`|To install|
|`sudo tcpdump -i any -X -c 1`| To see the output |



# PROTOCOL

## Application Layer
- HTTP
- HTTPS
- FTP
- SFTP
- SMTP
- POP3
- IMAP
- DNS
- DHCP
- SNMP
- Telnet
- SSH
- NTP
## Transport Layer
- TCP
- UDP
## Network Layer
- IP (IPv4, IPv6)
- ICMP
- ARP
## Others / Supporting
- TLS/SSL
- SIP
- RTP
- SMB
- NFS



|Command|Usage|
|---|---|
|`curl ifconfig.me`|to find your public IP address directly from the command line|
|`curl ifconfig.me/host`|Returns your remote hostname|
|`curl ifconfig.me/ua`|Returns your User Agent string|
|`curl ifconfig.me/all.json`|Returns all available info (IP, user agent, port, etc.) in a structured JSON format|


# DNS Resolution — Step by Step
```
1
Check local cache
Browser checks its own cache. OS checks /etc/hosts. If found → done.
↓
2
Recursive Resolver (your ISP or 8.8.8.8)
Your device asks a resolver. The resolver checks its cache. If cached → returns answer.
↓
3 Root Name Server
Resolver asks: "who handles .com?" Root servers say "Ask the .com TLD servers."
↓
4
TLD Name Server (.com)
"Who handles google.com?" TLD says "Ask Google's name servers: ns1.google.com"
↓
5
Authoritative Name Server (Google's)
Returns the actual IP: 142.250.183.46. Resolver caches this for the TTL period.
```

#### Full DNS trace — see every step
dig +trace google.com

#### Check specific record types
dig google.com A<br>
dig google.com MX<br>
dig google.com TXT

#### Query specific DNS server (bypass cache)
dig @1.1.1.1 example.com    # Cloudflare<br>
dig @8.8.8.8 example.com    # Google

#### Check TTL (time to live)
dig +ttl google.com<br>
google.com.  299  IN  A  142.250.183.46<br>
                ^^ 299 seconds until cache expires

#### View /etc/resolv.conf — your DNS config
cat /etc/resolv.conf<br>
nameserver 8.8.8.8<br>
nameserver 8.8.4.4

#### Flush DNS cache
sudo systemd-resolve --flush-caches  <br>
sudo dscacheutil -flushcache          