# Kafka

### What is Kafka?

Kafka is a distributed data streaming platform that can publish, subscribe to, store and process streams of records.

### What is the data structure used?

It’s a distributed commit log.

A log (a.k.a. {_write-ahead, commit, transaction} log_) is the simplest data structure.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>



### How is data stored in Kafka?

Data is stored in topics in Kafka

Kafka stores its data in topics. They’re split into partitions, and replicated across brokers.

```markdown
The structure is roughly:
* topics
    * partitions
            * a bunch of files (the log)
```

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

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

<mark style="color:yellow;">Kafka replicas are placed across different brokers within the same Kafka cluste</mark>r. This placement strategy is fundamental to Kafka's high availability and fault tolerance. By spreading replicas across multiple brokers, Kafka ensures that the failure of a single broker does not result in the loss of data. Each partition has one leader and multiple follower replicas; the leader handles all read and write requests for the partition, while the followers replicate the leader's data. The followers are ready to be elected as the new leader in case the current leader broker becomes unavailable. This design allows Kafka to provide robust data redundancy and resilience against failures within a cluster.\


## Offset Policy

{% embed url="https://docs.confluent.io/platform/current/clients/consumer.html" %}

1. **What Is an Offset?**
   * In Kafka, messages are stored in topics, and each topic can have multiple partitions.
   * Offsets are integers starting from zero that increment by one as messages get stored.
   * When a consumer reads messages from a partition, Kafka keeps track of the offsets for the messages that the consumer has read.
   * Without offsets, it would be impossible to avoid duplicate processing or data loss.
2.  **Ways to Commit Offsets:**

    There are several ways to commit offsets, each with its own use cases, advantages, and disadvantages.

    a. **Auto Commit:**

    * The simplest way to commit offsets.
    * <mark style="color:yellow;">By default, Kafka uses auto-commit, which triggers a commit every five seconds.</mark>
    * The consumer commits the largest offset returned by the `poll()` method.
    * However, there’s a drawback: in case of application failure, there’s a high chance of data loss.
    * For example, if the consumer processes 60 messages out of 100 returned by `poll()`, and then crashes, the new consumer will start reading from offset 101, missing messages 61 to 100.

    b. **Manual Commit:**

    * To avoid the drawbacks of auto-commit, we can manually commit offsets.
    * Set `auto.commit.offset` to `false`.
    * Use the `commitAsync()` method of the KafkaConsumer to commit offsets asynchronously.
3. **Delivery Semantics:**
   * Correct offset management affects delivery semantics.
   * <mark style="color:yellow;">Auto-commit provides “at least once” delivery, where no messages are missed, but duplicates are possible.</mark>
   * After a crash or rebalance, partitions owned by a crashed consumer are reset to the last committed offset.
   * Messages received since the last commit need to be read again.
4. **Reducing Duplicates:**
   * If you want to minimize duplicate processing, consider reducing the auto-commit interval.

Offset commit policies in Kafka are strategies that dictate when and how the consumer’s position, or offset, in a partition is updated. This is crucial as it affects the delivery semantics of messages from Kafka topics. Here are the key points:

