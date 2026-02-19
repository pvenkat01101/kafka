# Kafka Interview Questions
## Beginner to FAANG Level

---

## Level 1: Beginner Questions (0-2 Years Experience)

### Basic Concepts

**Q1: What is Apache Kafka and what are its primary use cases?**
- Distributed event streaming platform
- Use cases: real-time analytics, log aggregation, event-driven microservices, metrics collection

**Q2: Explain the key components of Kafka architecture.**
- Broker: Kafka server that stores and serves data
- Producer: Publishes messages to topics
- Consumer: Reads messages from topics
- Topic: Category/feed name to which records are published
- Partition: Ordered, immutable sequence of records
- ZooKeeper/KRaft: Manages cluster metadata and coordination

**Q3: What is a Kafka topic?**
- Logical channel or category for organizing messages
- Similar to a database table or folder
- Topics are multi-subscriber; can have zero, one, or many consumers

**Q4: What is a partition in Kafka?**
- A topic is divided into partitions for parallelism
- Each partition is ordered and immutable
- Messages in a partition have a unique offset
- Enables horizontal scalability

**Q5: What is an offset in Kafka?**
- Unique identifier for each message within a partition
- Sequential integer that increments for each new message
- Consumers track their position using offsets

**Q6: What is the difference between a Kafka producer and consumer?**
- Producer: Writes/publishes messages to topics
- Consumer: Reads/subscribes to messages from topics
- Decoupled architecture allows independent scaling

**Q7: What is a consumer group?**
- Group of consumers working together to consume a topic
- Each partition is consumed by exactly one consumer in the group
- Enables parallel processing and load balancing

**Q8: How does Kafka ensure message ordering?**
- Ordering guaranteed only within a partition, not across partitions
- Messages with the same key go to the same partition
- Consumers read messages in order from each partition

**Q9: What are the different acknowledgment modes in Kafka?**
- acks=0: Producer doesn't wait for acknowledgment (fire-and-forget)
- acks=1: Wait for leader acknowledgment only
- acks=all or -1: Wait for all in-sync replicas

**Q10: What is replication in Kafka?**
- Creating copies of partitions across multiple brokers
- Provides fault tolerance and high availability
- Replication factor determines number of copies

---

## Level 2: Intermediate Questions (2-5 Years Experience)

### Architecture and Design

**Q11: Explain the concept of leader and follower replicas.**
- Leader: Handles all read/write requests for a partition
- Followers: Replicate data from leader, can take over if leader fails
- Only one leader per partition at any time
- ISR (In-Sync Replicas): Followers that are caught up with leader

**Q12: What is log compaction and when would you use it?**
- Retains at least the last known value for each message key
- Useful for maintaining state, like latest user profile
- cleanup.policy=compact
- Different from delete policy which removes old messages based on time/size

**Q13: How do you ensure exactly-once semantics in Kafka?**
- Enable idempotent producer: enable.idempotence=true
- Use transactions for multi-partition writes
- Consumer: read_committed isolation level
- Requires acks=all and retries > 0

**Q14: What is the purpose of ZooKeeper in Kafka?**
- Stores cluster metadata (brokers, topics, partitions)
- Manages broker membership and leader election
- Coordinates distributed configuration
- Note: Being replaced by KRaft (Kafka Raft) in newer versions

**Q15: Explain different partition assignment strategies in consumer groups.**
- Range: Assigns contiguous partitions per topic
- Round-robin: Distributes partitions evenly in circular fashion
- Sticky: Minimizes partition movement during rebalance
- Cooperative sticky: Allows incremental rebalancing without stop-the-world

**Q16: What happens when a consumer in a consumer group fails?**
- Rebalance is triggered
- Failed consumer's partitions are reassigned to remaining consumers
- Group coordinator manages the rebalancing process
- New consumer joins trigger rebalance too

**Q17: How does Kafka achieve high throughput?**
- Sequential disk I/O (append-only log)
- Zero-copy data transfer
- Batch compression
- Efficient binary protocol
- Partition parallelism
- Page cache utilization

