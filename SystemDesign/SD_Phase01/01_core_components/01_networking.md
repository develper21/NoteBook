# Networking Fundamentals for System Design

## OSI Model Overview

| Layer | Name          | Protocol Data Unit | Examples                     |
|-------|---------------|-------------------|-----------------------------|
| 7     | Application  | Data              | HTTP, FTP, SMTP, WebSockets |
| 6     | Presentation | Data              | SSL/TLS, JPEG, ASCII        |
| 5     | Session      | Data              | NetBIOS, RPC                |
| 4     | Transport    | Segments (TCP)    | TCP, UDP                    |
| 3     | Network      | Packets           | IP, ICMP, Routers           |
| 2     | Data Link    | Frames            | Ethernet, Switches, MAC     |
| 1     | Physical     | Bits              | Cables, Hubs, Repeaters     |

## Key Protocols

### HTTP (Hypertext Transfer Protocol)
- **Port**: 80 (HTTP), 443 (HTTPS)
- **Methods**: GET, POST, PUT, DELETE, PATCH
- **Stateless**: Each request is independent
- **Headers**: Key-value pairs for metadata
- **Status Codes**: 
  - 2xx: Success (200 OK, 201 Created)
  - 3xx: Redirection (301 Moved Permanently, 302 Found)
  - 4xx: Client Error (400 Bad Request, 404 Not Found)
  - 5xx: Server Error (500 Internal Server Error)

### TCP vs UDP

| Feature          | TCP                          | UDP                          |
|------------------|------------------------------|------------------------------|
| Connection      | Connection-oriented         | Connectionless               |
| Reliability     | Reliable, in-order delivery | Unreliable, no ordering      |
| Flow Control    | Yes                         | No                           |
| Congestion Ctrl | Yes                         | No                           |
| Speed           | Slower                      | Faster                       |
| Use Cases       | Web, Email, File Transfer   | Video Streaming, DNS, Gaming |

### DNS (Domain Name System)
- **Purpose**: Translates domain names to IP addresses
- **Hierarchy**: Root → TLD (.com, .org) → Domain → Subdomain
- **Record Types**:
  - A: Maps hostname to IPv4
  - AAAA: Maps hostname to IPv6
  - CNAME: Alias of one name to another
  - MX: Mail server records
  - TXT: Text records (e.g., for verification)
- **DNS Lookup Process**:
  1. Check browser cache
  2. Check OS cache
  3. Contact recursive DNS server
  4. Root server → TLD server → Authoritative server

### WebSockets
- **Purpose**: Full-duplex communication over a single TCP connection
- **Handshake**: Starts as HTTP, then upgrades to WebSocket
- **Use Cases**:
  - Real-time applications
  - Chat applications
  - Live updates
  - Multiplayer games

## Network Latency

### Factors Affecting Latency
1. **Propagation Delay**: Time for signal to travel
2. **Transmission Delay**: Time to push packets onto the link
3. **Processing Delay**: Time to process packet headers
4. **Queuing Delay**: Time spent in router buffers

### Typical Latencies
- Same datacenter: <1ms
- Cross-country: 40-100ms
- Intercontinental: 100-300ms
- Satellite: 500-800ms

## Network Throughput

### Bandwidth vs Throughput
- **Bandwidth**: Maximum rate of data transfer
- **Throughput**: Actual rate of successful data transfer

### Calculating Throughput
```
Throughput = Window Size / Round Trip Time (RTT)
```

### Common Bandwidths
- Home Internet: 10-1000 Mbps
- Mobile Data (4G): 10-100 Mbps
- Data Center: 1-100 Gbps
- Inter-DC Links: 10-400 Gbps

## Security

### HTTPS
- HTTP over SSL/TLS
- Encrypts data in transit
- Requires SSL certificate
- Port 443

### VPN (Virtual Private Network)
- Creates secure tunnel
- Encrypts all traffic
- Masks IP address

### Firewalls
- Filters incoming/outgoing traffic
- Can be hardware or software
- Stateful vs Stateless

## Load Balancing
(Note: Covered in detail in 02_load_balancing.md)
- Distributes traffic across servers
- Improves scalability and reliability
- Can be L4 (transport) or L7 (application)
