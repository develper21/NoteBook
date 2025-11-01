# Databases in System Design

## Database Types

### 1. Relational Databases (RDBMS)
- **Examples**: PostgreSQL, MySQL, Oracle, SQL Server
- **Data Model**: Tables with rows and columns
- **Schema**: Fixed schema with relationships
- **ACID Properties**: Atomicity, Consistency, Isolation, Durability
- **Use Cases**: Complex queries, transactions, data integrity

### 2. NoSQL Databases

#### Document Stores
- **Examples**: MongoDB, CouchDB
- **Data Model**: JSON-like documents
- **Schema**: Schema-less or flexible schema
- **Use Cases**: Content management, user profiles

#### Key-Value Stores
- **Examples**: Redis, DynamoDB, Riak
- **Data Model**: Key-value pairs
- **Performance**: Very fast for simple lookups
- **Use Cases**: Caching, session storage

#### Wide-Column Stores
- **Examples**: Cassandra, HBase, ScyllaDB
- **Data Model**: Column families, rows with variable columns
- **Scalability**: Excellent horizontal scaling
- **Use Cases**: Time-series data, large datasets

#### Graph Databases
- **Examples**: Neo4j, Amazon Neptune
- **Data Model**: Nodes, edges, and properties
- **Strength**: Relationships between data points
- **Use Cases**: Social networks, recommendation engines

## Database Scaling

### Vertical Scaling (Scale Up)
- **What**: Increase server capacity (CPU, RAM, storage)
- **Pros**: Simple, no application changes needed
- **Cons**: Hardware limits, single point of failure
- **When to use**: Small to medium workloads

### Horizontal Scaling (Scale Out)
- **What**: Add more servers to distribute load
- **Pros**: Virtually unlimited scale, high availability
- **Cons**: Complex to implement, consistency challenges
- **When to use**: Large-scale applications

## Database Replication

### Master-Slave Replication
- **How it works**: One master (writes), multiple read replicas
- **Pros**: Read scalability, fault tolerance
- **Cons**: Replication lag, single point of failure for writes

### Multi-Master Replication
- **How it works**: Multiple masters can accept writes
- **Pros**: Write availability, geographic distribution
- **Cons**: Conflict resolution needed, complex to implement

## Database Partitioning

### Horizontal Partitioning (Sharding)
- **What**: Split table rows across multiple databases
- **Sharding Key**: Determines which shard stores the data
- **Challenges**: Cross-shard queries, rebalancing

### Vertical Partitioning
- **What**: Split table columns across different tables
- **Use Case**: When some columns are accessed more frequently than others

## CAP Theorem

### Consistency (C)
- All nodes see the same data at the same time

### Availability (A)
- Every request receives a response, even if some nodes are down

### Partition Tolerance (P)
- System continues to operate despite network partitions

### Trade-offs
- **CA**: Traditional SQL databases (PostgreSQL, MySQL)
- **CP**: MongoDB, HBase (consistency over availability)
- **AP**: Cassandra, DynamoDB (availability over consistency)

## Database Indexing

### B-Tree Index
- **Structure**: Balanced tree structure
- **Best for**: Range queries, equality comparisons
- **Drawbacks**: Slower for write operations

### Hash Index
- **Structure**: Key-value pairs using hash function
- **Best for**: Exact match lookups
- **Drawbacks**: Doesn't support range queries

### Bitmap Index
- **Structure**: Bit vectors
- **Best for**: Low-cardinality columns
- **Use Case**: Data warehousing

## Database Transactions

### ACID Properties
- **Atomicity**: All or nothing
- **Consistency**: Valid state to valid state
- **Isolation**: Concurrent transactions don't interfere
- **Durability**: Committed transactions persist

### Isolation Levels
1. **Read Uncommitted**
   - Lowest isolation level
   - Dirty reads possible

2. **Read Committed**
   - Default in many databases
   - Prevents dirty reads
   - Non-repeatable reads possible

3. **Repeatable Read**
   - Prevents non-repeatable reads
   - Phantom reads possible

4. **Serializable**
   - Highest isolation level
   - Complete isolation
   - Performance impact

## Database Optimization

### Query Optimization
- **Indexing**: Add appropriate indexes
- **Query Restructuring**: Rewrite complex queries
- **Partitioning**: Split large tables
- **Denormalization**: Reduce joins for read-heavy workloads

### Connection Pooling
- **What**: Cache of database connections
- **Benefits**: Reduces connection overhead
- **Implementation**: HikariCP, c3p0

### Caching Layers
- **Application-level caching**: Redis, Memcached
- **Database caching**: Query cache, buffer pool

## Database Migration

### Schema Migrations
- **Tools**: Flyway, Liquibase, Django Migrations
- **Best Practices**:
  - Version control all migrations
  - Make migrations reversible
  - Test migrations in staging

### Data Migration
- **ETL Processes**: Extract, Transform, Load
- **Zero-downtime Migrations**: Blue-green deployment

## Database Security

### Authentication & Authorization
- **Authentication**: Verify user identity
- **Authorization**: Control access to data

### Encryption
- **At Rest**: Encrypt data on disk
- **In Transit**: Use TLS/SSL

### SQL Injection Prevention
- **Use Prepared Statements**
- **Input Validation**
- **Least Privilege Principle**

## Time-Series Databases
- **Examples**: InfluxDB, TimescaleDB
- **Optimized For**: Time-stamped data
- **Use Cases**: IoT, monitoring, analytics

## NewSQL Databases
- **Examples**: Google Spanner, CockroachDB
- **Features**: ACID + horizontal scaling
- **Use Cases**: Global applications needing both consistency and scale

## Polyglot Persistence
- **Concept**: Using different databases for different data needs
- **Example**:
  - PostgreSQL for transactions
  - Redis for caching
  - Elasticsearch for search
  - Neo4j for relationships

## Database as a Service (DBaaS)
- **Examples**:
  - AWS: RDS, Aurora, DynamoDB
  - Google: Cloud SQL, Firestore
  - Azure: Azure SQL, Cosmos DB
- **Benefits**: Managed service, automatic backups, scaling
- **Considerations**: Vendor lock-in, cost at scale
