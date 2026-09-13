# Lesson 10: Linux Networking Basics 🌐

This is a **very important Linux lesson for Cloud/DevOps** because almost everything you do on AWS—EC2, VPC, ALB, RDS, SSH, Docker, Kubernetes—depends on networking.

---

# 1. What is Networking?

**Networking = communication between two or more devices/systems.**

Example:

```text
Your Laptop
     |
   Internet
     |
    AWS
     |
    EC2
     |
    RDS
```

When your laptop connects to an EC2 server, networking is responsible for moving the data between them.

For Cloud/DevOps, you should understand:

```text
IP → Port → Protocol → DNS → Connection
```

---

# 2. IP Address

An **IP address** identifies a device/interface on a network.

Example:

```text
192.168.1.10
```

AWS EC2 private IP:

```text
10.0.1.25
```

Public IP:

```text
13.234.XX.XX
```

### Two major types

| Type | Example | Purpose |
|---|---|---|
| Private IP | `10.0.1.25` | Internal communication |
| Public IP | `13.234.XX.XX` | Internet communication |

Private IP ranges include:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

---

# 3. IPv4

Most Linux networking you'll initially work with uses **IPv4**.

Example:

```text
192.168.1.100
```

IPv4 contains **32 bits**.

```text
192 . 168 . 1 . 100
```

Each section can range from:

```text
0 → 255
```

---

# 4. IPv6

IPv6 uses **128 bits**.

Example:

```text
2001:db8::1
```

You don't need to master IPv6 immediately for your Cloud/DevOps roadmap.

### Priority

```text
IPv4       → MUST KNOW
IPv6 basics → SHOULD KNOW
Advanced IPv6 → OPTIONAL for now
```

---

# 5. MAC Address

A **MAC address** identifies a network interface at the Layer-2 level.

Example:

```text
02:42:ac:11:00:02
```

On Linux:

```bash
ip link
```

Example:

```text
eth0
    link/ether 02:42:ac:11:00:02
```

Think:

```text
MAC → Network interface identity
IP  → Network-layer address
```

---

# 6. Network Interface

A network interface allows your Linux machine to communicate over a network.

Common names:

```text
eth0
ens5
enp0s3
lo
```

Check interfaces:

```bash
ip link
```

or:

```bash
ip addr
```

---

# 7. `ip` Command ⭐⭐⭐

This is one of the **most important Linux networking commands**.

Check IP addresses:

```bash
ip addr
```

Short version:

```bash
ip a
```

Example:

```text
2: eth0:
    inet 10.0.1.25/24
```

This means:

```text
Interface = eth0
IP        = 10.0.1.25
Subnet    = /24
```

---

## Check routing

```bash
ip route
```

Example:

```text
default via 10.0.1.1 dev eth0
10.0.1.0/24 dev eth0
```

The important part:

```text
default via 10.0.1.1
```

This is the **default gateway**.

---

# 8. Default Gateway

A default gateway is where traffic goes when the destination isn't on the local network.

Example:

```text
EC2
10.0.1.25
   |
   | default gateway
   ↓
10.0.1.1
   |
   ↓
Network
```

Check it:

```bash
ip route
```

---

# 9. Port

An **IP identifies the machine/interface.**

A **port identifies a network service/application on that machine.**

Think:

```text
IP   = Building address
Port = Apartment/room
```

Example:

```text
192.168.1.10:22
```

means:

```text
IP   = 192.168.1.10
Port = 22
```

---

# 10. Important Ports ⭐⭐⭐

Memorize these:

| Port | Protocol/Service | Purpose |
|---:|---|---|
| 22 | SSH | Remote Linux access |
| 23 | Telnet | Remote access |
| 25 | SMTP | Email |
| 53 | DNS | Name resolution |
| 80 | HTTP | Web |
| 443 | HTTPS | Secure web |
| 3306 | MySQL | Database |
| 5432 | PostgreSQL | Database |
| 6379 | Redis | Cache |
| 8080 | HTTP alternative | Java/Tomcat/common apps |
| 3000 | Node.js/common dev server | Application |
| 5000 | Flask/common app | Application |

