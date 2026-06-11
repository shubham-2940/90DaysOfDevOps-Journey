# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

## Objective

Build a strong foundation in networking concepts that every DevOps Engineer uses daily. This includes understanding DNS resolution, IP addressing, subnetting, CIDR notation, ports, and troubleshooting connectivity between applications and services.

---

# Task 1: DNS – How Names Become IPs

## What Happens When You Type google.com in a Browser?

When a user enters `google.com` in a browser, the browser first checks its local cache for a previously resolved IP address. If no cached record exists, the request is sent to a DNS resolver, which queries DNS servers to find the corresponding IP address. Once the IP address is returned, the browser establishes a connection to the server and loads the website content.

### DNS Resolution Flow

```text
Browser
   ↓
Local DNS Cache
   ↓
DNS Resolver
   ↓
Root DNS Server
   ↓
TLD Server (.com)
   ↓
Authoritative DNS Server
   ↓
IP Address Returned
   ↓
Browser Connects to Server
```

---

## DNS Record Types

### A Record

Maps a domain name to an IPv4 address.

Example:

```text
google.com → 142.250.183.14
```

### AAAA Record

Maps a domain name to an IPv6 address.

Example:

```text
google.com → 2404:6800:4009:80f::200e
```

### CNAME Record

Creates an alias for another domain name.

Example:

```text
www.example.com → example.com
```

### MX Record

Specifies the mail servers responsible for receiving email for a domain.

Example:

```text
gmail.com → mail server information
```

### NS Record

Identifies the authoritative DNS servers for a domain.

Example:

```text
google.com → ns1.google.com
```

---

## DNS Lookup Using dig

### Command

```bash
dig google.com
```

### Sample Output

```text
;; ANSWER SECTION:
google.com.     300    IN    A    142.250.183.14
```

### Analysis

| Field        | Value          |
| ------------ | -------------- |
| Domain       | google.com     |
| TTL          | 300 seconds    |
| Record Type  | A              |
| IPv4 Address | 142.250.183.14 |

### What is TTL?

TTL (Time To Live) specifies how long a DNS response can remain cached before another lookup must be performed.

---

# Task 2: IP Addressing

## What is an IPv4 Address?

IPv4 (Internet Protocol Version 4) is a 32-bit addressing system used to uniquely identify devices on a network.

It consists of four octets separated by dots.

Example:

```text
192.168.1.10
```

Structure:

```text
192 . 168 . 1 . 10
 ↑     ↑     ↑    ↑
Octet Octet Octet Octet
```

Each octet ranges from:

```text
0 – 255
```

---

## Public vs Private IP Address

### Public IP Address

A public IP address is accessible over the internet and is assigned by an Internet Service Provider (ISP).

Example:

```text
8.8.8.8
```

### Private IP Address

A private IP address is used inside internal networks and is not directly reachable from the internet.

Example:

```text
192.168.1.100
```

---

## Differences Between Public and Private IP

| Public IP              | Private IP                           |
| ---------------------- | ------------------------------------ |
| Accessible on Internet | Accessible only inside local network |
| Assigned by ISP        | Assigned by router or DHCP server    |
| Globally unique        | Can be reused in different networks  |
| Example: 8.8.8.8       | Example: 192.168.1.100               |

---

## Private IP Address Ranges

### Class A

```text
10.0.0.0 – 10.255.255.255
```

### Class B

```text
172.16.0.0 – 172.31.255.255
```

### Class C

```text
192.168.0.0 – 192.168.255.255
```

---

## Identifying Private IP Using ip addr show

### Command

```bash
ip addr show
```

### Sample Output

```text
inet 192.168.29.55/24 brd 192.168.29.255 scope global
```

### Analysis

```text
192.168.29.55
```

belongs to:

```text
192.168.x.x
```

Therefore, it is a **Private IP Address**.

---

# Task 3: CIDR & Subnetting

## What Does /24 Mean?

In:

```text
192.168.1.0/24
```

The `/24` indicates that the first 24 bits are reserved for the network portion and the remaining 8 bits are available for host addresses.

Binary Representation:

```text
11111111.11111111.11111111.00000000
```

Subnet Mask:

```text
255.255.255.0
```

---

## Why Do We Subnet?

Subnetting divides a large network into smaller logical networks.

Benefits:

