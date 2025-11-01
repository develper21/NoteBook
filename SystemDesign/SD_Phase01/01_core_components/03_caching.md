# Caching in System Design

## What is Caching?
Caching is a high-speed data storage layer that stores a subset of data, typically transient in nature, so that future requests for that data are served up faster than is possible by accessing the data's primary storage location.

## Why Use Caching?
- **Performance**: Drastically reduces data access latency
- **Throughput**: Increases system throughput
- **Cost Reduction**: Reduces load on primary data stores
- **Offline Capabilities**: Enables operation with intermittent connectivity

## Cache Levels

### 1. Client-Side Caching
- **Location**: Browser or mobile device
- **Examples**: Browser cache, mobile app cache
- **Use Case**: Static assets, API responses
- **Advantage**: Reduces network calls

### 2. CDN Caching
- **Location**: Edge servers distributed globally
- **Examples**: CloudFront, Cloudflare, Akamai
- **Use Case**: Static content, media files
- **TTL**: Typically hours to days

### 3. Web Server Caching
- **Location**: Reverse proxies and web servers
- **Examples**: Nginx, Varnish
- **Use Case**: HTML pages, API responses
- **Advantage**: Reduces application server load

### 4. Application Caching
- **Location**: Application memory or local cache
- **Examples**: In-memory caches (Guava, Caffeine)
- **Use Case**: Frequently accessed data
- **Consideration**: Cache invalidation strategy

### 5. Distributed Caching
- **Location**: External cache servers
- **Examples**: Redis, Memcached, Hazelcast
- **Use Case**: Shared cache across multiple instances
- **Advantage**: Horizontal scalability

## Caching Strategies

### 1. Cache-Aside (Lazy Loading)
```python
def get_data(key):
    # Try to get from cache first
    data = cache.get(key)
    if data is None:
        # Cache miss: get from database
        data = database.get(key)
        # Update cache for future requests
        cache.set(key, data, ttl=3600)
    return data
```
- **Pros**: Simple, resilient to cache failures
- **Cons**: Cache miss penalty, potential for stale data

### 2. Read-Through
- **How it works**: Cache sits in front of database
- **Pros**: Cleaner code, better for read-heavy workloads
- **Cons**: More complex cache implementation

### 3. Write-Through
```python
def save_data(key, value):
    # Write to cache and database
    cache.set(key, value)
    database.set(key, value)
```
- **Pros**: Data consistency, read-after-write consistency
- **Cons**: Higher write latency

### 4. Write-Behind (Write-Back)
- **How it works**: Write to cache first, then asynchronously to database
- **Pros**: Improved write performance
- **Cons**: Risk of data loss on cache failure

### 5. Refresh-Ahead
- **How it works**: Proactively refresh cache before expiration
- **Pros**: Better user experience
- **Cons**: Wasted resources if data isn't accessed

## Cache Eviction Policies

### 1. LRU (Least Recently Used)
- **How it works**: Evicts the least recently accessed items first
- **Use Case**: General purpose
- **Implementation**: Hash table + doubly linked list

### 2. LFU (Least Frequently Used)
- **How it works**: Evicts the least frequently accessed items
- **Use Case**: Stable access patterns
- **Drawback**: Doesn't adapt well to changing access patterns

### 3. FIFO (First In, First Out)
- **How it works**: Evicts the oldest items first
- **Use Case**: Simple scenarios
- **Drawback**: Poor performance in many real-world scenarios

### 4. Random Replacement
- **How it works**: Randomly selects items to evict
- **Use Case**: Simple implementations
- **Drawback**: Unpredictable performance

## Cache Invalidation

### 1. Time-based (TTL)
- **How it works**: Data expires after set time
- **Pros**: Simple to implement
- **Cons**: May serve stale data until expiration

### 2. Write-Through Invalidation
- **How it works**: Update cache when data is written
- **Pros**: Strong consistency
- **Cons**: Higher write latency

### 3. Cache Invalidation on Update
- **How it works**: Explicitly invalidate cache on data change
- **Pros**: Fine-grained control
- **Cons**: Complex to implement correctly

## Distributed Caching Patterns

### 1. Cache Sharding
- **How it works**: Distribute cache across multiple nodes
- **Benefits**: Horizontal scaling, fault tolerance
- **Challenges**: Data distribution, rebalancing

### 2. Cache Replication
- **How it works**: Maintain multiple copies of cache
- **Benefits**: High availability, read scalability
- **Challenges**: Cache coherence, write amplification

### 3. Cache-Aside with Write-Behind
- **How it works**: Combine read-through with async writes
- **Benefits**: Good read/write performance
- **Challenges**: Eventual consistency

## Cache Coherence

### 1. Write-Invalidate
- **How it works**: Invalidate cache on write
- **Pros**: Simplicity
- **Cons**: Thundering herd on cache miss

### 2. Write-Update
- **How it works**: Update all cache copies on write
- **Pros**: No cache misses
- **Cons**: Higher write overhead

## Cache-Aside Pattern in Detail

### Read Path
1. Application receives read request for key X
2. Check cache for X
3. If found (cache hit), return X
4. If not found (cache miss):
   - Read X from database
   - Store X in cache
   - Return X

### Write Path
1. Application receives write request for key X
2. Update database with new value for X
3. Invalidate/update cache for X

## Common Pitfalls

### 1. Cache Stampede (Thundering Herd)
- **Scenario**: Many requests for the same key miss cache simultaneously
- **Solution**: Use locks or lease mechanisms

### 2. Cache Penetration
- **Scenario**: Requests for non-existent keys bypass cache
- **Solution**: Cache negative results

### 3. Cache Avalanche
- **Scenario**: Multiple cache entries expire simultaneously
- **Solution**: Add jitter to TTLs

## Real-world Cache Implementations

### 1. Redis
- **Type**: In-memory data structure store
- **Features**: Persistence, replication, transactions
- **Use Cases**: Session store, rate limiting, pub/sub

### 2. Memcached
- **Type**: High-performance distributed memory cache
- **Features**: Simple key-value store, multi-threaded
- **Use Cases**: Simple caching needs, session storage

### 3. CDN Caching
- **Examples**: CloudFront, Cloudflare, Fastly
- **Features**: Global distribution, DDoS protection
- **Use Cases**: Static assets, API responses

## Cache Monitoring and Metrics

### Key Metrics to Monitor
- **Hit Ratio**: Hits / (Hits + Misses)
- **Latency**: Read/Write times
- **Eviction Rate**: How often items are evicted
- **Memory Usage**: Current cache size

### Alerting
- Set up alerts for:
  - Low hit ratio
  - High eviction rate
  - Memory pressure

## Advanced Topics

### 1. Cache Warming
- **What**: Pre-loading cache with data before it's needed
- **When**: After deployment, after cache eviction

### 2. Cache Coherence Protocols
- **MSI**: Modified, Shared, Invalid
- **MESI**: Modified, Exclusive, Shared, Invalid

### 3. Cache-Oblivious Algorithms
- **Concept**: Algorithms that are efficient regardless of cache size
- **Example**: Blocked matrix multiplication