### Cloud example

Your three-tier application:

```text
Internet
   ↓
ALB :443
   ↓
EC2/Nginx :80
   ↓
Tomcat :8080
   ↓
RDS :3306
```

This is why **ports are extremely important in AWS Security Groups**.

---

# 11. TCP

**TCP = Transmission Control Protocol**

TCP is:

- connection-oriented
- reliable
- ordered
- retransmits lost packets

Used by:

```text
HTTP
HTTPS
SSH
FTP
MySQL
PostgreSQL
```

Example:

```text
Client
  |
  | TCP connection
  ↓
Server
```

---

# 12. UDP

**UDP = User Datagram Protocol**

UDP is:

- connectionless
- faster
- no guaranteed delivery
- lower overhead

Common uses:

```text
DNS
DHCP
Streaming
VoIP
Gaming
```

Simple comparison:

| TCP | UDP |
|---|---|
| Reliable | Faster |
| Connection-oriented | Connectionless |
| Ordered | No guaranteed ordering |
| More overhead | Less overhead |
| SSH/HTTPS | DNS/common real-time traffic |

---

# 13. TCP 3-Way Handshake

Before establishing a TCP connection:

```text
Client                  Server
  |                       |
  | ------ SYN ---------> |
  | <----- SYN-ACK ------ |
  | ------ ACK ---------> |
  |                       |
  |   Connection ready    |
```

Remember:

```text
SYN
SYN-ACK
ACK
```

### Interview question

**Q: What is TCP 3-way handshake?**

Answer:

> It is the process used by TCP to establish a connection between a client and server using SYN, SYN-ACK, and ACK packets.

---

# 14. `ss` Command ⭐⭐⭐

`ss` shows socket/network connection information.

Check listening ports:

```bash
ss -lnt
```

Useful version:

```bash
ss -lntp
```

Meaning:

```text
-l = listening
-n = numeric
-t = TCP
-p = process
```

Example:

```text
LISTEN 0 128 0.0.0.0:80
```

This means a service is listening on port 80.

---

# 15. Check a Specific Port

```bash
ss -lntp | grep :80
```

For Tomcat:

```bash
ss -lntp | grep :8080
```

For SSH:

```bash
ss -lntp | grep :22
```

---

# 16. `netstat`

Older command:

```bash
netstat
```

Example:

```bash
netstat -lntp
```

However, modern Linux systems generally prefer:

```bash
ss
```

### Remember

```text
netstat → older/common legacy tool
ss      → modern preferred tool
```

You should know both for interviews.

---

# 17. `ping` ⭐⭐⭐

`ping` tests basic network reachability.

Example:

```bash
ping google.com
```

or:

```bash
ping 8.8.8.8
```

Stop:

```text
Ctrl + C
```

Example output:

```text
64 bytes from ...
time=20 ms
```

### Important

Successful ping doesn't necessarily mean that a particular application port is working.

For example:

```text
ping server
```

can work while:

```text
server:8080
```

is unavailable.

---

# 18. `curl` ⭐⭐⭐

`curl` is extremely important for DevOps.

It can make HTTP requests and test web services.

Example:

```bash
curl http://example.com
```

Check headers:

```bash
curl -I http://example.com
```

Verbose:

```bash
curl -v http://example.com
```

Test local Nginx:

```bash
curl http://localhost
```

Test port 8080:

```bash
curl http://localhost:8080
```

---

# 19. Why `curl` is Important in DevOps

Suppose your browser cannot access your application.

Instead of immediately blaming AWS, test from the server:

```bash
curl localhost
```

If this works:

```text
EC2 → Nginx works
```

Then investigate:

```text
Security Group
ALB
NACL
Route Table
DNS
```

This is a very common troubleshooting technique.

---

# 20. `wget`

`wget` is mainly used to download files.

Example:

```bash
wget https://example.com/file.zip
```

