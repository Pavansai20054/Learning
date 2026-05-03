# Apache Kafka Crash Course – Summary

By Piysh Garg

---

## Table of Contents

1. [Introduction to Kafka](#introduction-to-kafka)
2. [Problems Kafka Solves](#problems-kafka-solves)
3. [Kafka vs Database](#kafka-vs-database)
4. [Kafka Producers & Consumers](#kafka-producers--consumers)
5. [Kafka Server Architecture](#kafka-server-architecture)
6. [Consumer Groups](#consumer-groups)
7. [Queue vs Pub/Sub Architecture](#queue-vs-pubsub-architecture)
8. [Kafka Infrastructure & Coding](#kafka-infrastructure--coding)

---

## Introduction to Kafka

- Kafka is a high-throughput distributed event streaming platform developed by LinkedIn and widely used in industry.
- It excels in handling massive volumes of real-time data.

---

## Problems Kafka Solves

- Traditional databases struggle with high Operations Per Second (OPS), leading to low throughput and potential downtime.
    - *Example: Live tracking drivers (Zomato) or large Discord servers can overwhelm databases.*
- Kafka addresses these issues by efficiently handling and streaming large-scale, real-time data.

---

## Kafka vs Database

| Feature        | Kafka                                               | Database                       |
|----------------|-----------------------------------------------------|--------------------------------|
| Throughput     | Very high                                           | Very low                       |
| Storage        | Very low (not suitable for huge data storage)       | High (designed for large data) |
| Use Case       | Real-time streaming, event processing               | Persistent, structured storage |
| Data Retention | Short-term (configurable, but not for long-term)    | Long-term                      |
| Query Support  | Limited (no complex queries)                        | Advanced (SQL, NoSQL, etc.)    |
| Scalability    | Horizontally scalable for messaging                 | Scalable for storage/queries   |
| Reliability    | Good for message delivery, not permanent storage    | Reliable for long-term storage |

- Best practice: **Use Kafka and Database together**—Kafka for high-throughput ingestion and event processing, Database for long-term storage.

---

## Kafka Producers & Consumers

- **Producers:** Generate and send raw data to Kafka.
- **Consumers:** Receive and analyze data from Kafka.
- **Data Flow:** Analyzed data is sent back to the database in bulk, reducing database load by batching inserts.

---

## Kafka Server Architecture

- Supports multiple producers and consumers.
- **Topics:** Logical partitions where messages are published.
    - Messages on a topic can be organized further into **partitions** for scalability.
- **Auto-balancing:** Consumers automatically share the load of partitions. If there are more consumers than partitions, some consumers remain idle.
- **Rules:**
    - One consumer can consume multiple partitions.
    - Multiple consumers cannot consume the same partition.
    - Use **Consumer Groups** to manage parallel consumption and load balancing.

---

## Consumer Groups

- Every consumer belongs to a group.
- Within a group:
    - Partitions are divided among consumers for balanced load.
    - No partition is assigned to more than one consumer in the same group.
- Multiple groups allow independent consumption of the same topic (Pub/Sub).
- **Group-level balancing** ensures optimal distribution of partition consumption within each group.

---

## Queue vs Pub/Sub Architecture

- **Queue Architecture:** One producer, one consumer (single-group, one consumer per partition).
- **Pub/Sub Architecture:** One producer, multiple consumers (multiple groups, each with consumers for all partitions).
    - Each group receives its own copy of each message.
- Kafka supports both architectures using consumer groups and partitions.

---

- **ZooKeeper** is used by Kafka for partition management.
- For Queue: Use one consumer group, with consumers equal to partitions.
- For Pub/Sub: Use multiple groups, each with consumers matching the number of partitions.

---

## Kafka Infrastructure & Coding

### Prerequisites

- NodeJS
- Docker
- kafkajs (Node.js Kafka client library)

### Setup Steps

1. **Run ZooKeeper:**  
   - Default port: `2181`

2. **Run Kafka Server:**  
   - Default port: `9092`
   - Use Docker with environment variables for ZooKeeper connection, advertised listeners, and replication factor.

3. **Project Setup in VS Code:**
   - Initialize Node project: `yarn init` or `npm init`
   - Install kafkajs: `npm add kafkajs`
   - Create `admin.js`, `client.js`, `producer.js`, `consumer.js` for managing Kafka operations.

### Components

- **Admin:** Sets up infrastructure (partitions, topics).
- **Producers:** Send messages to Kafka.
- **Consumers:** Consume messages from Kafka.

---

## References

- [Watch the Crash Course](https://www.youtube.com/watch?v=ZJJHm_bd9Zo)

---