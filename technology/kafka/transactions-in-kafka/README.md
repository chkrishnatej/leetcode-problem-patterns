# Transactions in Kafka

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*76kqQ6N6efPY-GnqtSGjlg.png" alt="" height="372" width="700"><figcaption><p>Kafka Transactions</p></figcaption></figure>

In Kafka, transactions play a crucial role in ensuring data consistency and reliability when producing and consuming messages. Transactions enable producers and consumers to work together to achieve atomicity and durability, ensuring that messages are reliably processed across different Kafka topics and partitions.

## T**he flow of transactions in Kafka**

### **Producer for transaction initiation:**

1. Producer Initialization:
   * Producer initializes a transactional context using `beginTransaction()`.
   * The producer obtains a unique transactional ID from the Kafka broker (transaction coordinator).
2. Producing Messages within a Transaction:
   * Producer sends messages using `send()` within the transactional context.
   * Messages are buffered in memory within the transaction.
3. &#x20;Committing a Transaction:
   * Producer calls `commitTransaction()` to commit the transaction.
   * Producer sends a commit request to the transaction coordinator.
   * The coordinator forwards commit requests to relevant partition leaders.
   * Partition leaders ensure message replicas are written and acknowledge the commit.
   * Once a majority of replicas acknowledge, the coordinator confirms commit to the producer.
4. Aborting a Transaction:
   * If an error occurs or `abortTransaction()` is explicitly called, the producer aborts the transaction.
   * The producer sends an abort request to the coordinator.
   * The coordinator instructs partition leaders to discard buffered messages.

### **Consumer Isolation and Reading:**

* Consumers can read messages with different isolation levels.
* In `read_uncommitted` mode, consumers read messages as soon as they are written.
* In `read_committed` mode, consumers only read messages that are committed.

### **Exactly Once Semantics**:

* Transactions, idempotent producers, and consumer offsets tracking enable exactly-once semantics.
* Exactly once ensures that messages are processed and delivered exactly once, without duplication.

### **Transaction Coordinator Role:**

* Kafka brokers act as transaction coordinators.
* They manage transaction states, commit/abort requests, and coordinate with partition leaders.
* They ensure that each transactional ID is used by only one producer at a time.

Sample Java Code for handling transactions at the producer’s end:

```java
import org.apache.kafka.clients.producer.*;

import java.util.Properties;

public class KafkaTransactionalProducerExample {
    public static void main(String[] args) {
        // Set up producer properties
        Properties properties = new Properties();
        properties.put("bootstrap.servers", "localhost:9092");
        properties.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        properties.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        properties.put("acks", "all");
        properties.put("enable.idempotence", "true"); // Enable idempotent producer
        properties.put("transactional.id", "my-transactional-id"); // Unique transactional ID

        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

        try {
            // Initialize transaction
            producer.initTransactions();

            // Begin the transaction
            producer.beginTransaction();

            // Send messages within the transaction
            ProducerRecord<String, String> record1 = new ProducerRecord<>("topic-1", "key1", "value1");
            ProducerRecord<String, String> record2 = new ProducerRecord<>("topic-2", "key2", "value2");

            producer.send(record1);
            producer.send(record2);

            // Commit the transaction
            producer.commitTransaction();
        } catch (ProducerFencedException | OutOfOrderSequenceException | AuthorizationException e) {
            // Handle exceptions
            producer.close();
        } catch (KafkaException e) {
            // Handle other exceptions
            producer.abortTransaction();
        } finally {
            producer.close();
        }
    }
}
```

Java code for consumers to read the commit messages by setting the required isolation level.

```java
import org.apache.kafka.clients.consumer.*;
import org.apache.kafka.common.serialization.StringDeserializer;

import java.util.Properties;

public class KafkaConsumerIsolationExample {
    public static void main(String[] args) {
        // Set up consumer properties
        Properties properties = new Properties();
        properties.put("bootstrap.servers", "localhost:9092");
        properties.put("group.id", "my-group");
        properties.put("key.deserializer", StringDeserializer.class.getName());
        properties.put("value.deserializer", StringDeserializer.class.getName());

        // Set isolation level
        // Read uncommitted (default) or read committed
        properties.put("isolation.level", "read_committed"); // or "read_uncommitted"

        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(properties);

        // Subscribe to topics
        consumer.subscribe(java.util.Arrays.asList("topic-1"));

        try {
            while (true) {
                ConsumerRecords<String, String> records = consumer.poll(100);
                for (ConsumerRecord<String, String> record : records) {
                    System.out.printf("Partition: %d, Offset: %d, Key: %s, Value: %s%n",
                            record.partition(), record.offset(), record.key(), record.value());
                }
            }
        } finally {
            consumer.close();
        }
    }
}
```

### K**ey Points**

Here’s a list of key features of transactional APIs in Kafka:

1. Transactional Producer API: The transactional producer API allows you to produce messages within a transaction, ensuring that messages are either all committed or none are committed. Key methods include:

* `beginTransaction()`: Initializes a new transactional context for the producer.
* `send()`: Sends messages within the ongoing transaction.
* `commitTransaction()`: Commits the transaction, ensuring that the messages are committed.
* `abortTransaction()`: Aborts the transaction, discarding any buffered messages.



2. Transactional Consumer API: The Kafka consumer API allows you to consume messages in sync with producer transactions. You can use `read_committed` isolation to ensure you only consume committed messages.
3. &#x20;Transaction Coordinator: The Kafka brokers act as transaction coordinators. They manage producer transactions and ensure coordination between producers and brokers to achieve atomicity and durability.
4. &#x20;Exactly Once Semantics: Kafka supports exactly-once semantics, which means that a message is processed and delivered once and only once. This is achieved by combining idempotent producers, transactions, and consumer offsets tracking.
5. Isolation Levels: Kafka consumers can read messages with different isolation levels:
   1. `read_uncommitted`: Consumers can read messages immediately, even if they are part of an ongoing transaction.
   2. `read_committed`: Consumers only see messages that have been committed, ensuring higher data consistency.
6. Producer Idempotence: While not strictly a transactional API, enabling producer idempotence ensures that duplicate messages are not produced, which is crucial for maintaining data consistency.
7. Exactly Once Sink Connectors: Kafka Connect provides exactly-once semantics for data sinks using connectors that support transactions.

## R**ealtime use case of transactions in Kafka**

A real-time use case of transactions in Kafka involves maintaining data consistency and reliability in an event-streaming architecture. Let’s consider a scenario where an e-commerce platform uses Kafka for processing orders and payments. Transactions are crucial to ensure that order and payment data remain consistent across different parts of the system:

### **Use Case: E-Commerce Order Processing**

1. Order Placement:

* Customers place orders on the e-commerce website, generating order events.
* These order events are produced to a Kafka topic named “orders” using a transactional producer.

2\. Payment Processing:

* Simultaneously, payment events are generated as customers complete the payment process.
* These payment events are also produced to a Kafka topic named “payments” using another transactional producer.

3\. Atomic Order and Payment Relationship:

* Each order event and its corresponding payment event need to be related atomically to ensure data integrity.
* To achieve this, both order and payment events are produced within a single transaction.

4\. Commit or Abort:

* The transactional producer ensures that either both the order and payment events are successfully committed or none at all.
* If any part of the transaction fails (e.g., payment gateway error), the transaction is aborted, preventing inconsistent data.

5\. Consumer Processing:

* Consumer applications read from the “orders” and “payments” topics using the `read_committed` isolation level.
* This guarantees that only successfully committed orders and payment events are processed.

6\. Exactly Once Semantics:

* With Kafka’s exactly once semantics, consumers can process the order and payment events exactly once, even in the presence of failures or retries.
* This prevents duplicate orders or incorrect payment processing.

7\. Error Handling and Compensation:

* If a transaction is aborted due to payment processing failure, the system can trigger compensation logic, such as notifying the customer or rolling back any order-related changes.

By using Kafka transactions in this e-commerce scenario, the platform ensures that orders and payments are consistently and reliably processed. Transactions help maintain the integrity of the data and provide exactly-once once processing guarantees, enhancing the overall reliability and trustworthiness of the e-commerce platform.

These transactional APIs and components in Kafka play a critical role in enabling applications to maintain data integrity, reliability, and consistency across distributed systems. By leveraging these APIs, developers can build robust and reliable data processing pipelines and event-driven architectures. Always refer to the official Kafka documentation for the most up-to-date information on how to use these APIs effectively.

R**eferences**

[Transactions in Apache Kafka | ConfluentNote For the latest, check out Building Systems Using Transactions in Apache Kafka on Confluent Developer.www.confluent.io](https://www.confluent.io/blog/transactions-apache-kafka/?source=post\_page-----ee1c469fc090--------------------------------)[Kafka Transactional Support: How It Enables Exactly-Once SemanticsKafka transactions are important for atomicity and deliver exactly-once semantics (EOS). Learn about common errors and…developer.confluent.io](https://developer.confluent.io/courses/architecture/transactions/?utm\_source=youtube\&utm\_medium=video\&utm\_campaign=tm.devx\_ch.cd-apache-kafka-internals-101\_content.apache-kafka\&source=post\_page-----ee1c469fc090--------------------------------)[Building Systems Using Transactions in Apache Kafka®Kafka's transactions prevent failure and retries in distributed systems. Learn how transactions and guarantees work…developer.confluent.io](https://developer.confluent.io/learn/kafka-transactions-and-guarantees/?source=post\_page-----ee1c469fc090--------------------------------)[Kafka](https://medium.com/tag/kafka?source=post\_page-----ee1c469fc090---------------kafka-----------------)[Transactions](https://medium.com/tag/transactions?source=post\_page-----ee1c469fc090---------------transactions-----------------)[\
](https://medium.com/tag/kafka-consumer?source=post\_page-----ee1c469fc090---------------kafka\_consumer-----------------)