Another example:

```bash
wget http://example.com/index.html
```

Difference:

```text
curl → request/test APIs/services
wget → download files
```

Both can do more, but this mental model is useful.

---

# 21. DNS ⭐⭐⭐

**DNS = Domain Name System**

DNS converts:

```text
google.com
```

into an IP address.

Conceptually:

```text
Browser
   |
   | google.com
   ↓
DNS
   |
   | 142.250.x.x
   ↓
Server
```

Without DNS, users would need to remember IP addresses.

---

# 22. `nslookup`

Check DNS:

```bash
nslookup google.com
```

Example:

```text
Name: google.com
Address: 142.250.x.x
```

---

# 23. `dig` ⭐⭐⭐

`dig` provides detailed DNS information.

```bash
dig google.com
```

Short answer:

```bash
dig +short google.com
```

This is particularly useful for DevOps troubleshooting.

---

# 24. DNS Troubleshooting

Suppose:

```text
https://myapp.example.com
```

is not working.

First check:

```bash
dig +short myapp.example.com
```

If DNS returns the expected IP/load balancer address, DNS is resolving.

Then:

```bash
curl -I https://myapp.example.com
```

Now you can determine whether the issue is DNS or HTTP/application connectivity.

---

# 25. HTTP vs HTTPS

### HTTP

```text
Port 80
```

### HTTPS

```text
Port 443
```

HTTPS uses encryption through TLS.

Typical web request:

```text
Client
  |
  | HTTPS :443
  ↓
ALB
  |
  | HTTP :80
  ↓
Nginx
```

Or:

```text
ALB :443
   ↓
EC2 :8080
```

depending on architecture.

---

# 26. SSH ⭐⭐⭐

**SSH = Secure Shell**

Used to remotely connect to Linux servers.

Example:

```bash
ssh -i key.pem ec2-user@<EC2-IP>
```

Typical AWS:

```text
Your Laptop
     |
     | SSH :22
     ↓
EC2
```

Security Group must allow:

```text
TCP 22
```

from an appropriate source.

---

# 27. SSH Troubleshooting

If SSH doesn't work:

### Step 1 — Check EC2 running

```text
EC2 → Running
```

### Step 2 — Check public IP

```text
Correct IP?
```

### Step 3 — Security Group

```text
TCP 22 allowed?
```

### Step 4 — Network route

```text
Internet Gateway?
Route table?
```

### Step 5 — Local key permissions

```bash
chmod 400 key.pem
```

### Step 6 — Test SSH

```bash
ssh -i key.pem ec2-user@<IP>
```

---

# 28. Localhost

`localhost` refers to the current machine.

IPv4:

```text
127.0.0.1
```

Example:

```bash
ping localhost
```

or:

```bash
curl http://localhost
```

If Nginx is running:

```bash
curl localhost
```

may return your web page.

---

# 29. Loopback Interface

Linux has a loopback interface:

```text
lo
```

Check:

```bash
ip addr
```

You'll see something like:

```text
lo:
    inet 127.0.0.1/8
```

Traffic to:

```text
127.0.0.1
```

stays inside the machine.

---

# 30. `/etc/hosts`

Linux can locally map hostnames to IP addresses.

View:

```bash
cat /etc/hosts
```

Example:

```text
127.0.0.1 localhost
10.0.1.25 app-server
10.0.2.25 db-server
```

Then:

```bash
ping app-server
```

can resolve locally without DNS.

---

# 31. `/etc/resolv.conf`

Contains DNS resolver configuration.

Check:

```bash
cat /etc/resolv.conf
```

You may see:

```text
nameserver ...
```

This tells the system which DNS resolver to use.

---

# 32. Network Configuration Commands — Quick Table

| Command | Purpose |
|---|---|
| `ip addr` | Show IP addresses |
| `ip link` | Show interfaces |
| `ip route` | Show routing table |
| `ping` | Test reachability |
| `ss` | Show sockets/connections |
| `netstat` | Legacy network connections |
| `curl` | Test HTTP/API |
| `wget` | Download files |
| `nslookup` | DNS lookup |
| `dig` | Detailed DNS lookup |
| `ssh` | Remote connection |
| `hostname` | Show hostname |
| `hostname -I` | Show IP addresses |