**Q18: What is the difference between KStream and KTable in Kafka Streams?**
- KStream: Immutable stream of records (event stream)
- KTable: Changelog stream, represents current state (like database table)
- KStream processes each record individually
- KTable maintains latest value per key

**Q19: Explain the producer buffer and batch configuration.**
- buffer.memory: Total memory for buffering records
- batch.size: Batch size in bytes before sending
- linger.ms: Time to wait before sending batch
- Larger batches improve throughput but increase latency

**Q20: What is consumer lag and why is it important?**
- Difference between latest offset and consumer's current offset
- Indicates how far behind the consumer is
- High lag suggests consumer can't keep up with production rate
- Monitor using kafka-consumer-groups or monitoring tools

---

## Level 3: Advanced Questions (5-10 Years Experience)

### Performance and Optimization

**Q21: How would you tune Kafka for high throughput vs low latency?**

**High Throughput:**
- Increase batch.size and linger.ms
- Enable compression (lz4, snappy)
- Larger buffer.memory
- More partitions for parallelism
- acks=1 instead of all

**Low Latency:**
- Small batch.size and linger.ms=0
- No compression or fast compression
- acks=1
- Fewer partitions to reduce overhead
- Optimize network and disk I/O

**Q22: Explain the Kafka request processing pipeline.**
1. Client sends request to broker
2. Request queued in request queue
3. I/O thread picks up request
4. Request processed (log append, fetch, etc.)
5. Response placed in response queue
6. I/O thread sends response to client

**Q23: How does Kafka handle broker failures?**
- Controller detects failure via ZooKeeper/KRaft
- New leader elected from ISR for affected partitions
- Producers/consumers automatically connect to new leader
- If no ISR available, unclean leader election (if enabled)
- Data durability depends on replication factor and min.insync.replicas

**Q24: What is the producer idempotence and how does it work internally?**
- Prevents duplicate messages during retries
- Producer ID (PID) and sequence number assigned
- Broker detects duplicates using PID + sequence number
- Works within single session (PID changes on restart)
- Foundation for exactly-once semantics

**Q25: Explain Kafka transactions and their guarantees.**
- Atomic writes across multiple partitions/topics
- Begin transaction → Write messages → Commit/Abort
- Uses transaction coordinator and transaction log
- Two-phase commit protocol
- Consumers with read_committed see only committed messages
- Enables exactly-once processing

**Q26: How would you design a multi-datacenter Kafka deployment?**

**Approaches:**
- Active-Passive: MirrorMaker 2.0 for replication, failover on disaster
- Active-Active: Bidirectional replication, conflict resolution needed
- Stretched cluster: Not recommended, high latency
- Aggregate-and-forward: Local clusters, aggregate to central

**Considerations:**
- Network latency and bandwidth
- Data sovereignty and compliance
- Consistency vs availability trade-offs
- Replication lag monitoring

**Q27: What are the implications of increasing partition count?**

**Benefits:**
- Higher parallelism for producers/consumers
- Better load distribution
- Increased throughput

**Drawbacks:**
- More open file handles on brokers
- Longer leader election time
- Higher memory overhead
- Increased end-to-end latency
- More complexity in rebalancing

**Q28: How do you handle schema evolution in Kafka?**
- Use Schema Registry (Confluent)
- Choose compatibility mode: backward, forward, full, none
- Avro with default values for new fields
- Version schemas appropriately
- Test schema changes before production
- Plan migration for breaking changes

**Q29: Explain the difference between at-least-once, at-most-once, and exactly-once delivery.**

**At-most-once:**
- Messages may be lost but never duplicated
- acks=0, no retries
- Fastest but least reliable

**At-least-once:**
- No message loss but possible duplicates
- acks=all with retries
- Most common default

**Exactly-once:**
- No loss, no duplicates
- Idempotent producer + transactions
- Requires careful configuration

