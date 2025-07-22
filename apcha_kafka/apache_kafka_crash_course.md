# [Apache Kafka Crash Course](https://www.youtube.com/watch?v=ZJJHm_bd9Zo) - By Piysh Garg

## Table of Content

- What is kafka?

- Why it is used?

- What are the problems that Kafka solves?

- Why kafka is so much used in the Industries?

---

### Problem Statements

**Example 1:** Zomato Live Tracking of the customer on the map.

- If we try to create it manually then we will just get the live location of driver every second and store it in Database, and send the live location to the customer.

**Problems:**  
  
  - There are 100's of delivery partners in the country, and we are inserting the record every second in the database that means there will be massive amount of data.

  - The database might go down after 1 or 2 minutes, because we are using more database **Operations Per Second** (OPS).

  - **Throughput:** How much OPS we perform on the Database, If we can perform more number of OPS then the throughput is high if not then the throughput is low.
  
  - Throughput of our databases is very low. 

**Example 2:** Large number of people in Discord Servers.

- Insert operation in database takes some milliseconds of time, that means the application is not real-time.

- Here OPS is very high, that means the database is going be down.

**Conclusion:**

- The throughput of the database is very low, we cannot perform more number of OPS.

### Kafka

- It is very much useful in the Industries.

- It was developed by LinkedIn

- Kafka has high throughput, if you insert one lakh records every second it is capable to handle it.

# Kafka Vs Database

| Feature        | Kafka                                               | Database                       |
|----------------|-----------------------------------------------------|--------------------------------|
| Throughput     | Very high                                           | Very low       |
| Storage        | Very low (not suitable for huge data storage)       | High (designed for large data) |
| Use Case       | Real-time streaming, event processing               | Persistent, structured storage |
| Data Retention | Short-term (configurable, but not for long-term)    | Long-term                      |
| Query Support  | Limited (no complex queries)                        | Advanced (SQL, NoSQL, etc.)    |
| Scalability    | Horizontally scalable for messaging                 | Scalable for storage/queries   |
| Reliability    | Good for message delivery, not for permanent storage| Reliable for long-term storage |

- We should use kafka and Database together.

---

### **Producers & Consumers:**

- **Producers:** The one who produces the data (raw data).

The raw data is directly collected by the kafka.

- **Consumers:** The consumers use the raw data and do the analysis. (They are nothing but services.)

- After the analysis is completed, the analyzed data is sent **back to the database in bulk**.

- Here we are inserting the processed data into the database in one transcaction. (1 command) whole data sent to the database. 

--- 

### **Kafka server Archicture**

- Kafka supports having multiple producers.

- Producers will produce the data inside the kafka and consumer will consume the data.

- Kafka servers have topics inside them 

  - **Topic:** A logical partitioning. 

- Consumers will listen on the topics.

- Whenever a message comes then it will tell in which topic we should placed.

**Probelm**

- If ther are many messages on any topic then the messages will get cluttered up.

**Solution**

- This can be further divided into partition.

---

- If there is only one consumer and multiple topics (Partition). Then that one consumer will consume whole data. It will auto balance.

- **Auto balancing** states that if there is only one consumer then it's the responsibility of one consumer to consume whole data.

- If we buy and add one more server (Consumer) then the **kafka will auto balance the data**

- If there are four partitions in the topic in kafka server and five consumers then one of the consumer will not get any load. Because there are only four partitions and four consumers so they will be Balancing the load.

---

**Rule**

- If there are two partitions in the topic in kafka server and only one consumer then the load of the both partitions will be handled by that one consumer.

- **One consumer can consume multiple partitions but Multiple consumers cannot consume one partition**

- To solve this problem of Multiple consumers canout consume one partition we have **Consumer Groups.**

---

## **Consumer Groups:**

- When you create a consumer by default that consumer will be in the group, without groups there are no consumers.

- Let's suppose we have one group with name `one` in that group we have five consumers. And there are only four partitions in the topic of the Kafka server. Here one of the consumer will be idle.

- Now if we created a new group with name `two` and in that group we have another consumer but we only have same four partitions in the topic of the kafka server. 

- Here in this situation the group one consumers will be consuming afor partitions and `one` consumer will be idle. And in group `two` all the partitions of the kafka server will be assigned to ne consumer because we only have one consumer in group two.

**Group level balancing**

- Self-balancing can be happaned only in group level. In one group there one partition cannot have multiple consumers/listener. 

### **Queue Architecture** & **Pub/Sub Architecture**

- **Queue Architecture:** In Queue there will be only one producer and one consumer.

- **Pub/Sub Architecture:** In pub/sub architecture there will be one producer and multiple consumers.

---

- **Kafka internally uses ZooKeeper for partitions handling**.
 
- Kafka uses the concept of the Consumer Group it can be of both the architectures (Queue & Pub/Sub).

- If we want to use Kafka as a queue, we should create a one consumer group with the same number of consumers as there are partitions.

- If we want to use Kafka as a Pub/Sub system, we should create two or more consumer groups, each with the same number of consumers as there are partitions in the topic.

- In Pub/Sub mode, each consumer group receives a copy of every message. Creating multiple consumer groups allows multiple independent applications to consume the Kafka topic independently.

---

## **Coding part of Kafka**

**Prerequisites**

- NodeJS

- Docker

- kafkajs


**Step 1:** First we should run Zookeeper service

* port of zookeeper: 2181 (by default)

**Step 2:** Then we should turn on the Kafka 
* Default port of Kafka: 9092

- To turn on the kafka server we need to use these commands:

  -  docker run -p 9092:9092\
  - -e KAFKA_ZOOKEEPER_CONNECT=`Local_IP_addrress:2181 \` (On which port the zookeper is working)
  - -e KAFKA_ADVERTISED_LISTENERS=`PLAINTEXT://Local_IP_address:9092 \` (On which port the Kafka is working)
  - -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=`1 \` (How many replications should it make)
  - confluentinc/cp-kafka


  ---

- **Infrastracture Setup:** Like creating Partitions, Topics this will be done by Admin

- **Producers:** Message producers.

- **Consumers:** Consumes messages produced by producers.

### In VS code:

- yarn init / npm init

- npm add kafkajs

- Then create `admin.js`, `client.js`, `producer.js`, `consumer.js`

- The services which is running in 9092 is called brockers