---

# 33. `hostname`

Check hostname:

```bash
hostname
```

Example:

```text
ip-10-0-1-25
```

Check IP:

```bash
hostname -I
```

---

# 34. Linux Networking Troubleshooting Flow ⭐⭐⭐

This is something you should **memorize**.

Suppose:

```text
Application is not accessible
```

Don't randomly change settings.

Follow:

```text
1. Is the server running?
        ↓
2. Does it have an IP?
        ↓
3. Is the service running?
        ↓
4. Is the port listening?
        ↓
5. Is local connectivity working?
        ↓
6. Is DNS resolving?
        ↓
7. Is the firewall/Security Group allowing it?
        ↓
8. Is routing correct?
        ↓
9. Is the application responding?
```

Commands:

```bash
ip addr
```

```bash
systemctl status nginx
```

```bash
ss -lntp
```

```bash
curl localhost
```

```bash
dig example.com
```

```bash
ip route
```

---

# 35. Real AWS Example

Suppose you deployed an Nginx website on EC2.

Architecture:

```text
Internet
    |
    | HTTP :80
    ↓
EC2 Public IP
    |
    ↓
Nginx
```

On EC2:

### Check IP

```bash
ip addr
```

### Check Nginx

```bash
systemctl status nginx
```

### Check port

```bash
ss -lntp | grep :80
```

### Test locally

```bash
curl localhost
```

If this works but the website doesn't open externally:

```text
Nginx ✔
Port 80 ✔
Application ✔

Investigate:
Security Group
NACL
Route Table
Internet Gateway
Public IP
```

This distinction is **very important for AWS troubleshooting**.

---

# 36. Three-Tier Application Networking

This connects directly to your AWS project.

```text
                 Internet
                    |
                    | :443
                    ↓
              ┌───────────┐
              │    ALB    │
              └─────┬─────┘
                    |
                    | :80
                    ↓
              ┌───────────┐
              │   Nginx   │
              └─────┬─────┘
                    |
                    | :8080
                    ↓
              ┌───────────┐
              │  Tomcat   │
              └─────┬─────┘
                    |
                    | :3306
                    ↓
              ┌───────────┐
              │   RDS     │
              └───────────┘
```

Every connection involves:

```text
Source
Destination
IP
Port
Protocol
Route
Security rules
```

---

# 37. Very Important: IP vs Port vs Protocol

Don't confuse them.

Example:

```text
10.0.2.25:8080/TCP
```

Means:

```text
IP       = 10.0.2.25
Port     = 8080
Protocol = TCP
```

Think:

```text
WHO?     → IP
WHERE?   → Port
HOW?     → Protocol
```

---

# 38. Practical Lab 🧪

Do this on your Amazon Linux/Ubuntu VM or EC2.

## Lab 1 — Network information

```bash
hostname
```

```bash
hostname -I
```

```bash
ip addr
```

```bash
ip link
```

```bash
ip route
```

---

## Lab 2 — Test connectivity

```bash
ping 8.8.8.8
```

Then:

```bash
ping google.com
```

Compare the results.

---

## Lab 3 — DNS

```bash
nslookup google.com
```

Then:

```bash
dig google.com
```

Then:

```bash
dig +short google.com
```

---

## Lab 4 — HTTP

```bash
curl http://example.com
```

Headers:

```bash
curl -I http://example.com
```

Verbose:

```bash
curl -v http://example.com
```

---

## Lab 5 — Check ports

```bash
ss -lntp
```

If Nginx is installed:

```bash
ss -lntp | grep :80
```

---

## Lab 6 — Localhost

```bash
curl localhost
```

If Nginx isn't installed, install it on Amazon Linux with:

```bash
sudo yum install nginx -y
```

Start it:

