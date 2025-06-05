# Commit in Kafka

By default, Kafka has `enable.auto.commit=true`. This commits the offset for every 5 seconds. Apart from this, there are two other types of offset commit strategies. Lets discuss all the commit strategies:

1.  **Auto Commit - Default** \
    This is the simplest way to commit offsets. **Kafka, by default, uses auto-commit – at every five seconds it commits the largest offset returned by the&#x20;**_**poll()**_**&#x20;method**. _poll()_ returns a set of messages with a timeout of _10_ seconds, as we can see in the code:\


    <pre class="language-java"><code class="lang-java">KafkaConsumer&#x3C;Long, String> consumer = new KafkaConsumer&#x3C;>(props);
    consumer.subscribe(KafkaConfigProperties.getTopic());
    <strong>
    </strong><strong>ConsumerRecords&#x3C;Long, String> messages = consumer.poll(Duration.ofSeconds(10));
    </strong>
    for (ConsumerRecord&#x3C;Long, String> message : messages) {
      // processed message
    }
    </code></pre>

    \
    <mark style="color:yellow;">The problem with auto-commit is that there is a very high chance of data loss in case of application failure. When</mark> [_<mark style="color:yellow;">poll</mark>_](https://www.baeldung.com/java-kafka-consumer-api-read#2-consuming-messages)_<mark style="color:yellow;">()</mark>_ <mark style="color:yellow;"></mark><mark style="color:yellow;">returns the messages,</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**Kafka may commit the largest offset before processing messages**</mark><mark style="color:yellow;">.</mark>\


    Let’s say _poll()_ returns 100 messages, and the consumer processes 60 messages when the auto-commit happens. Then, due to some failure, the consumer crashes. When a new consumer goes live to read messages, it commences reading from offset 101, resulting in the loss of messages between 61 and 100.\


    Thus, we need other ways where this drawback isn’t present. The answer is manual commit.
2.  **Manual Sync Commit**\
    \
    In manual commits, whether sync or async,  it’s necessary to disable auto-commit by setting the default property to `false`\


    ```java
    Properties props = new Properties();
    props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, "false");
    ```

    \
    After disabling the manual commit, let’s now understand the use of  `commitSync()`\


    ```java
    KafkaConsumer<Long, String> consumer = new KafkaConsumer<>(props);
    consumer.subscribe(KafkaConfigProperties.getTopic());
    ConsumerRecords<Long, String> messages = consumer.poll(Duration.ofSeconds(10));
      //process the messages
    consumer.commitSync();
    ```

    \
    <mark style="color:yellow;">**This method prevents data loss by committing the offset only after processing the messages**</mark><mark style="color:yellow;">. However, it doesn’t prevent duplicate reading when a consumer crashes before committing the offset. Besides this, it also impacts application performance.</mark>\


    The `commitSync()` blocks the code until it completes. Also, in case of an error, it keeps on retrying. This decreases the throughput of the application, which we don’t want. So, Kafka provides another solution, async commit, that deals with these drawbacks.\

3.  **Async Commit**\
    Kafka provides `commitAsync()` to commit offsets asynchronously. **It overcomes the performance overhead of manual sync commits by committing offsets in different threads.** \
    \
    Let’s implement an async commit to understand this:

    ```java
    KafkaConsumer<Long, String> consumer = new KafkaConsumer<>(props); 
    consumer.subscribe(KafkaConfigProperties.getTopic()); 
    ConsumerRecords<Long, String> messages = consumer.poll(Duration.ofSeconds(10));
      //process the messages
    consumer.commitAsync();
    ```

    \
    <mark style="color:yellow;">The problem with the async commit is that it doesn’t retry in case of failure. It relies on the next call of</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">`commitAsync()`</mark><mark style="color:yellow;">, which will commit the latest offset.</mark>\


    Suppose 300 is the largest offset we want to commit, but our _commitAsync()_ fails due to some issue. It could be possible that before it retries, another call of _commitAsync()_ commits the largest offset of 400 as it is asynchronous. When failed _commitAsync()_ retries and if it commits offsets 300 successfully, it will overwrite the previous commit of 400, resulting in duplicate reading. That is why _commitAsync()_ doesn’t retry.\

4.  **Commit Specific Offset** \
    Sometimes, we need to take more control over offsets. Let’s say we’re processing the messages in small batches and want to commit the offsets as soon as messages are processed. **We can use the overloaded method of** `commitSync()` **and** `commitAsync()` **that takes a map argument to commit the specific offset**:\


    ```java
    KafkaConsumer<Long, String> consumer = new KafkaConsumer<>(props);
    consumer.subscribe(KafkaConfigProperties.getTopic());
    Map<TopicPartition, OffsetAndMetadata> currentOffsets = new HashMap<>();
    int messageProcessed = 0;
      while (true) {
        ConsumerRecords<Long, String> messages = consumer.poll(Duration.ofSeconds(10));
        for (ConsumerRecord<Long, String> message : messages) {
            // processed one message
          messageProcessed++;
          currentOffsets.put(
              new TopicPartition(message.topic(), message.partition()),
              new OffsetAndMetadata(message.offset() + 1));
          if (messageProcessed%50==0){
            consumer.commitSync(currentOffsets);
          }
        }
      }
    ```

    \
    In this code, we manage a currentOffsets map, which takes _TopicPartition_ as key and _OffsetAndMetadata_ as value. We insert the _TopicPartition_ and _OffsetAndMetadata_ of processed messages during message processing into the _currentOffsets_ map. When the number of processed messages reaches fifty, we call _commitSync()_ with the _currentOffsets_ map to mark these messages as committed.

    The behavior of this way is the same as sync and async commit. The only difference is that here we’re deciding the offsets to be committed not Kafka.