* Better network organization
* Reduced broadcast traffic
* Improved security
* Efficient IP address utilization
* Easier troubleshooting

Example:

Instead of one large network:

```text
192.168.1.0/24
```

We can split it into multiple smaller networks:

```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

---

## Host Calculations

### Formula

```text
Usable Hosts = 2^(Host Bits) - 2
```

The subtraction of 2 accounts for:

* Network Address
* Broadcast Address

---

## CIDR Table

| CIDR | Subnet Mask     | Total IPs | Usable Hosts |
| ---- | --------------- | --------- | ------------ |
| /24  | 255.255.255.0   | 256       | 254          |
| /16  | 255.255.0.0     | 65,536    | 65,534       |
| /28  | 255.255.255.240 | 16        | 14           |

---

## Example Network Breakdown

### Network

```text
192.168.1.0/24
```

### Network Address

```text
192.168.1.0
```

### First Host

```text
192.168.1.1
```

### Last Host

```text
192.168.1.254
```

### Broadcast Address

```text
192.168.1.255
```

---

# Task 4: Ports – The Doors to Services

## What is a Port?

A port is a logical communication endpoint used by applications and services running on a computer.

While an IP address identifies a device, a port identifies a specific application or service running on that device.

Example:

```text
IP Address: 192.168.1.10
Port: 80
```

Together:

```text
192.168.1.10:80
```

This points to a web server running on that machine.

---

## Common Ports

| Port  | Service |
| ----- | ------- |
| 22    | SSH     |
| 80    | HTTP    |
| 443   | HTTPS   |
| 53    | DNS     |
| 3306  | MySQL   |
| 6379  | Redis   |
| 27017 | MongoDB |

---

## Checking Listening Ports

### Command

```bash
ss -tulpn
```

### Sample Output

```text
tcp LISTEN 0 128 0.0.0.0:22
tcp LISTEN 0 511 0.0.0.0:80
```

### Port Mapping

| Port | Service         |
| ---- | --------------- |
| 22   | SSH Server      |
| 80   | HTTP Web Server |

---

## Why Ports Matter in DevOps

Ports allow multiple services to run on the same server simultaneously.

Example:

```text
22    → SSH
80    → Web Server
443   → Secure Web Server
3306  → MySQL Database
6379  → Redis Cache
27017 → MongoDB
```

Without ports, the operating system would not know which application should receive incoming traffic.

---

# Task 5: Putting It Together

## Scenario 1

### Command

```bash
curl http://myapp.com:8080
```

### Networking Concepts Involved

1. DNS resolves `myapp.com` to an IP address.
2. The client establishes a TCP connection to the server.
3. Traffic is directed to port `8080`.
4. HTTP protocol transfers data between client and server.
5. The server responds with the requested content.

---

## Scenario 2

### Problem

```text
Application cannot reach database at:

10.0.1.50:3306
```

### What Would I Check First?

1. Verify the database server is reachable.

```bash
ping 10.0.1.50
```

2. Verify port 3306 is open.

```bash
nc -zv 10.0.1.50 3306
```

3. Check if MySQL service is running.

```bash
systemctl status mysql
```

4. Check firewall rules.

```bash
sudo ufw status
```

5. Verify database IP and port configuration.

6. Confirm routing and network connectivity between application and database servers.

---

# Commands Used During This Exercise

## DNS Lookup

```bash
dig google.com
```

## View Network Interfaces

```bash
ip addr show
```

## View Listening Ports

```bash
ss -tulpn
```

## Connectivity Test

```bash
ping google.com
```

## DNS Lookup Alternative

```bash
nslookup google.com
```

---

# Key Learnings

### 1. DNS Converts Names to IP Addresses

Human-friendly domain names are translated into IP addresses before communication can occur.

### 2. CIDR Defines Network Size

CIDR notation determines how many IP addresses and hosts are available within a network.

### 3. Ports Identify Services

Ports allow multiple applications and services to communicate independently on the same machine.

---

# Conclusion

Today I learned the fundamental networking concepts used by DevOps Engineers daily. I explored how DNS resolves domain names, understood IPv4 addressing and private IP ranges, learned CIDR notation and subnetting basics, examined common service ports, and connected all these concepts through real-world troubleshooting scenarios. These networking fundamentals are essential for deploying, managing, and debugging applications in modern infrastructure environments.
