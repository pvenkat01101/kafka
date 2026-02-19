# Kafka Interview Preparation Topics
## Complete Roadmap: Basics to In-Depth for FAANG Interviews

---

## 1. Kafka Fundamentals

### 1.1 Core Concepts
- What is Apache Kafka and its use cases
- Event streaming and publish-subscribe pattern
- Kafka vs traditional messaging systems (RabbitMQ, ActiveMQ)
- Kafka vs other streaming platforms (Kinesis, Pulsar)
- Distributed commit log architecture
- Pull-based vs push-based messaging

### 1.2 Kafka Architecture Components
- **Broker**: Role and responsibilities
- **Topic**: Logical organization of data
- **Partition**: Physical distribution of data
- **Offset**: Message position tracking
- **Producer**: Message publishing
- **Consumer**: Message consumption
- **Consumer Group**: Parallel consumption model
- **ZooKeeper**: Coordination and metadata (legacy)
- **KRaft**: ZooKeeper-less Kafka (new mode)

### 1.3 Message Structure
- Key-value pairs
- Headers
- Timestamp
- Offset
- Partition number
- Serialization formats (JSON, Avro, Protobuf, String)

---

## 2. Topics and Partitions

### 2.1 Topic Design
- Naming conventions and best practices
- Topic creation and configuration
- Retention policies (time-based, size-based)
- Cleanup policies (delete vs compact)
- Replication factor considerations
- Partition count selection criteria

### 2.2 Partitioning Strategy
- Default partitioner behavior
- Custom partitioning logic
- Key-based partitioning
- Round-robin partitioning
- Partition assignment strategies
- Hot partition problem and solutions
- Partition rebalancing

### 2.3 Log Compaction
- Compacted topics vs deleted topics
- Use cases for log compaction
- Tombstone records (null values)
- Compaction process and guarantees
- min.cleanable.dirty.ratio
- segment.ms configuration

---

## 3. Producers

### 3.1 Producer Basics
- Producer API and configuration
- Message sending patterns (fire-and-forget, sync, async)
- Callbacks and error handling
- Batching and buffering
- Compression (gzip, snappy, lz4, zstd)

### 3.2 Producer Guarantees
- **Idempotence**: enable.idempotence=true
- **Transactions**: Exactly-once semantics (EOS)
- **Acknowledgments (acks)**:
  - acks=0: Fire and forget
  - acks=1: Leader acknowledgment
  - acks=all/-1: All in-sync replicas
- Retries and retry backoff
- max.in.flight.requests.per.connection

### 3.3 Producer Performance Tuning
- Batch size optimization (batch.size)
- Linger time (linger.ms)
- Buffer memory (buffer.memory)
- Compression trade-offs
- Partitioner performance
- Producer metrics to monitor

### 3.4 Advanced Producer Concepts
- Transactional producer
- Idempotent producer internals
- Producer interceptors
- Custom serializers
- Schema registry integration

---

## 4. Consumers

### 4.1 Consumer Basics
- Consumer API and configuration
- Poll loop pattern
- Offset management
- Auto-commit vs manual commit
- Commit strategies (sync vs async)
- seek() and seekToBeginning/End()

### 4.2 Consumer Groups
- Consumer group coordination
- Group coordinator and group leader
- Partition assignment strategies:
  - Range assignor
  - Round-robin assignor
  - Sticky assignor
  - Cooperative sticky assignor
- Rebalance process and triggers
- Static membership (group.instance.id)

### 4.3 Offset Management
- Auto offset reset (earliest, latest, none)
- Committing offsets
- Offset storage (__consumer_offsets topic)
- External offset storage
- Exactly-once consumption patterns

### 4.4 Consumer Performance Tuning
- fetch.min.bytes and fetch.max.wait.ms
- max.poll.records
- max.poll.interval.ms
- session.timeout.ms vs heartbeat.interval.ms
- Consumer lag monitoring
- Parallel consumption strategies

### 4.5 Advanced Consumer Concepts
- Pause and resume consumption
- Consumer interceptors
- Custom deserializers
- Handling poison pills
- Dead letter queues (DLQ)
- Retry topics pattern

---

## 5. Replication and High Availability

### 5.1 Replication Fundamentals
- Leader and follower replicas
- In-sync replicas (ISR)
- Replication factor best practices
- Leader election process
- Preferred leader election
- Unclean leader election

### 5.2 Replication Configuration
- min.insync.replicas
- unclean.leader.election.enable
- replica.lag.time.max.ms
- Replication throttling
- Replica fetcher threads

### 5.3 Durability Guarantees
- Durability vs availability trade-offs
- Quorum-based replication
- Handling broker failures
- Data loss scenarios
- Message ordering guarantees

