# Day 14 – Networking Fundamentals & Hands-on Checks

## Objective

The goal of Day 14 was to understand fundamental networking concepts and practice essential troubleshooting commands used by Linux administrators and DevOps engineers. Networking is a core skill in DevOps because almost every service depends on reliable communication between systems.

---

# Quick Concepts

## OSI Model vs TCP/IP Model

| OSI Model    | TCP/IP Model |
| ------------ | ------------ |
| Application  | Application  |
| Presentation | Application  |
| Session      | Application  |
| Transport    | Transport    |
| Network      | Internet     |
| Data Link    | Link         |
| Physical     | Link         |

### OSI Layers (L1–L7)

#### Layer 1 – Physical

* Responsible for transmitting raw bits over physical media.
* Examples: cables, switches, network cards.

#### Layer 2 – Data Link

* Handles MAC addressing and communication within a local network.
* Examples: Ethernet, ARP.

#### Layer 3 – Network

* Responsible for routing packets between networks.
* Example: IP Protocol.

#### Layer 4 – Transport

* Ensures reliable or fast communication between hosts.
* Examples: TCP and UDP.

#### Layer 5 – Session

* Manages communication sessions between applications.

#### Layer 6 – Presentation

* Handles encryption, compression, and data formatting.

#### Layer 7 – Application

* Provides network services directly to users and applications.
* Examples: HTTP, HTTPS, DNS, SSH.

---

## TCP/IP Stack

### Link Layer

* Handles physical network communication.
* Includes Ethernet and Wi-Fi technologies.

### Internet Layer

* Responsible for IP addressing and routing.
* Uses IPv4 and IPv6.

### Transport Layer

* Provides end-to-end communication.
* Uses TCP and UDP protocols.

### Application Layer

* Includes user-facing protocols such as:

  * HTTP
  * HTTPS
  * DNS
  * SSH
  * FTP

---

## Protocol Placement in the Stack

| Protocol | Layer             |
| -------- | ----------------- |
| IP       | Internet Layer    |
| TCP      | Transport Layer   |
| UDP      | Transport Layer   |
| DNS      | Application Layer |
| HTTP     | Application Layer |
| HTTPS    | Application Layer |
| SSH      | Application Layer |

---

## Real Example

```text
curl https://example.com
```

Flow:

```text
Application Layer → HTTPS
Transport Layer → TCP
Internet Layer → IP
Link Layer → Ethernet/Wi-Fi
```

This demonstrates how an application request travels through multiple networking layers before reaching the destination server.

---

# Hands-on Networking Checks

For all connectivity tests, the target host used was:

```text
google.com
```

---

## 1. Identity Check

### Command

```bash
hostname -I
```

### Sample Output

```text
192.168.1.10
```

### Observation

* Displays the IP address assigned to the machine.
* Confirms the network identity of the host.
* Useful for verifying local network configuration.

---

## 2. Reachability Test

### Command

```bash
ping google.com -c 4
```

### Sample Output

```text
4 packets transmitted, 4 received, 0% packet loss
```

### Observation

* Successfully reached the destination host.
* No packet loss was observed.
* Average latency was within acceptable limits.
* Ping is often the first command used during troubleshooting.

---

## 3. Network Path Analysis

### Command

```bash
traceroute google.com
```

or

```bash
tracepath google.com
```

### Observation

* Shows the route packets take to reach the destination.
* Multiple hops were visible.
* Some routers may not respond due to firewall restrictions.
* Useful for identifying routing issues and network bottlenecks.

---

## 4. Listening Services and Ports

### Command

```bash
ss -tulpn
```

### Sample Output

```text
tcp LISTEN 0 128 *:22
```

### Observation

* SSH service was found listening on port 22.
* Confirms that the service is actively accepting connections.
* Helps identify which applications are exposing network ports.

---

## 5. DNS Resolution Test

### Command

```bash
dig google.com
```

or

```bash
nslookup google.com
```

### Observation

* Domain name successfully resolved to an IP address.
* Confirms that DNS services are functioning correctly.
* DNS issues often prevent users from reaching services even when network connectivity exists.

---

## 6. HTTP Connectivity Check

### Command

```bash
curl -I https://google.com
```

### Sample Output

```text
HTTP/2 200
```

### Observation

* Successfully received HTTP response headers.
* Indicates that the web service is reachable.
* Useful for validating web applications and APIs.

---

## 7. Connection Snapshot

### Command

```bash
netstat -an | head
```

### Observation

* Displayed active network connections.
* Showed LISTEN and ESTABLISHED states.
* Helps identify current network activity and service bindings.

---

# Mini Task – Port Probe & Interpretation

## Identified Service

SSH Service

```text
Port 22
```

### Command

```bash
nc -zv localhost 22
```

### Sample Output

```text
Connection to localhost 22 port [tcp/ssh] succeeded!
```

### Observation

* Port 22 was reachable locally.
* SSH daemon was actively accepting connections.

### If Connection Failed

The following checks would be performed:

1. Verify service status.

```bash
sudo systemctl status ssh
```

2. Check firewall rules.

```bash
sudo ufw status
```

3. Verify port binding.

```bash
ss -tulpn | grep 22
```

---

# Commands Used

```bash
hostname -I

ip addr show

ping google.com -c 4

traceroute google.com

tracepath google.com

ss -tulpn

dig google.com

nslookup google.com

curl -I https://google.com

netstat -an | head

nc -zv localhost 22

systemctl status ssh

ufw status
```

---

# Challenges Faced

### 1. traceroute Command Not Found

Issue:

```text
traceroute: command not found
```

Solution:

```bash
sudo apt install traceroute -y
```

---

### 2. dig Command Not Found

Issue:

```text
dig: command not found
```

Solution:

```bash
sudo apt install dnsutils -y
```

---

### 3. netstat Command Not Found

Issue:

```text
netstat: command not found
```

Solution:

```bash
sudo apt install net-tools -y
```

---

### 4. nc Command Not Found

Issue:

```text
nc: command not found
```

Solution:

```bash
sudo apt install netcat-openbsd -y
```

---

# What I Learned

* Understood the differences between the OSI and TCP/IP networking models.
* Learned where common protocols such as IP, TCP, UDP, HTTP, HTTPS, and DNS fit within the network stack.
* Practiced using networking tools for troubleshooting connectivity issues.
* Learned how to verify DNS resolution and HTTP responses.
* Identified listening services and validated open ports.
* Understood how network packets travel through multiple hops before reaching a destination.

---

# Reflection

## Which command gives the fastest signal when something is broken?

```text
ping
```

Because it quickly verifies basic connectivity and packet loss between systems.

---

## What layer would you inspect if DNS fails?

```text
Application Layer
```

Additionally, verify Internet Layer connectivity because DNS requires successful IP communication.

---

## What layer would you inspect if HTTP 500 appears?

```text
Application Layer
```

The server is reachable, but the application itself is experiencing an internal error.

---

## Two Follow-up Checks During a Real Incident

### Check Service Status

```bash
sudo systemctl status <service-name>
```

Verifies whether the service is running properly.

### Check Logs

```bash
journalctl -xe
```

or

```bash
tail -f /var/log/syslog
```

Provides detailed error messages for troubleshooting.

---

# Why This Matters for DevOps

Networking is the foundation of modern infrastructure. Every application, server, container, and cloud service relies on network communication. Understanding connectivity, DNS resolution, routing, ports, and HTTP responses enables faster troubleshooting and helps maintain reliable production systems.

Day 14 strengthened practical networking skills that are frequently used in real-world DevOps and Site Reliability Engineering (SRE) environments.
