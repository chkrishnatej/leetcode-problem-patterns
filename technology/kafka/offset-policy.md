# Offset Policy

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