**Q30: How does Kafka's zero-copy optimization work?**
- Data transferred directly from page cache to network socket
- Bypasses user space, reducing CPU cycles
- Uses sendfile() system call
- Significantly improves throughput for consumers
- Works for fetching messages from disk to network

---

## Level 4: Expert/FAANG Level Questions (10+ Years Experience)

### System Design and Architecture

**Q31: Design a real-time fraud detection system using Kafka.**

**Requirements:**
- Process millions of transactions per second
- Detect fraudulent patterns in real-time
- Low latency (<100ms)
- High availability

**Architecture:**
1. **Ingestion Layer:**
   - Kafka topics for transaction events
   - Partition by user_id or account_id
   - Replication factor 3, min.insync.replicas=2

2. **Processing Layer:**
   - Kafka Streams for stateful processing
   - Windowed aggregations (5-minute tumbling windows)
   - Join transaction stream with user profile KTable
   - Rules engine for pattern detection

3. **Enrichment:**
   - GlobalKTable for reference data (merchant info, geo data)
   - External API calls for risk scoring

4. **Output:**
   - Fraud alerts topic
   - Legitimate transactions topic
   - Dead letter queue for errors

5. **Storage:**
   - State stores with RocksDB
   - Changelog topics for recovery
   - Standby replicas for fast failover

**Optimizations:**
- Interactive queries for real-time lookups
- Partition count based on throughput requirements
- Compression (lz4) for network efficiency
- Monitoring: lag, processing time, error rates

**Q32: How would you migrate from a legacy messaging system to Kafka with zero downtime?**

**Strategy:**
1. **Dual Write Phase:**
   - Producers write to both old and new systems
   - Implement at application level or using adapter

2. **Consumer Migration:**
   - Run consumers on both systems in parallel
   - Use feature flags for gradual rollout
   - Compare outputs for validation

3. **Monitoring:**
   - Track message counts, latency, errors
   - Set up dashboards for both systems
   - Alert on discrepancies

4. **Cutover:**
   - Stop writing to old system
   - Continue reading until drained
   - Monitor Kafka consumer lag
   - Decommission old system

5. **Rollback Plan:**
   - Keep old system running for rollback window
   - Ability to switch traffic back quickly
   - Data reconciliation process

**Challenges:**
- Message ordering during dual write
- Handling duplicates
- Schema differences
- Performance impact of dual write

**Q33: Explain how you would implement exactly-once processing in a multi-stage Kafka Streams application.**

**Implementation:**
```
Source Topic → Processing Stage 1 → Intermediate Topic → 
Processing Stage 2 → Sink Topic
```

**Configuration:**
```java
props.put(StreamsConfig.PROCESSING_GUARANTEE_CONFIG, 
          StreamsConfig.EXACTLY_ONCE_V2);
props.put(StreamsConfig.COMMIT_INTERVAL_MS_CONFIG, 100);
```

**How it works:**
1. **Idempotent Producers:**
   - Each stage uses idempotent producer
   - Prevents duplicates during retries

2. **Transactional Writes:**
   - Offsets and output messages written in same transaction
   - Atomic commit of consumption and production

3. **Isolation Level:**
   - Downstream consumers use read_committed
   - Only see committed messages

4. **State Store Updates:**
   - Included in transaction boundary
   - Consistent with offset commits

**Challenges:**
- Performance overhead (lower throughput)
- Increased latency due to transactions
- Broker configuration requirements
- Rebalancing handling

**Q34: Design a system to handle 10 million events per second with Kafka.**

**Requirements Analysis:**
- 10M events/sec = 600M events/min
- Assume average message size: 1KB
- Throughput: ~10 GB/sec

**Architecture:**

1. **Cluster Sizing:**
   - Brokers: 50-100 brokers (assuming 100-200 MB/sec per broker)
   - Partition count: 500-1000 partitions per topic
   - Replication factor: 3
   - ISR: min.insync.replicas=2