1. **Default Policy (Auto Commit)**: By default, Kafka consumers are configured to auto-commit offsets. This means that Kafka automatically commits the largest offset returned by the `poll()` method at a regular interval (default is every 5 seconds). This policy provides “at least once” delivery, ensuring no messages are missed, but duplicates are possible
2. **Customizing the Policy**: You can customize the offset commit policy in several ways[1](https://www.baeldung.com/kafka-commit-offsets)[3](https://medium.com/@rramiz.rraza/kafka-programming-different-ways-to-commit-offsets-7bcd179b225a):
   * **Change Auto-Commit Interval**: You can reduce the auto-commit interval to decrease the window for duplicates. This is done by adjusting the `auto.commit.interval.ms` configuration property
   * **Disable Auto-Commit**: If you want finer control over offsets, you can disable auto-commit by setting the `enable.auto.commit` property to false. This allows you to use the commit API directly for manual offset management
   * **Synchronous Commits**: Each call to the `commitSync()` method results in an offset commit request being sent to the broker. The consumer is blocked until the request returns successfully[1](https://www.baeldung.com/kafka-commit-offsets).
   * **Asynchronous Commits**: The consumer can send the request and return immediately using asynchronous commits. However, the consumer does not retry the request if the commit fails.
   * **Commit Specific Offset**: You can use the overloaded method of `commitSync()` and `commitAsync()` that takes a map argument to commit a specific offset

Remember, the choice of offset commit policy can significantly impact the performance and reliability of your Kafka consumer application. It’s essential to choose a policy that best suits your application’s requirements.

{% hint style="info" %}
To optimize the performance of the poll loop, you can tune various configuration parameters such as the batch size, fetch size, and poll timeout. By setting these parameters appropriately, you can balance the trade-off between latency and throughput and achieve the desired level of performance for your Kafka consumer application.
{% endhint %}

{% hint style="info" %}
Offsets are stored for each `(consumer group, topic, partition)` tuple. This combination is also used as the key when publishing offsets to the `__consumer_offsets` topic so that log compaction can delete old unneeded offset commit messages and so that all offsets for the same `(consumer group, topic, partition)` tuple are stored in the same partition of the \_\_consumer\_offsets topic (which defaults to 50 partitions)\
\
If you have two consumer groups reading the same topic and even partition a commit from one consumer group will not have any
{% endhint %}

### Reset Offset

{% embed url="https://docs.confluent.io/platform/current/clients/consumer.html#reset-offsets" %}





### Optimize the performance of the poll loop in Kafka Consumer <a href="#a9fb" id="a9fb"></a>

<mark style="color:yellow;">It is important to note that the poll loop is a blocking operation, meaning that it will block until it receives a batch of messages from the broker or a timeout occurs</mark>. Therefore, the poll loop must be run in a separate thread or event loop to avoid blocking the main application thread.

To optimize the performance of the poll loop, you can tune various configuration parameters such as the batch size, fetch size, and poll timeout. By setting these parameters appropriately, you can balance the trade-off between latency and throughput and achieve the desired level of performance for your Kafka consumer application.

To optimize the performance of the poll loop in a Kafka consumer, you can consider the following tips:

1. **Increase the batch size:**\
   To increase the batch size in a Kafka consumer, you can use the `max.poll.records` configuration parameter. This parameter determines the maximum number of records returned in a single call to `poll()` method. By default, it is set to 500 records. You can increase this value to increase the batch size and reduce the number of network round-trips required to fetch messages.\
   \
   Here’s an example of how to set `max.poll.records` to a larger value:

<pre class="language-java"><code class="lang-java"><strong>Properties props = new Properties();
</strong>props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "test-group");
props.put("enable.auto.commit", "true");
props.put("auto.commit.interval.ms", "1000");
props.put("max.poll.records", "1000"); // Set batch size to 1000 records

KafkaConsumer&#x3C;String, String> consumer = new KafkaConsumer&#x3C;>(props, new StringDeserializer(), new StringDeserializer());
consumer.subscribe(Arrays.asList("test-topic"));

while (true) {
    ConsumerRecords&#x3C;String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord&#x3C;String, String> record : records) {
        System.out.println("Received message: " + record.value());
    }
}
</code></pre>

The batch size determines the maximum number of messages that the consumer will fetch in a single poll request. By increasing the batch size, you can reduce the number of network round-trips required to fetch messages, which can improve the overall throughput of the consumer. However, increasing the batch size too much can also increase the latency of the consumer.

