# Durability in Kafka

## Durabilty in Kafka

Durability in Kafka is acheived by replication. Kafka by design makes sure the message is available to the consumers only after it's acknowledged to the topic successfully.&#x20;

The replication is defined using `replication_factor`

Here the message in topic is acknowledged based on `acks`. This defines if the message should be available to the consumers or not.&#x20;

### Types of \`acks\`

1. acks = 0
   * This is fire and forget. Once any of the Kafka node recieves the message, it is immediately considered as acknowledged and is available to consumers.
   * Even though replication factor is configured and will eventually be replicated but it does acknowledge the message immediately.
   * This is great fot high throughput but bad for durability
2. acks = 1
   * Broker acknowdges the message if the leader receives the message
   * Eventual replication will happen based on the `replication_factor`
   * Little bit of latency is introduced but the durability is much better than `acks = 0`
   * Durability is improved, but when leader re-election happens there is a chance of losing the data
3. acks = all
   * Broker acknowledges the message only when all in-sync replicas received the message
   * Provides you with highest durability but the throughput is affected
4. min.insync.replicas
   * Broker acknowledges the message when the `min.insync.replicas` receive the message rather than all in-sync replicas

{% hint style="info" %}
The above durability gurantees can be achieved in single availabilty zone (AZ). But Kafka supports disaster recorvery by having data in multiple availability zones. This is acheived using `rack_awareness`
{% endhint %}

{% embed url="https://www.uber.com/en-IN/blog/kafka/" %}