2. **Producer Configuration:**
   - Multiple producer instances (hundreds)
   - batch.size=64KB-128KB
   - linger.ms=5-10ms
   - compression=lz4 (3-5x compression)
   - acks=1 for high throughput (or acks=all with idempotence)
   - buffer.memory=512MB-1GB

3. **Consumer Configuration:**
   - Consumer groups with 500-1000 consumers
   - One consumer per partition maximum
   - fetch.min.bytes=1MB
   - max.poll.records=5000
   - Parallel processing threads

4. **Infrastructure:**
   - NVMe SSDs for low latency
   - 10 Gbps network cards
   - 256GB+ RAM per broker
   - 32+ CPU cores per broker

5. **Optimizations:**
   - OS tuning: file descriptors, vm.swappiness=1
   - JVM: G1GC, heap size 6-8GB
   - Page cache optimization
   - Dedicated ZooKeeper/KRaft cluster

6. **Monitoring:**
   - Prometheus + Grafana
   - Metrics: throughput, latency (p50, p99), error rates
   - Consumer lag monitoring
   - Broker resource utilization

**Q35: How would you debug and resolve a consumer lag issue in production?**

**Investigation Steps:**

1. **Identify the Problem:**
   ```bash
   kafka-consumer-groups --bootstrap-server localhost:9092 \
     --group my-group --describe
   ```
   - Check lag per partition
   - Identify problematic consumers

2. **Analyze Metrics:**
   - Consumer fetch rate and throughput
   - Processing time per message
   - GC pauses and CPU usage
   - Network I/O

3. **Common Causes and Solutions:**

   **Slow Processing:**
   - Profile application code
   - Optimize database queries
   - Add caching layer
   - Increase parallelism

   **Insufficient Consumers:**
   - Add more consumers to group
   - Ensure partition count >= consumer count
   - Check max.poll.records

   **Network Issues:**
   - Check fetch.min.bytes and fetch.max.wait.ms
   - Network latency between consumers and brokers
   - Bandwidth saturation

   **Rebalancing:**
   - Check session.timeout.ms and max.poll.interval.ms
   - Look for frequent rebalances in logs
   - Use static membership

   **Configuration Issues:**
   - Increase max.poll.records if processing is fast
   - Adjust fetch.max.bytes
   - Tune JVM heap size

4. **Immediate Actions:**
   - Scale out consumers
   - Pause non-critical consumers
   - Increase retention if data loss risk
   - Add monitoring and alerts

5. **Long-term Solutions:**
   - Auto-scaling based on lag
   - Circuit breakers for downstream dependencies
   - Bulkhead pattern for resource isolation
   - Regular load testing

**Q36: Explain the internals of Kafka's transaction coordinator.**

**Components:**

1. **Transaction Coordinator:**
   - One per broker, manages subset of transactional IDs
   - Maintains transaction state in __transaction_state topic
   - Coordinates two-phase commit

2. **Transaction States:**
   - Empty: Initial state
   - Ongoing: Transaction in progress
   - PrepareCommit/PrepareAbort: First phase of 2PC
   - CompleteCommit/CompleteAbort: Final state

3. **Transaction Flow:**

   a. **Begin Transaction:**
      - Producer sends InitPid request
      - Coordinator assigns PID and epoch
      - Registers transactional.id → PID mapping

   b. **Add Partitions:**
      - Producer adds partitions to transaction
      - Coordinator tracks all involved partitions

   c. **Write Messages:**
      - Producer writes with PID + epoch + sequence
      - Messages marked as part of transaction

   d. **Commit (Two-Phase):**
      - Phase 1: Write PREPARE_COMMIT to transaction log
      - Phase 2: Write transaction markers to all partitions
      - Write COMPLETE_COMMIT to transaction log

4. **Recovery:**
   - If coordinator fails, new coordinator reads transaction log
   - Completes any pending transactions
   - Ensures atomicity

5. **Performance Considerations:**
   - Transaction log is a bottleneck
   - Partitioning of __transaction_state topic
   - Batch transaction markers
   - Transaction timeout (default 60s)

