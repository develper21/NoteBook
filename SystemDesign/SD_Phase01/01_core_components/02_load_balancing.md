# Load Balancing in System Design

## What is a Load Balancer?
A load balancer distributes incoming network traffic across multiple servers to ensure no single server becomes overwhelmed, improving responsiveness and availability.

## Types of Load Balancers

### 1. Layer 4 (Transport Layer)
- **Operation Level**: Works at the transport layer (TCP/UDP)
- **Decision Making**: Uses IP addresses and ports
- **Use Case**: Simple packet-level load balancing
- **Performance**: Very fast, low overhead
- **Limitations**: No content awareness

### 2. Layer 7 (Application Layer)
- **Operation Level**: Works at the application layer (HTTP/HTTPS)
- **Decision Making**: Can inspect message content (headers, URLs, cookies)
- **Features**:
  - Path-based routing
  - Host-based routing
  - SSL termination
  - Content caching
  - Sticky sessions
- **Performance**: Slightly slower due to deeper inspection

## Load Balancing Algorithms

### 1. Round Robin
- **How it works**: Distributes requests sequentially
- **Best for**: Homogeneous server environments
- **Drawback**: Doesn't consider server load

### 2. Least Connections
- **How it works**: Sends requests to the server with fewest active connections
- **Best for**: Variable request processing times

### 3. IP Hash
- **How it works**: Uses client IP to determine server
- **Benefit**: Consistent server assignment
- **Use case**: Session persistence

### 4. Least Response Time
- **How it works**: Combines fastest response time and fewest active connections
- **Best for**: Optimizing response times

### 5. Weighted Algorithms
- **How it works**: Assigns weights to servers based on capacity
- **Use case**: Heterogeneous server environments

## Load Balancer Features

### Health Checks
- **Active Checks**: Periodic requests to verify server health
- **Passive Checks**: Monitors server responses to real traffic
- **Graceful Degradation**: Removes unhealthy servers from rotation

### SSL Termination
- **Benefit**: Offloads CPU-intensive encryption/decryption from backend servers
- **Consideration**: Ensure secure internal communication

### Session Persistence (Sticky Sessions)
- **How it works**: Routes client to same server using cookies or IP
- **Use case**: Shopping carts, user sessions
- **Challenge**: Can create uneven load distribution

### Caching
- **Benefit**: Reduces load on backend servers
- **Implementation**: Can cache static content at the load balancer level

## Load Balancer Architectures

### 1. Single Load Balancer
```
[Client] → [Load Balancer] → [Server 1]
                      → [Server 2]
                      → [Server 3]
```
- **Risk**: Single point of failure
- **Use case**: Development or small-scale production

### 2. Active-Passive
- **How it works**: Primary handles traffic, secondary on standby
- **Benefit**: High availability
- **Drawback**: Underutilized resources

### 3. Active-Active
- **How it works**: Multiple load balancers share traffic
- **Benefit**: Maximum resource utilization
- **Requirement**: Synchronized state between load balancers

### 4. Global Server Load Balancing (GSLB)
- **How it works**: Distributes traffic across multiple data centers
- **Benefit**: Geographic load balancing and disaster recovery
- **Use case**: Global applications

## Common Load Balancer Implementations

### Hardware Load Balancers
- **Examples**: F5 BIG-IP, Citrix ADC
- **Pros**: High performance, dedicated hardware
- **Cons**: Expensive, less flexible

### Software Load Balancers
- **Examples**: Nginx, HAProxy, Envoy
- **Pros**: Cost-effective, flexible, cloud-friendly
- **Cons**: Slightly lower performance than hardware

### Cloud Provider Solutions
- **AWS**: Application Load Balancer (ALB), Network Load Balancer (NLB)
- **GCP**: Cloud Load Balancing
- **Azure**: Azure Load Balancer, Application Gateway

## Load Balancer vs Reverse Proxy

| Feature          | Load Balancer               | Reverse Proxy                |
|------------------|----------------------------|-----------------------------|
| Primary Purpose | Distribute traffic          | Front-end for backend servers|
| Layer           | L4 or L7                   | L7                          |
| Caching         | Limited or none            | Yes                         |
| SSL             | Termination                | Termination & Offloading    |
| Use Case        | High availability, scaling | Security, caching, SSL      |

## Best Practices

### 1. Redundancy
- Always deploy at least two load balancers
- Use different availability zones

### 2. Monitoring
- Track request rates, error rates, and response times
- Set up alerts for anomalies

### 3. Security
- Implement rate limiting
- Use Web Application Firewall (WAF)
- Regular security updates

### 4. Performance Tuning
- Tune timeouts based on application needs
- Configure appropriate keep-alive settings
- Monitor and adjust buffer sizes

## Common Pitfalls

### 1. Single Point of Failure
- **Solution**: Deploy multiple load balancers

### 2. Session State Management
- **Challenge**: Maintaining user sessions during failover
- **Solution**: Use distributed session stores or stateless designs

### 3. SSL/TLS Termination Overhead
- **Challenge**: CPU-intensive operations
- **Solution**: Use hardware acceleration or dedicated SSL offloading

### 4. Configuration Drift
- **Challenge**: Inconsistent configurations across environments
- **Solution**: Use infrastructure as code (IaC) tools