{% hint style="info" %}
[Increasing the batch size can also increase the memory usage of the consumer. You should monitor the memory usage of the consumer and ensure that it has enough memory available to handle the larger batch size.](#user-content-fn-1)[^1]
{% endhint %}

**2. Tune the fetch size:**

To tune the fetch size of a Kafka consumer, you can use the `fetch.max.bytes` configuration parameter. This parameter determines the maximum number of bytes that the consumer will fetch in a single request. <mark style="color:yellow;">By default, it is set to 50 MB</mark>.

To optimize the fetch size, you can adjust this parameter to a value that balances the trade-off between network latency and memory usage. A larger fetch size can reduce the number of requests required to fetch a set of records, thereby reducing network overhead. However, a larger fetch size also means that more memory will be required to buffer the fetched records.

Here’s an example of how to set `fetch.max.bytes` to a larger value:

```java
props.put("fetch.max.bytes", "10485760");  // Set fetch size to 10 MB
```

The fetch size determines the maximum number of bytes that the consumer will fetch in a single poll request. By increasing the fetch size, you can reduce the number of network round-trips required to fetch messages, which can also improve the overall throughput of the consumer. However, increasing the fetch size too much can increase the memory usage of the consumer.

**3. Reduce the poll timeout**:

<mark style="color:yellow;">The poll timeout determines how long the consumer will wait for messages before returning an empty response. By reducing the poll timeout, you can make the consumer more responsive to new messages, which can reduce the overall latency of the consumer</mark>. However, reducing the poll timeout too much can also increase the number of empty responses, which can reduce the overall throughput of the consumer.

```java
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100)); // Set poll timeout to 100 milliseconds
    for (ConsumerRecord<String, String> record : records) {
        System.out.println("Received message: " + record.value());
    }
}
```

**4. Use asynchronous processing**:

If your message processing logic is CPU-bound or involves I/O operations, you can consider using asynchronous processing techniques such as threading or non-blocking I/O. By processing messages asynchronously, you can reduce the amount of time that the poll loop is blocked, which can improve the responsiveness and throughput of the consumer.

```java
ExecutorService executorService = Executors.newFixedThreadPool(10);

while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        executorService.submit(() -> {
            processRecord(record); // Process the record asynchronously
        });
    }
}

public void processRecord(ConsumerRecord<String, String> record) {
    // Process the record
    System.out.println("Received message: " + record.value());
}
```

**5. Use parallel processing:**

If your message processing logic can be parallelized, you can consider using multiple threads or processes to process messages in parallel. By processing messages in parallel, you can take advantage of multiple CPU cores and increase the overall throughput of the consumer.

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "test-group");
props.put("enable.auto.commit", "true");
props.put("auto.commit.interval.ms", "1000");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props, new StringDeserializer(), new StringDeserializer());
consumer.subscribe(Arrays.asList("test-topic"));

int numThreads = 10;
int batchSize = 100;
List<ConsumerRecord<String, String>> records = new ArrayList<>();

while (true) {
    ConsumerRecords<String, String> batchRecords = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : batchRecords) {
        records.add(record);
    }
    if (records.size() >= batchSize) {
        List<List<ConsumerRecord<String, String>>> partitions = Lists.partition(records, batchSize / numThreads);
        List<Callable<Void>> tasks = new ArrayList<>();
        for (List<ConsumerRecord<String, String>> partition : partitions) {
            tasks.add(() -> {
                for (ConsumerRecord<String, String> record : partition) {
                    processRecord(record); // Process the record
                }
                return null;
            });
        }
        ExecutorService executorService = Executors.newFixedThreadPool(numThreads);
        try {
            executorService.invokeAll(tasks);
        } catch (InterruptedException e) {
            // Handle the exception
        } finally {
            executorService.shutdown();
        }
        consumer.commitSync();
        records.clear();
    }
}

public void processRecord(ConsumerRecord<String, String> record) {
    // Process the record
    System.out.println("Received message: " + record.value());
}
```

**6. Monitor the consumer performance:**

It is important to monitor the performance of the consumer regularly to detect any bottlenecks or issues that may affect its performance. You can use tools such as JConsole or Kafka consumer lag monitoring tools to monitor the consumer performance and identify any performance issues.

By applying these tips, you can optimize the performance of the poll loop in a Kafka consumer and achieve the desired level of throughput and latency for your application.

\


[^1]: 