**Q37: Design a Kafka-based event sourcing system for an e-commerce platform.**

**Requirements:**
- Capture all state changes as events
- Rebuild state from event log
- Support CQRS (Command Query Responsibility Segregation)
- Handle billions of events
- Provide point-in-time recovery

**Architecture:**

1. **Event Topics:**
   - order-events: Order lifecycle events
   - inventory-events: Stock changes
   - payment-events: Payment transactions
   - user-events: User actions

2. **Partitioning Strategy:**
   - Partition by entity ID (order_id, user_id)
   - Ensures ordering for entity-level events
   - Partition count: 100-500 per topic

3. **Event Schema:**
   ```json
   {
     "event_id": "uuid",
     "event_type": "OrderPlaced",
     "aggregate_id": "order_123",
     "aggregate_version": 1,
     "timestamp": "2025-01-24T...",
     "data": {...},
     "metadata": {...}
   }
   ```

4. **Write Side (Commands):**
   - API receives commands
   - Validate business rules
   - Publish events to Kafka
   - Use transactions for multi-entity operations

5. **Read Side (Queries):**
   - Kafka Streams for projections
   - Multiple materialized views:
     - Order summary (current state)
     - Order history (full audit trail)
     - Analytics aggregations
   - Store in appropriate databases:
     - PostgreSQL for transactional queries
     - Elasticsearch for search
     - Redis for caching

6. **Event Processing:**
   ```
   Event Topics → Kafka Streams → State Stores → Materialized Views
   ```
   - Use KTables for current state
   - Use KStreams for audit logs
   - Windowed aggregations for analytics

7. **Key Features:**

   **Compaction:**
   - Use log compaction for snapshot topics
   - Periodic snapshots to reduce replay time

   **Versioning:**
   - Track aggregate version in events
   - Optimistic locking for concurrent updates

   **Replay:**
   - Rebuild projections from events
   - Use seekToBeginning() for full replay
   - Incremental catchup for new projections

8. **Challenges:**

   **Schema Evolution:**
   - Use Avro with Schema Registry
   - Backward and forward compatibility
   - Event versioning strategy

   **Data Volume:**
   - Retention policy: infinite for core events
   - Tiered storage for cold data
   - Compression to reduce storage

   **Consistency:**
   - Eventual consistency between write and read models
   - Version vectors for conflict resolution
   - Idempotent event handlers

**Q38: How would you implement a multi-tenant Kafka architecture?**

**Approaches:**

**1. Topic-Based Isolation:**
- Separate topics per tenant: tenant1-orders, tenant2-orders
- Pros: Strong isolation, easy ACLs
- Cons: Topic explosion, management overhead

**2. Partition-Based Isolation:**
- Single topic, partition by tenant_id
- Pros: Fewer topics, easier management
- Cons: Weaker isolation, noisy neighbor issues

**3. Cluster-Based Isolation:**
- Dedicated cluster per tier (premium, standard, basic)
- Pros: Strongest isolation, SLA guarantees
- Cons: Highest cost, complex management

**Recommended Hybrid Approach:**

1. **Tiered Clusters:**
   - Enterprise tier: Dedicated clusters
   - Business tier: Shared cluster, dedicated topics
   - Basic tier: Shared cluster, shared topics with partitioning

2. **Resource Quotas:**
   ```
   kafka-configs --alter --add-config 'producer_byte_rate=1048576' \
     --entity-type clients --entity-name tenant_123
   ```
   - Per-client quotas for bandwidth
   - Request rate limiting
   - Prevent resource starvation

3. **Security:**
   - ACLs per tenant
   - Separate service accounts
   - Audit logging
   - Encryption at rest and in transit

4. **Monitoring:**
   - Per-tenant metrics
   - Alerting on quota violations
   - Chargeback/showback reporting

5. **Naming Conventions:**
   - <tier>.<tenant_id>.<entity>: enterprise.acme.orders
   - Enables easy filtering and management