### 5.4 Multi-Datacenter Replication
- MirrorMaker 2.0
- Active-passive setup
- Active-active setup
- Disaster recovery strategies
- Cross-cluster replication patterns

---

## 6. Kafka Streams

### 6.1 Stream Processing Basics
- Stateless transformations (map, filter, flatMap)
- Stateful transformations (aggregate, reduce, join)
- KStream vs KTable vs GlobalKTable
- Windowing operations
- Stream-table joins
- Stream-stream joins

### 6.2 State Stores
- In-memory vs persistent stores
- RocksDB state backend
- State store types (key-value, window, session)
- Changelog topics
- Standby replicas
- State store restoration

### 6.3 Time Semantics
- Event time vs processing time
- Timestamp extractors
- Out-of-order data handling
- Grace periods
- Windowing strategies (tumbling, hopping, sliding, session)

### 6.4 Kafka Streams Architecture
- Stream partitioning and parallelism
- Task assignment
- Rebalancing in Streams
- Exactly-once processing semantics
- Interactive queries
- Topology optimization

### 6.5 Advanced Kafka Streams
- Custom processors and transformers
- Punctuators for periodic processing
- Global state stores
- Stream branching and merging
- Error handling and deserialization
- Testing Kafka Streams applications

---

## 7. Kafka Connect

### 7.1 Connect Fundamentals
- Source connectors
- Sink connectors
- Standalone vs distributed mode
- Connector plugins and tasks
- Connector configuration
- Converters and transformations

### 7.2 Common Connectors
- JDBC source and sink connectors
- Elasticsearch sink connector
- S3 sink connector
- Debezium CDC connectors
- MongoDB connector
- Cloud storage connectors

### 7.3 Connect Architecture
- Worker nodes
- Task distribution
- Offset management
- Error handling and DLQ
- Exactly-once support in Connect
- REST API for management

### 7.4 Custom Connectors
- Developing source connectors
- Developing sink connectors
- Connector lifecycle
- Single Message Transforms (SMTs)
- Testing connectors

---

## 8. Schema Management

### 8.1 Schema Registry
- Purpose and benefits
- Schema evolution rules
- Compatibility types (backward, forward, full, none)
- Subject naming strategies
- Schema versioning
- Avro, Protobuf, JSON Schema support

### 8.2 Serialization Formats
- Avro advantages and use cases
- Protocol Buffers (Protobuf)
- JSON Schema
- Custom serializers
- Performance comparisons
- Schema-on-read vs schema-on-write

### 8.3 Schema Evolution
- Adding optional fields
- Removing fields
- Changing field types
- Default values
- Breaking vs non-breaking changes
- Migration strategies

---

## 9. Performance and Scalability

### 9.1 Kafka Performance Optimization
- OS-level tuning (page cache, file descriptors)
- JVM tuning for brokers
- Network optimization
- Disk I/O optimization
- Compression selection
- Batch size tuning

### 9.2 Scaling Kafka Clusters
- Adding brokers
- Partition reassignment
- Increasing partition count
- Broker decommissioning
- Rack awareness
- Load balancing strategies

### 9.3 Throughput Optimization
- Producer throughput tuning
- Consumer throughput tuning
- End-to-end latency optimization
- Zero-copy optimization
- Record batching strategies

### 9.4 Capacity Planning
- Storage capacity calculation
- Network bandwidth requirements
- CPU and memory sizing
- Retention policy impact
- Replication overhead
- Growth projections

---

## 10. Monitoring and Operations

### 10.1 Key Metrics to Monitor
- **Broker metrics**: Under-replicated partitions, offline partitions, active controller
- **Producer metrics**: Record send rate, error rate, batch size
- **Consumer metrics**: Lag, fetch rate, commit rate
- **JVM metrics**: Heap usage, GC pauses
- **OS metrics**: Disk I/O, network I/O, CPU usage

### 10.2 Monitoring Tools
- JMX metrics
- Prometheus and Grafana
- Confluent Control Center
- Kafka Manager / CMAK
- Burrow for consumer lag
- LinkedIn's Cruise Control

### 10.3 Common Operational Issues
- Under-replicated partitions
- Consumer lag
- Rebalancing storms
- Broker out of memory
- Disk space issues
- Network saturation
- ZooKeeper/KRaft issues

### 10.4 Maintenance and Upgrades
- Rolling broker upgrades
- Kafka version compatibility
- Protocol version management
- Downgrading considerations
- Data migration strategies
- Backup and recovery

---

## 11. Security

### 11.1 Authentication
- SASL mechanisms (PLAIN, SCRAM, GSSAPI/Kerberos, OAUTHBEARER)
- SSL/TLS for encryption
- Mutual TLS (mTLS)
- Delegation tokens
- JAAS configuration

