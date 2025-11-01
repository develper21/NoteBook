# Estimation Cheatsheet

## Latency Numbers Every Engineer Should Know

### Computer Latency
```
L1 cache reference: 0.5 ns
Branch mispredict: 5 ns
L2 cache reference: 7 ns
Mutex lock/unlock: 25 ns
Main memory reference: 100 ns
Compress 1KB with Zippy: 10,000 ns (10 µs)
Send 2K bytes over 1 Gbps network: 44 ns
Read 1 MB sequentially from memory: 3,000 ns (3 µs)
SSD random read: 16,000 ns (16 µs)
Read 1 MB sequentially from SSD: 49,000 ns (49 µs)
Round trip within same datacenter: 500,000 ns (500 µs)
Disk seek: 4,000,000 ns (4 ms)
Read 1 MB sequentially from disk: 825,000 ns (0.8 ms)
Send packet CA→Netherlands→CA: 150,000,000 ns (150 ms)
```

### Storage Requirements
```
1 ASCII character: 1 byte
1 UTF-8 character: 1-4 bytes
1 UTF-16 character: 2 or 4 bytes
32-bit integer: 4 bytes
64-bit integer: 8 bytes
Double-precision float: 8 bytes
Boolean: 1 byte
UUID: 16 bytes
MD5 hash: 16 bytes
SHA-1 hash: 20 bytes
SHA-256 hash: 32 bytes
```

### Common Data Sizes
```
Short email (text only): 10 KB
Medium email (with attachments): 300 KB
Web page: 2 MB
Average photo: 3 MB
High-res photo: 10 MB
1 minute of music (MP3): 1 MB
1 minute of video (480p): 10 MB
1 minute of video (1080p): 100 MB
1 minute of video (4K): 1 GB
```

### Throughput
```
1 Gbps network: 125 MB/s
10 Gbps network: 1.25 GB/s
PCIe 3.0 x16: 16 GB/s
NVMe SSD: 3.5 GB/s
SATA SSD: 550 MB/s
HDD: 100-200 MB/s
```

### Powers of 2 and 10
```
2^10 = 1,024 (Kilo)
2^20 = 1,048,576 (Mega)
2^30 = 1,073,741,824 (Giga)
2^40 = 1,099,511,627,776 (Tera)
2^50 = 1,125,899,906,842,624 (Peta)
2^60 = 1,152,921,504,606,846,976 (Exa)
```

## Estimation Examples

### Example 1: Photo Storage
- 10 million users
- Each user uploads 100 photos
- Average photo size: 3 MB
- Total storage = 10M * 100 * 3MB = 3,000 TB = 3 PB

### Example 2: Social Media Posts
- 1 million daily active users
- Each user creates 5 posts per day
- Average post size: 1 KB
- Daily storage growth = 1M * 5 * 1KB = 5 GB/day
- Annual storage = 5 GB * 365 = ~1.8 TB/year

### Example 3: Video Streaming
- 100,000 concurrent users
- Each stream: 5 Mbps
- Total bandwidth = 100,000 * 5 Mbps = 500 Gbps
- Monthly data transfer = 500 Gbps * 30 days = 162 PB/month