**Challenges:**
- Schema Registry multi-tenancy
- Cross-tenant analytics
- Tenant onboarding automation
- Cost allocation
- Compliance and data residency

**Q39: Explain how you would handle a Kafka cluster upgrade with zero downtime.**

**Strategy:**

**Phase 1: Preparation**
1. Review release notes and breaking changes
2. Test in staging environment
3. Backup ZooKeeper/KRaft data
4. Verify client compatibility
5. Plan rollback procedure

**Phase 2: Rolling Upgrade**

1. **Upgrade Protocol Version:**
   ```
   # Set to current version first
   inter.broker.protocol.version=2.8
   log.message.format.version=2.8
   ```

2. **Upgrade Brokers (One at a Time):**
   ```
   # For each broker:
   1. Stop broker
   2. Update Kafka binaries
   3. Start broker
   4. Wait for under-replicated partitions = 0
   5. Proceed to next broker
   ```

3. **Controller Last:**
   - Upgrade controller broker last
   - Minimizes leadership changes

4. **Update Protocol Version:**
   ```
   # After all brokers upgraded
   inter.broker.protocol.version=3.0
   log.message.format.version=3.0
   ```

**Phase 3: Upgrade Clients**
1. Upgrade producers (usually backward compatible)
2. Upgrade consumers
3. Upgrade Kafka Streams applications
4. Upgrade Connect clusters

**Monitoring During Upgrade:**
- Under-replicated partitions
- ISR shrinks/expands
- Producer/consumer errors
- Throughput and latency
- Rebalancing frequency

**Rollback Plan:**
- Downgrade brokers in reverse order
- Revert protocol versions
- Restore from backup if needed

**Best Practices:**
- Upgrade during low-traffic period
- One broker at a time with verification
- Monitor for at least 24 hours post-upgrade
- Document all steps and observations

**Q40: Design a disaster recovery strategy for a critical Kafka deployment.**

**Requirements:**
- RPO (Recovery Point Objective): < 5 minutes
- RTO (Recovery Time Objective): < 15 minutes
- Multi-region setup
- Data integrity guarantees

**Architecture:**

**1. Primary Region:**
- 3 AZ deployment
- Replication factor 3
- min.insync.replicas=2

**2. Secondary Region (DR):**
- MirrorMaker 2.0 for active replication
- Offset translation for consumer groups
- Separate Schema Registry replica

**3. Replication Configuration:**
```
# MirrorMaker 2.0
topics.blacklist = __.*  # Exclude internal topics
sync.topic.acls.enabled = true
sync.topic.configs.enabled = true
sync.group.offsets.enabled = true
```

**4. Failover Procedure:**

a. **Detection:**
   - Regional health checks
   - Automated alerts on region failure
   - Manual confirmation

b. **DNS/Load Balancer Failover:**
   - Update DNS to point to DR region
   - Use low TTL (60s) for quick propagation

c. **Application Changes:**
   - Update bootstrap servers
   - Consumer groups automatically resume from replicated offsets
   - Producers reconnect to new cluster

d. **Validation:**
   - Verify message flow
   - Check consumer lag
   - Monitor error rates

**5. Failback Procedure:**
- Reverse replication to primary
- Sync gap data
- Validate consistency
- DNS cutover back to primary

**6. Testing:**
- Quarterly DR drills
- Chaos engineering (random failures)
- Measure actual RPO/RTO
- Update runbooks

**7. Data Validation:**
- Checksums/hashing for data integrity
- Automated reconciliation jobs
- Alert on message count mismatches

**8. Considerations:**
- Network bandwidth for replication
- Replication lag monitoring
- Cost of running DR cluster
- Partial failover capability (topic-level)

**Alternative: Active-Active:**
- Bidirectional replication
- Conflict resolution strategy
- Regional affinity routing
- Aggregate results from both regions

---

## Behavioral and Scenario-Based Questions

**Q41: Describe a time when you had to debug a critical production issue with Kafka.**

