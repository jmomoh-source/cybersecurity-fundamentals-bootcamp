# Networking Fundamentals: A Technical Session
> Covering TCP/IP, DNS, HTTP/HTTPS — with Live Demos using `ping`, `nslookup`, and `curl`

---

## Table of Contents
1. [The TCP/IP Model](#1-the-tcpip-model)
2. [TCP vs IP — What's the Difference?](#2-tcp-vs-ip--whats-the-difference)
3. [DNS — The Internet's Phone Book](#3-dns--the-internets-phone-book)
4. [HTTP vs HTTPS](#4-http-vs-https)
5. [Live Demo: ping](#5-live-demo-ping)
6. [Live Demo: nslookup](#6-live-demo-nslookup)
7. [Live Demo: curl](#7-live-demo-curl)
8. [Putting It All Together](#8-putting-it-all-together)

---

## 1. The TCP/IP Model

TCP/IP is not a single protocol — it is a **suite of protocols** (a collection of rules) that governs how data is sent, routed, and received across networks and the internet.

Think of it as a **postal system** for the internet:
- You write a letter (your data).
- You put it in an envelope with an address (IP).
- A courier picks it up and ensures it is delivered safely and in order (TCP).

### The Four Layers of TCP/IP

| Layer | Name | Responsibility | Example Protocols |
|-------|------|----------------|-------------------|
| 4 | **Application** | What the user/application sees | HTTP, HTTPS, DNS, FTP, SSH |
| 3 | **Transport** | Breaks data into segments, ensures delivery | TCP, UDP |
| 2 | **Internet** | Addressing and routing packets | IP (IPv4, IPv6), ICMP |
| 1 | **Network Access** | Physical transmission of data | Ethernet, Wi-Fi |

### How Data Flows Through the Layers

When you visit `google.com`, here is what happens layer by layer:

```
Your Browser (Application Layer)
        ↓  HTTP Request
Transport Layer -> Breaks it into TCP segments, adds port numbers
        ↓
Internet Layer  -> Wraps each segment in an IP packet with source/destination IP
        ↓
Network Layer   -> Converts to electrical signals / radio waves and sends it
        ↓
       ... travels across the internet ...
        ↑
Google's server reassembles everything in reverse order
```

---

## 2. TCP vs IP — What's the Difference?

Although they are often mentioned together, **TCP and IP are two separate protocols** that do two very different jobs.

### IP (Internet Protocol)
IP is responsible for **addressing and routing**. Its only job is to get a packet from one machine to another using IP addresses.

- Every device on the internet has an **IP address** (e.g., `142.250.184.78`)
- IP is **connectionless** — it just sends packets without knowing if they arrived
- IP does **not** guarantee delivery, order, or integrity
- Think of IP as the **address on the envelope**

#### IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address format | `192.168.1.1` | `2001:0db8:85a3::8a2e:0370:7334` |
| Address space | ~4.3 billion addresses | ~340 undecillion addresses |
| Header size | 20 bytes | 40 bytes |
| Status | Exhausted, still dominant | Gradually being adopted |

### TCP (Transmission Control Protocol)
TCP is responsible for **reliable, ordered delivery** of data. It sits on top of IP and adds the reliability that IP lacks.

- TCP establishes a **connection** before any data is sent
- It ensures all packets arrive and are **reassembled in the right order**
- It handles **retransmission** if packets are lost
- Think of TCP as the **courier who confirms delivery and calls you if something is missing**

### The TCP Three-Way Handshake
Before any data transfer, TCP performs a handshake to establish a connection:

```
Client                          Server
  |                               |
  |  ----  SYN (Hey, can we talk?) ---->  |
  |                               |
  |  <--- SYN-ACK (Sure, I'm ready!) ---  |
  |                               |
  |  ----  ACK (Great, let's go!) ---->   |
  |                               |
  |  ====  Data Transfer Begins  ======   |
```

- **SYN** — Synchronize: Client initiates connection
- **SYN-ACK** — Server acknowledges and responds
- **ACK** — Client confirms; connection is established

### Summary: TCP vs IP in One Line
> **IP** gets the packet to the right machine. **TCP** makes sure the data arrives correctly and completely.

---

## 3. DNS — The Internet's Phone Book

### What is DNS?

**DNS (Domain Name System)** is the system that translates human-readable domain names into IP addresses that computers can understand.

You type `www.google.com` -> DNS resolves it to `142.250.184.78` -> Your browser connects to that IP.

Without DNS, you would need to memorize IP addresses for every website you visit.

### How DNS Resolution Works (Step by Step)

```
You type: www.google.com
         ↓
1. Browser Cache       -> "Have I looked this up before?" (checks local cache)
         ↓  (not found)
2. OS / hosts file     -> Checks /etc/hosts for a local override
         ↓  (not found)
3. Recursive Resolver  -> Your ISP's DNS server (e.g., 8.8.8.8 — Google's DNS)
         ↓
4. Root Name Server    -> "I don't know google.com, but ask the .com server"
         ↓
5. TLD Name Server     -> "I don't know google.com, but ask Google's server"
(.com server)           
         ↓
6. Authoritative NS    -> "www.google.com = 142.250.184.78" 
(Google's own server)   
         ↓
Answer returned -> Browser connects to 142.250.184.78 
```

### Key DNS Terms

- **TTL (Time To Live)** — How long a DNS result is cached (in seconds) before being refreshed
- **Recursive Resolver** — The server (usually your ISP or public DNS like `8.8.8.8`) that does the full lookup on your behalf
- **Authoritative Name Server** — The server that holds the actual, final DNS records for a domain

---

## 4. HTTP vs HTTPS

### What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the protocol used to transfer web pages and data between a browser and a web server. It defines how requests and responses are structured.

An HTTP interaction looks like this:

**Request (Browser -> Server):**
```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

**Response (Server -> Browser):**
```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<html>...</html>
```

### HTTP Methods

| Method | Purpose |
|--------|---------|
| **GET** | Retrieve a resource |
| **POST** | Send data to the server (e.g., form submission) |
| **PUT** | Update/replace a resource |
| **PATCH** | Partially update a resource |
| **DELETE** | Delete a resource |

### HTTP Status Codes

| Code Range | Meaning | Common Examples |
|------------|---------|-----------------|
| **2xx** | Success | `200 OK`, `201 Created` |
| **3xx** | Redirection | `301 Moved Permanently`, `302 Found` |
| **4xx** | Client Error | `400 Bad Request`, `401 Unauthorized`, `404 Not Found` |
| **5xx** | Server Error | `500 Internal Server Error`, `503 Service Unavailable` |

### What is HTTPS?

**HTTPS (HTTP Secure)** is simply HTTP with an added layer of encryption provided by **TLS (Transport Layer Security)** — formerly known as SSL.

```
HTTP  ->  Plain text over TCP/IP  (anyone on the network can read it)
HTTPS ->  Encrypted text over TLS/TCP/IP  (data is unreadable to eavesdroppers)
```

### HTTP vs HTTPS — Side by Side

| Feature | HTTP | HTTPS |
|---------|------|-------|
| Port | **80** | **443** |
| Encryption |  None |  TLS/SSL |
| Data Privacy |  Visible to anyone | Encrypted end-to-end |
| Authentication |  No server identity proof |  Certificate verifies identity |
| SEO & Trust | Lower trust signals | Preferred by browsers & Google |
| Use case | Internal/local systems, APIs (sometimes) | All public-facing websites |

>  With HTTP, if you log into a website on a public Wi-Fi, anyone on that network can see your username and password. HTTPS prevents this.

---

## 5. Live Demo: `ping`

### What is `ping`?

`ping` is a command-line tool that tests **network connectivity** between your machine and another host. It works by sending **ICMP (Internet Control Message Protocol) Echo Request** packets and waiting for **Echo Reply** packets back.

It answers the question: **"Can my machine reach this host, and how long does it take?"**

### Basic Syntax
```bash
ping [options] <hostname or IP>
```

### Demo Commands

**1. Ping a domain name (DNS + connectivity test)**
```bash
ping google.com
```
> This first resolves `google.com` to an IP via DNS, then sends ICMP packets. Shows you both that DNS is working and the host is reachable.

**2. Ping a specific IP address (no DNS involved)**
```bash
ping 8.8.8.8
```
> Google's public DNS server. A reliable host to test raw IP connectivity.

**3. Send a fixed number of packets (instead of running forever)**
```bash
ping -c 4 google.com
```
> `-c 4` sends exactly 4 packets then stops. Good for demos.

**4. Ping with a custom interval between packets**
```bash
ping -c 4 -i 0.5 google.com
```
> `-i 0.5` sends a packet every 0.5 seconds instead of the default 1 second.

**5. Test an unreachable host (to see what failure looks like)**
```bash
ping 192.168.99.99
```
> This should time out because no device has that IP

---

## 6. Live Demo: `nslookup`

### What is `nslookup`?

`nslookup` (Name Server Lookup) is a tool for **querying DNS** to find the IP address associated with a domain, or other DNS records like MX, CNAME, etc. It lets you inspect DNS directly.

### Basic Syntax
```bash
nslookup [options] <domain> [dns-server]
```

### Demo Commands

**1. Basic DNS lookup — find the IP of a domain**
```bash
nslookup google.com
```
> Returns the A record (IPv4 address) for `google.com`.

**2. Look up a specific record type (MX — mail servers)**
```bash
nslookup -type=MX gmail.com
```
> Shows which mail servers handle email for `gmail.com`. Great for demonstrating MX records.

**3. Look up NS records (who manages this domain's DNS?)**
```bash
nslookup -type=NS google.com
```
> Returns the authoritative name servers for `google.com`.

**4. Reverse DNS lookup — find the domain for an IP**
```bash
nslookup 8.8.8.8
```
> Takes an IP and returns its associated hostname (if a PTR record exists). This is `dns.google`.

**5. Query a specific DNS server (bypass your default resolver)**
```bash
nslookup google.com 1.1.1.1
```
> Sends the query directly to Cloudflare's DNS (`1.1.1.1`) instead of your system's default DNS. Useful for comparing results.

**6. Look up TXT records (SPF, DKIM, domain verification)**
```bash
nslookup -type=TXT google.com
```
> Returns TXT records — often used for email security (SPF) and domain ownership verification.


> **Non-authoritative answer** means the result was returned from a resolver's cache, not directly from the domain's own name server. 

---

## 7. Live Demo: `curl`

### What is `curl`?

`curl` (Client URL) is a powerful command-line tool for **making HTTP/HTTPS requests** and transferring data to/from servers. It supports HTTP, HTTPS, FTP, and many other protocols.

Think of it as a browser without the graphical interface — it lets you see the raw HTTP interactions.

### Basic Syntax
```bash
curl [options] <URL>
```

### Demo Commands

**1. Simple GET request — fetch a webpage**
```bash
curl http://scanme.nmap.org/
```
> Returns the HTML body of `example.com`. The most basic HTTP GET request.

**2. Show the HTTP response headers**
```bash
curl -I http://scanme.nmap.org/
```
> `-I` sends a HEAD request and shows only headers (not the body). Great for seeing status codes, content type, server info.

**3. Show both request and response headers (verbose mode)**
```bash
curl -v http://scanme.nmap.org/
```
> `-v` (verbose) shows the full conversation: DNS resolution, TLS handshake, request headers, response headers, and body. 

**4. Follow redirects automatically**
```bash
curl -L http://google.com
```
> `-L` follows HTTP redirects. Without it, you'd just see the `301 Moved Permanently` response. This shows the HTTP → HTTPS redirect in action.

**5. See the difference between HTTP and HTTPS**
```bash
# HTTP — no encryption, may redirect
curl -v http://google.com

# HTTPS — you'll see TLS handshake in verbose output
curl -v https://google.com
```

**6. Make a POST request with JSON data**
```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title": "Hello", "body": "World", "userId": 1}'
```
> `-X POST` sets the method, `-H` adds a header, `-d` sends the request body. Uses a free test API.

**7. Check the HTTP status code only**
```bash
curl -o /dev/null -s -w "%{http_code}\n" https://google.com
```
> Returns just the status code (e.g., `200`). Useful in scripts to check if a site is up.

**8. Download a file**
```bash
curl -O https://nmap.org/download.html
```
> `-O` saves the file with its original name to your current directory.

**9. Test a REST API (GET request)**
```bash
curl https://jsonplaceholder.typicode.com/users/1
```
> Hits a free public REST API and returns JSON. Great for showing real-world API usage.

---

## 8. Putting It All Together

Here is what actually happens when you type `https://www.google.com` and press Enter:

```
1. DNS (nslookup)
   -> Browser queries DNS to resolve www.google.com → 142.250.184.78

2. TCP/IP (ping)
   -> Your machine opens a TCP connection to 142.250.184.78 on port 443
   -> Three-way handshake: SYN → SYN-ACK → ACK

3. TLS Handshake (HTTPS)
   -> Client and server negotiate encryption
   -> Server presents its SSL certificate
   -> A shared encryption key is established

4. HTTP over TLS (curl)
   -> Browser sends: GET / HTTP/1.1 (encrypted)
   -> Server responds: 200 OK + HTML page (encrypted)

5. Rendering
   -> Browser decrypts the response and renders the page
```

Every tool in today's demo maps directly to one of these steps:

| Tool | Protocol Layer | What it tests |
|------|---------------|---------------|
| `ping` | IP / ICMP | Can I reach this machine? |
| `nslookup` | DNS (Application) | Can I resolve this domain name? |
| `curl` | HTTP/HTTPS (Application) | Can I make a full web request? |

---
