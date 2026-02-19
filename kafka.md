1. Core Architecture & Fundamentals
   Theory
   Kafka Architecture Components:

Brokers: Servers that store and serve data. Form a cluster for fault tolerance
Topics: Logical channels/categories for messages
Partitions: Topics split into ordered, immutable sequence of records. Enable parallelism
Replication: Each partition has leader and followers (replicas) for fault tolerance
Producers: Publish messages to topics
Consumers: Subscribe and process messages from topics
Consumer Groups: Enable parallel processing and load balancing
ZooKeeper/KRaft: Cluster coordination (KRaft is the newer replacement)

Key Concepts:

Offset: Unique sequential ID for each message within a partition
Retention: How long messages are kept (time/size based)
Compaction: Keeps only latest value for each key
ISR (In-Sync Replicas): Replicas that are caught up with leader



-------------------------

Commands :

# Start
docker compose up -d

# Verify Kafka is running
docker exec kafka kafka-topics --bootstrap-server localhost:9092 --list

# Create a test topic
docker exec kafka kafka-topics --bootstrap-server localhost:9092 --create --topic test-topic --partitions 6 --replication-factor 1

# Produce a test message
echo "Hello Kafka" | docker exec -i kafka kafka-console-producer --bootstrap-server localhost:9092 --topic test-topic

# Consume
docker exec kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic test-topic --from-beginning

# Check Schema Registry
curl http://localhost:8081/subjects