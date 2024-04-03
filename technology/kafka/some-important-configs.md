# Some Important Configs

## Kafka Producer Config

Now that you have a basic understanding of what Kafka and the Kafka Producer Configurations are, let’s discuss the various Kafka Producer Configurations in the Kafka ecosystem. Although there are tons of configurations, the mandatory Kafka Producer Configurations that one needs to know in order to get started are very limited.

* [`acks`](https://hevodata.com/learn/kafka-producer-config/#c1)
* [`bootstrap.servers`](https://hevodata.com/learn/kafka-producer-config/#c2)
* [`retries`](https://hevodata.com/learn/kafka-producer-config/#c3)
* [`enable.idempotence`](https://hevodata.com/learn/kafka-producer-config/#c4)
* [`max.in.flight.requests.per.connection`](https://hevodata.com/learn/kafka-producer-config/#c5)
* [`buffer.memory`](https://hevodata.com/learn/kafka-producer-config/#c6)
* [`max.block.ms`](https://hevodata.com/learn/kafka-producer-config/#c7)
* [`linger.ms`](https://hevodata.com/learn/kafka-producer-config/#c8)
* [`batch.size`](https://hevodata.com/learn/kafka-producer-config/#c9)
* [`compression.type`](https://hevodata.com/learn/kafka-producer-config/#c10)

#### acks <a href="#c1" id="c1"></a>

`Acks` represent the number of acknowledgments that the producer needs the leader broker to have received before considering a successful commit. This helps to control the durability of messages that are sent. The following are the common settings for the acks Kafka Producer Config:

* `acks=0`**:** Setting acks to 0 means the producer will not get any acknowledgment from the server at all. This means that the record will be immediately added to the socket buffer and considered sent.&#x20;
* `acks=1`**:** This means that as long as the producer receives an acknowledgment from the leader broker, it would consider it as a successful commit.&#x20;
* `acks=all`: This means the producer will have to wait for acknowledgments from all the in-sync replicas of that topic before considering a successful commit. It gives the strongest available message durability.

#### bootstrap.servers <a href="#c2" id="c2"></a>

`bootstrap.server` represents a list of host/port pairs that are used for establishing the initial connection to the Kafka Cluster. The list need not contain the full set of servers as they are used just to establish the initial connection to identify full cluster membership. The list should be in the format given below:

```
host1:port1,host2:port2,....
```

#### retries <a href="#c3" id="c3"></a>

By default, the producer doesn’t resend records if a commit fails. However, the producer can be configured to resend messages “`n`” a number of times with `retries=n`. `retries` basically represent the maximum number of times the producer would retry if the commit fails. The default value is 0.

#### enable.idempotence <a href="#c4" id="c4"></a>

In simple terms, idempotence is the property of certain operations to be applied multiple times without changing the result. When turned on, a producer will make sure that just one copy of a record is being published to the stream. The default value is`false`, meaning a producer may write duplicate copies of a message to the stream. To turn idempotence on, use the below command.

```
enable.idempotent=true
```

#### max.in.flight.requests.per.connection <a href="#c5" id="c5"></a>

`max.in.flight.requests.per.connection` Kafka Producer Config represents the maximum number of unacknowledged requests that the client will send on a single connection before blocking. The default value is 5.

If `retries` are enabled, and `max.in.flight.requests.per.connection` is set greater than 1, there lies a risk of message re-ordering.

#### buffer.memory <a href="#c6" id="c6"></a>

`buffer.memory` represents the total bytes of memory that the producer can use to buffer records waiting to be sent to the server. The default `buffer.memory` is 32MB. If the producer sends the records faster than they can be delivered to the server, the `buffer.memory` will be exceeded and the producer will block them for `max.block.ms` (discussed next), henceforth it will throw an exception. The `buffer.memory` setting should roughly correspond to the total memory used by the producer.

#### max.block.ms <a href="#c7" id="c7"></a>

`max.block.ms` basically defines the maximum duration for which the producer will block _KafkaProducer.send()_ and _KafkaProducer.partitionsFor()_. These methods can be blocked whenever the `buffer.memory` is exceeded or when the metadata is unavailable.

#### linger.ms <a href="#c8" id="c8"></a>

`linger.ms` represents the artificial delay time before the batched request of records is ready to be sent. Any records that come in between request transmissions are batched together into a single request by the producer. `linger.ms` signifies the upper bound on the delay for batching. The default value is 0 which means there will be no delay and the batches will be immediately sent (even if there is only 1 message in the batch).&#x20;

In some circumstances, the client may increase `linger.ms` to reduce the number of requests even under moderate load to improve throughput. But this way, more records will be stored in the memory.&#x20;

#### batch.size <a href="#c9" id="c9"></a>

Whenever multiple records are sent to the same partition, the producer attempts to batch the records together. This way, the performance of both the client and the server can be improved. `batch.size` represents the maximum size (in bytes) of a single batch.

Small batch size will make batching irrelevant and will reduce throughput, and a very large batch size will lead to memory wastage as a buffer is usually allocated in anticipation of extra records.

#### compression.type <a href="#c10" id="c10"></a>

`compression.type` signifies the compression type for all data generated by the producer. The default value is `none` which means there is no compression. You can further set the `compression.type` to `gzip`, `snappy`, or `lz4`.

***



