# Kafka

### What is Kafka?

Kafka is a distributed data streaming platform that can publish, subscribe to, store and process streams of records.

### What is the data structure used?

It’s a distributed commit log.

A log (a.k.a. {_write-ahead, commit, transaction} log_) is the simplest data structure.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



### How is data stored in Kafka?

Data is stored in topics in Kafka

Kafka stores its data in topics. They’re split into partitions, and replicated across brokers.

```markdown
The structure is roughly:
* topics
    * partitions
            * a bunch of files (the log)
```

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### Terminologies

* **Topic:** \
  Most fundamental unit of organization is the topic, which is something like a table in a relational database. <mark style="color:yellow;">A topic is a log of events</mark>. \
  Logs are easy to understand, because they are simple data structures with well-known semantics.
  * They are append only: When you write a new message into a log, it always goes on the end.
  * They can only be read by seeking an arbitrary offset in the log, then by scanning sequential log entries.&#x20;
  * Events in the log are immutable
* **Partition:**\
  <mark style="color:yellow;">Partitioning takes the single topic log and breaks it into multiple logs</mark>, each of which can live on a separate node in the Kafka cluster. \
  This way, the work of storing messages, writing new messages, and processing existing messages can be split among many nodes in the cluster.
* **Replica:**\
  <mark style="color:yellow;">A replica of a partition is a "copy" of the data, stored on a different broker</mark>. Replicas exist to provide redundancy and increase data availability.
* **Replication Factor:**\
  <mark style="color:yellow;">The replication factor is the number of nodes to which your data is replicated</mark>
* **Kafka Log:**\
  <mark style="color:yellow;">Kafka log is a distributed commit log present inside a partition which has an monotonically increasing offset.</mark>
* **Offset:** \
  <mark style="color:yellow;">The offset is a unique identifier for each message within a partition. It denotes the position of a message within the log and allows consumers to track their progress.</mark>
* **Node:** \
  <mark style="color:yellow;">A node is a single machine (physical or virtual) that is part of a Kafka cluster. Each node hosts one or more Kafka brokers</mark>.
* **Kafka Cluster:** \
  <mark style="color:yellow;">A Kafka cluster is a group of nodes or servers that work together to manage and store Kafka topics and partitions.</mark> The clustering allows Kafka to be highly available and scalable.
* **Producers:** \
  <mark style="color:yellow;">Producers are applications or processes that publish messages to Kafka topics</mark>. They are responsible for choosing which record to assign to which partition within a topic.
* **Consumers:** \
  <mark style="color:yellow;">Consumers are applications or processes that subscribe to topics and process the stream of records produced to them by the Kafka cluster</mark>. Consumers work in consumer groups to consume from topics in parallel.
* **Consumer Groups:**\
  <mark style="color:yellow;">A consumer group is a set of consumers that reads from a topic.</mark> Kafka divides all partitions among the consumers in a group, where any given partition is always consumed once by a group member.\
  Kraft algorithm is a consensus algorithm which balances the partitions among partitions propotionally
* **Bootstrap Server:**\
  <mark style="color:yellow;">A Bootstrap Server is an entry point into the Kafka ecosystem</mark>. It is used by Kafka clients (producers and consumers) to initially connect to the Kafka cluster. The bootstrap server list must include one or more broker addresses. Once connected, clients will obtain the full cluster topology and find the specific brokers that hold the partitions they are interested in. This mechanism allows clients to dynamically discover the entire Kafka cluster through an initial connection to a subset of brokers.

### **What is the difference between Kafka Broker, Kafka Cluster, Kafka Replica and Kafka node?**

#### Differences between Kafka Components

* **Kafka Broker**: A Kafka Broker is a single instance within a Kafka cluster. It serves as a point of storage and message exchange within the cluster. Each broker manages storage for one or more partitions.
* **Kafka Cluster**: A Kafka Cluster refers to the group of Kafka brokers working together. The cluster is the collective entity that manages the distribution of data and ensures availability and resilience.
* **Kafka Replica**: A Kafka Replica is a duplicate of a partition stored on a different broker. Replicas are crucial for Kafka's fault tolerance. They ensure that data remains available even if a broker fails.
* **Kafka Node**: A node is a single machine (physical or virtual) in the cluster. Each node runs one instance of a Kafka broker. Nodes collectively form a Kafka cluster.

The key difference lies in their roles within the Kafka ecosystem: brokers and nodes are about the infrastructure that stores and exchanges data, clusters represent the collective operation of these components, and replicas concern the mechanism for ensuring data reliability and availability.

### Are Kafka replicas placed in the same broker or same cluster?

<mark style="color:yellow;">Kafka replicas are placed across different brokers within the same Kafka cluste</mark>r. This placement strategy is fundamental to Kafka's high availability and fault tolerance. By spreading replicas across multiple brokers, Kafka ensures that the failure of a single broker does not result in the loss of data. Each partition has one leader and multiple follower replicas; the leader handles all read and write requests for the partition, while the followers replicate the leader's data. The followers are ready to be elected as the new leader in case the current leader broker becomes unavailable. This design allows Kafka to provide robust data redundancy and resilience against failures within a cluster.
