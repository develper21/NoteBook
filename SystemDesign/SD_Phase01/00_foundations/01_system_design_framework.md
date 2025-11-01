# System Design Framework

A structured, step-by-step approach for designing large-scale systems.

## 1. Clarify Requirements

### Functional Requirements
- List of features the system must perform
- Example: "Users should be able to upload and share photos"

### Non-Functional Requirements (NFRs)
- Scalability: Handle growth in users/data/traffic
- Availability: Uptime requirements (e.g., 99.99%)
- Reliability: Consistency and fault tolerance
- Performance: Response time and throughput
- Maintainability: Ease of updates and debugging

### Out of Scope
- Explicitly state what the system won't do
- Example: "User authentication is out of scope for initial version"

## 2. Back-of-the-Envelope Estimation

### Key Metrics
- **QPS (Queries Per Second)**: Read and write operations
- **Storage Requirements**: Data size over time
- **Bandwidth**: Data transfer needs
- **Memory**: Caching requirements (80/20 rule)

### Estimation Techniques
- Use powers of 10 for quick calculations
- Apply the 80/20 rule for caching
- Consider growth over time (5-10 years)

## 3. High-Level Design (HLD)
- System architecture diagram
- Main components and their interactions
- Data flow through the system
- Initial technology choices

## 4. Deep Dive
- Focus on 2-3 critical components
- Detailed design of key algorithms
- Data models and schemas
- API contracts

## 5. Trade-offs and Justifications
- Document design decisions
- Considered alternatives
- Pros and cons of chosen approach
- Potential bottlenecks and mitigation strategies

## 6. Scaling and Optimization
- Horizontal vs vertical scaling
- Caching strategies
- Database optimizations
- Performance tuning

## 7. Monitoring and Maintenance
- Key metrics to monitor
- Alerting strategy
- Disaster recovery plan
- Maintenance procedures