```bash
sudo systemctl enable --now nginx
```

Then:

```bash
curl localhost
```

Check:

```bash
ss -lntp | grep :80
```

---

# 39. Troubleshooting Scenario 🎯

### Problem

You deployed Nginx on EC2, but the browser says:

```text
This site can't be reached
```

### Step 1

```bash
systemctl status nginx
```

If inactive:

```bash
sudo systemctl start nginx
```

### Step 2

```bash
ss -lntp | grep :80
```

### Step 3

```bash
curl localhost
```

If this works:

```text
Nginx is working locally.
```

### Step 4

Check AWS:

```text
Security Group
    ↓
TCP 80
    ↓
Source allowed?
```

### Step 5

Check:

```text
Public IP
Route Table
Internet Gateway
NACL
```

This is how a Cloud Engineer should troubleshoot instead of simply restarting the server repeatedly.

---

# 40. Important Interview Questions

### Q1. What is an IP address?

An IP address identifies a device or network interface on a network.

### Q2. What is a port?

A port identifies a specific network service/application on a host.

### Q3. Difference between IP and port?

```text
IP   → identifies host/interface
Port → identifies service
```

### Q4. What is TCP?

A reliable, connection-oriented transport protocol.

### Q5. What is UDP?

A connectionless transport protocol with lower overhead and no delivery guarantee.

### Q6. What is DNS?

DNS translates domain names into IP addresses.

### Q7. What does `ping` do?

Tests basic network reachability using ICMP.

### Q8. What does `curl` do?

Makes network requests and is commonly used to test HTTP/HTTPS services and APIs.

### Q9. What does `ss` do?

Displays socket, listening-port, and network connection information.

### Q10. `ss` vs `netstat`?

`ss` is the modern preferred tool; `netstat` is an older/legacy tool.

### Q11. What is localhost?

The current machine, commonly represented by `127.0.0.1` for IPv4.

### Q12. What is the default gateway?

The router used to forward traffic to destinations outside the local network.

### Q13. What is TCP 3-way handshake?

```text
SYN → SYN-ACK → ACK
```

used to establish a TCP connection.

### Q14. What port does SSH use?

```text
22/TCP
```

### Q15. HTTP and HTTPS ports?

```text
HTTP  → 80
HTTPS → 443
```

---

# 41. MUST KNOW for Cloud/DevOps ⭐⭐⭐

Before moving forward, make sure you know these:

### Commands

```bash
ip addr
ip link
ip route
ping
ss -lntp
curl
wget
nslookup
dig
ssh
hostname
hostname -I
```

### Concepts

```text
IP address
Private vs Public IP
MAC address
Network interface
Port
TCP
UDP
DNS
HTTP
HTTPS
SSH
localhost
Default Gateway
Routing
TCP 3-way handshake
```

### AWS connection

You should understand:

```text
EC2
 ↓
Private IP
 ↓
Public IP
 ↓
Security Group
 ↓
Port
 ↓
Protocol
 ↓
Route Table
 ↓
Internet Gateway
```

---

# 🧠 Lesson 10 Cheat Sheet

```text
IP
 └── Identifies host/interface

MAC
 └── Identifies network interface at Layer 2

PORT
 └── Identifies service

TCP
 └── Reliable + connection-oriented

UDP
 └── Connectionless + lower overhead

DNS
 └── Domain → IP

PING
 └── Reachability

CURL
 └── HTTP/API testing

SS
 └── Listening ports/connections

SSH
 └── Remote Linux access

HTTP
 └── 80

HTTPS
 └── 443

SSH
 └── 22

MYSQL
 └── 3306

TOMCAT
 └── commonly 8080
```

## 🔥 Remember this troubleshooting formula

```text
IP → Route → Port → Service → DNS → Firewall/Security Group → Application
```

And for AWS:

```text
Client
  ↓
DNS
  ↓
Public IP / ALB
  ↓
Route
  ↓
Security Group
  ↓
Port
  ↓
Service
  ↓
Application
```