*Expected elements:*
- Problem identification process
- Tools and metrics used
- Root cause analysis
- Resolution steps
- Prevention measures
- Communication with stakeholders

**Q42: How do you decide when to use Kafka vs other messaging systems?**

*Kafka is better for:*
- High throughput, append-only logs
- Event streaming and replay
- Multiple consumers for same data
- Durable storage with retention

*Others might be better for:*
- Complex routing (RabbitMQ)
- Priority queues
- Request-reply patterns
- Small message volumes with low latency requirements

**Q43: Walk me through how you would onboard a new team to Kafka.**

- Training materials and documentation
- Hands-on labs and sandbox
- Code reviews and best practices
- Monitoring and alerting setup
- Incident response procedures
- Knowledge sharing sessions

**Q44: How do you stay updated with Kafka developments?**

- KIPs (Kafka Improvement Proposals)
- Confluent blog and documentation
- Apache Kafka mailing lists
- Conference talks (Kafka Summit)
- Community contributions
- Production experience and experimentation

**Q45: Describe how you would approach capacity planning for Kafka.**

1. **Understand Requirements:**
   - Message volume (current and projected)
   - Message size distribution
   - Retention requirements
   - Throughput and latency SLAs

2. **Calculate Resources:**
   - Storage: messages/day × message_size × retention × replication
   - Network: throughput × replication × consumer_count
   - CPU: compression, serialization overhead
   - Memory: page cache, producer buffers

3. **Broker Sizing:**
   - Rule of thumb: 2000-4000 partitions per broker
   - Partition leaders evenly distributed
   - Consider growth (2-3x buffer)

4. **Testing:**
   - Load testing with production-like data
   - Measure actual throughput and latency
   - Identify bottlenecks

5. **Monitoring and Adjustment:**
   - Track resource utilization
   - Set up alerting for capacity thresholds
   - Regular capacity reviews

---

## Coding and Hands-On Questions

**Q46: Write a Kafka producer with proper error handling.**

```java
Properties props = new Properties();
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
props.put(ProducerConfig.ACKS_CONFIG, "all");
props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
props.put(ProducerConfig.RETRIES_CONFIG, Integer.MAX_VALUE);
props.put(ProducerConfig.MAX_IN_FLIGHT_REQUESTS_PER_CONNECTION, 5);

KafkaProducer<String, String> producer = new KafkaProducer<>(props);

try {
    ProducerRecord<String, String> record = 
        new ProducerRecord<>("my-topic", "key", "value");
    
    producer.send(record, (metadata, exception) -> {
        if (exception != null) {
            // Handle different exception types
            if (exception instanceof RetriableException) {
                log.error("Retriable error: ", exception);
                // Kafka will retry automatically
            } else {
                log.error("Non-retriable error: ", exception);
                // Custom error handling (DLQ, alert, etc.)
            }
        } else {
            log.info("Sent to partition {}, offset {}", 
                     metadata.partition(), metadata.offset());
        }
    });
} catch (Exception e) {
    log.error("Unexpected error: ", e);
} finally {
    producer.flush();
    producer.close();
}
```

**Q47: Write a Kafka consumer with manual offset management.**

```java
Properties props = new Properties();
props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group");
props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Collections.singletonList("my-topic"));

try {
    while (true) {
        ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
        
        for (ConsumerRecord<String, String> record : records) {
            try {
                // Process record
                processRecord(record);
                
                // Commit offset for this record
                Map<TopicPartition, OffsetAndMetadata> offsets = new HashMap<>();
                offsets.put(
                    new TopicPartition(record.topic(), record.partition()),
                    new OffsetAndMetadata(record.offset() + 1)
                );
                consumer.commitSync(offsets);
                
            } catch (Exception e) {
                log.error("Error processing record: ", e);
                // Send to DLQ or handle error
                sendToDLQ(record);
            }
        }
    }
} catch (WakeupException e) {
    // Shutting down
} finally {
    consumer.close();
}
```

