### 🔧 **Core Kafka Architecture & Concepts**

1. **Kafka Architecture & Components**

   * Brokers, topics, partitions, leaders, replicas, ISR, ZooKeeper/KRaft.
   * Producer/Consumer lifecycle.

2. **Topic Partitions and Data Distribution**

   * How partitioning works, key-based partitioning, round-robin.
   * Trade-offs between throughput and ordering guarantees.

3. **Producers: Acks, Retries, Idempotence**

   * `acks=0/1/all`, idempotent producers, exactly-once semantics (EOS).
   * How retries and batching affect performance and delivery guarantees.

4. **Consumers & Consumer Groups**

   * Group coordination, partition rebalancing, cooperative rebalancing.
   * Offset management (auto/manual commits).

5. **Kafka Message Delivery Semantics**

   * At-most-once, at-least-once, exactly-once.
   * How to build idempotent consumers.

---

### 🛠️ **Performance, Tuning & Scaling**

6. **Kafka Performance Tuning**

   * Batching, compression, linger.ms, fetch.min.bytes, buffer.memory.
   * High-throughput producer/consumer tuning tips.

7. **Kafka Storage Internals**

   * Log segments, retention (time/size), compaction, indexes.
   * Cleanup policies: `delete` vs `compact`.

8. **High Availability & Fault Tolerance**

   * Leader election, replication, ISR, unclean.leader.election.
   * How Kafka ensures durability and availability.

9. **Kafka in Production: Monitoring & Metrics**

   * JMX metrics to monitor (lag, ISR shrink, disk usage, throughput).
   * Tools: Prometheus + Grafana, Confluent Control Center.

10. **Backpressure and Consumer Lag**

    * Identifying and mitigating lag.
    * Rate limiting strategies; pausing/resuming consumers.

---

### 📦 **Advanced Kafka Usage**

11. **Kafka Streams API & Use Cases**

    * Stateless vs stateful operations, joins, windowing.
    * When to use Streams vs consumers + DB.

12. **Schema Management with Kafka**

    * Avro, Protobuf, JSON Schema + Confluent Schema Registry.
    * Compatibility modes: BACKWARD, FORWARD, FULL.

13. **Kafka Connect**

    * Source & sink connectors, custom connector basics.
    * Debezium (CDC), integration patterns.

14. **Exactly-Once Semantics (EOS) in Kafka**

    * Transactions, idempotence, `enable.idempotence`, `transactional.id`.
    * Coordination between producer, Kafka, consumer (EOS chain).

15. **Kafka Security**

    * TLS encryption, SASL (PLAIN, SCRAM), ACLs.
    * Securing producer/consumer and broker.