### 11.2 Authorization
- ACLs (Access Control Lists)
- Principal types
- Resource types (topics, consumer groups, clusters)
- Operation types (read, write, describe)
- Authorizer implementations
- Role-based access control

### 11.3 Encryption
- In-transit encryption (SSL/TLS)
- At-rest encryption
- End-to-end encryption
- Key management
- Performance impact of encryption

### 11.4 Security Best Practices
- Least privilege principle
- Network segmentation
- Audit logging
- Security monitoring
- Vulnerability management
- Compliance requirements (GDPR, HIPAA)

---

## 12. Advanced Topics

### 12.1 Exactly-Once Semantics (EOS)
- Idempotent producer internals
- Transactional API
- Transaction coordinator
- Transaction log
- Isolation levels (read_uncommitted, read_committed)
- EOS performance implications

### 12.2 Kafka Internals
- Log segment structure
- Index files (.index, .timeindex)
- Log cleaning process
- Controller responsibilities
- Group coordinator internals
- Request handling pipeline

### 12.3 Advanced Patterns
- Event sourcing with Kafka
- CQRS (Command Query Responsibility Segregation)
- Change Data Capture (CDC)
- Outbox pattern
- Saga pattern for distributed transactions
- Event-driven microservices

### 12.4 Kafka and Big Data Ecosystem
- Integration with Spark
- Integration with Flink
- Integration with Hadoop/HDFS
- Lambda architecture
- Kappa architecture
- Real-time analytics pipelines

---

## 13. Troubleshooting and Debugging

### 13.1 Common Producer Issues
- Messages not being sent
- Timeout exceptions
- Serialization errors
- Buffer full exceptions
- Performance degradation

### 13.2 Common Consumer Issues
- Consumer lag
- Rebalancing issues
- Duplicate consumption
- Message loss
- Offset commit failures
- Deserialization errors

### 13.3 Cluster Issues
- Broker failures
- Network partitions
- Split-brain scenarios
- Controller election failures
- Disk failures
- ZooKeeper/KRaft issues

### 13.4 Debugging Tools and Techniques
- kafka-console-producer/consumer
- kafka-topics.sh
- kafka-consumer-groups.sh
- kafka-configs.sh
- Log analysis
- Thread dumps and heap dumps
- Network packet analysis

---

## 14. Best Practices and Design Patterns

### 14.1 Topic Design Best Practices
- Single responsibility per topic
- Appropriate partition count
- Proper replication factor
- Retention policy selection
- Naming conventions
- Schema versioning strategy

### 14.2 Producer Best Practices
- Enable idempotence
- Use appropriate acks setting
- Implement proper error handling
- Use compression
- Batch optimization
- Key selection for partitioning

### 14.3 Consumer Best Practices
- Use consumer groups for scalability
- Handle rebalances gracefully
- Commit offsets appropriately
- Monitor consumer lag
- Implement retry and DLQ patterns
- Handle poison pills

### 14.4 Operational Best Practices
- Regular monitoring and alerting
- Capacity planning
- Disaster recovery planning
- Security hardening
- Documentation
- Testing strategies (unit, integration, load)

---

## 15. Real-World Scenarios and Case Studies

### 15.1 Use Cases
- Activity tracking and logging
- Metrics and monitoring
- Stream processing pipelines
- Event-driven microservices
- Log aggregation
- Change Data Capture (CDC)
- IoT data ingestion

### 15.2 FAANG-Level Scenarios
- Designing a real-time analytics system
- Building a distributed transaction system
- Implementing exactly-once processing
- Handling millions of messages per second
- Multi-region Kafka deployment
- Migrating from legacy messaging systems
- Handling schema evolution in production

### 15.3 Trade-offs and Decisions
- Throughput vs latency
- Availability vs consistency
- Storage costs vs retention
- Replication factor selection
- Partition count selection
- When to use Kafka vs alternatives

---

## Study Plan Recommendations

### Week 1-2: Fundamentals
- Core concepts and architecture
- Producers and consumers
- Topics and partitions

### Week 3-4: Intermediate
- Replication and high availability
- Performance tuning
- Monitoring basics

### Week 5-6: Advanced
- Kafka Streams
- Kafka Connect
- Schema management
- Security

### Week 7-8: Expert Level
- Exactly-once semantics
- Kafka internals
- Advanced patterns
- Real-world scenarios
- Practice interview questions

---

## Key Resources
- Apache Kafka Documentation
- Confluent Documentation
- "Kafka: The Definitive Guide" book
- Martin Kleppmann's "Designing Data-Intensive Applications"
- LinkedIn Engineering Blog
- Confluent Blog
- Kafka Improvement Proposals (KIPs)