**Q48: Implement a simple Kafka Streams application for word count.**

```java
Properties props = new Properties();
props.put(StreamsConfig.APPLICATION_ID_CONFIG, "word-count");
props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
props.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass());
props.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass());

StreamsBuilder builder = new StreamsBuilder();

KStream<String, String> textLines = builder.stream("input-topic");

KTable<String, Long> wordCounts = textLines
    .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
    .groupBy((key, word) -> word)
    .count(Materialized.as("word-counts-store"));

wordCounts.toStream().to("output-topic", Produced.with(Serdes.String(), Serdes.Long()));

KafkaStreams streams = new KafkaStreams(builder.build(), props);
streams.start();

// Shutdown hook
Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
```

**Q49: Implement idempotent message processing in a consumer.**

```java
// Use external store to track processed message IDs
ConcurrentHashMap<String, Long> processedMessages = new ConcurrentHashMap<>();

void processMessage(ConsumerRecord<String, String> record) {
    String messageId = extractMessageId(record);
    
    // Check if already processed
    if (processedMessages.containsKey(messageId)) {
        long previousOffset = processedMessages.get(messageId);
        log.info("Duplicate message {} at offset {}, previously at {}", 
                 messageId, record.offset(), previousOffset);
        return;
    }
    
    try {
        // Process message
        doBusinessLogic(record.value());
        
        // Mark as processed (in database transaction ideally)
        processedMessages.put(messageId, record.offset());
        
    } catch (Exception e) {
        log.error("Error processing message: ", e);
        throw e;
    }
}

// Cleanup old entries periodically
void cleanupProcessedMessages() {
    long cutoffTime = System.currentTimeMillis() - RETENTION_MS;
    processedMessages.entrySet().removeIf(entry -> 
        entry.getValue() < cutoffTime
    );
}
```

**Q50: Design and implement a retry mechanism with exponential backoff.**

```java
public class RetryableKafkaConsumer {
    private static final int MAX_RETRIES = 3;
    private static final long INITIAL_BACKOFF_MS = 1000;
    
    public void processWithRetry(ConsumerRecord<String, String> record) {
        int attempt = 0;
        Exception lastException = null;
        
        while (attempt < MAX_RETRIES) {
            try {
                processRecord(record);
                return; // Success
                
            } catch (RetriableException e) {
                lastException = e;
                attempt++;
                
                long backoffMs = INITIAL_BACKOFF_MS * (long) Math.pow(2, attempt - 1);
                log.warn("Retry attempt {} for record {} after {}ms", 
                         attempt, record.key(), backoffMs);
                
                try {
                    Thread.sleep(backoffMs);
                } catch (InterruptedException ie) {
                    Thread.currentThread().interrupt();
                    throw new RuntimeException("Interrupted during retry", ie);
                }
                
            } catch (NonRetriableException e) {
                log.error("Non-retriable error, sending to DLQ", e);
                sendToDLQ(record, e);
                return;
            }
        }
        
        // Max retries exceeded
        log.error("Max retries exceeded for record {}", record.key(), lastException);
        sendToDLQ(record, lastException);
    }
    
    private void sendToDLQ(ConsumerRecord<String, String> record, Exception e) {
        ProducerRecord<String, String> dlqRecord = new ProducerRecord<>(
            "dlq-topic",
            record.key(),
            createDLQPayload(record, e)
        );
        dlqProducer.send(dlqRecord);
    }
}
```

---

## Practice Recommendations

1. **Set up a local Kafka cluster** and experiment with configurations
2. **Build sample applications** using Kafka Streams and Connect
3. **Monitor and troubleshoot** using JMX, Prometheus, Grafana
4. **Read the source code** of Kafka (especially core modules)
5. **Contribute to open source** or internal tools
6. **Design systems** on paper/whiteboard
7. **Practice explaining** complex concepts simply
8. **Stay updated** with latest Kafka releases and KIPs

Good luck with your FAANG interviews!
