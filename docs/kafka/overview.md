Apache Kafka is a distributed streaming platform that was developed by LinkedIn in 2010 and later donated to the Apache Software Foundation. It's designed to handle high-throughput, fault-tolerant, and real-time data streaming. It is often referred to as the *distributed commit log*.

**------------------------------------------------------------------------------------------------------------**

### **Messaging Systems**

The main task of messaging system is to transfer data from one application to another so that the applications can mainly work on data without worrying about sharing it.
Distributed messaging is based on the reliable message queuing process. Messages are queued non-synchronously between the messaging system and client applications.

---

> --- ***There are two types of messaging patterns available***

1. Point to Point - In this messaging system, messages continue to remain in a queue. More than one consumer can consume the messages in the queue but only one consumer can consume a particular message. After the consumer reads the message in the queue, the message disappears from that queue.

2. Publish-subscribe - In this messaging system, messages continue to remain in a Topic. Contrary to Point to point messaging system, consumers can take more than one topic and consume every message in that topic. Message producers are known as publishers and Kafka consumers are known as subscribers.

---

> --- ***Key characteristics***

   1. Scalable - It supports horizontal scaling by allowing you to add new brokers (servers) to the clusters.
   2. Fault-tolerant - It can handle failures effectively due to its distributed nature and replication mechanisms.
   3. Durable - Kafka uses a "distributed commit log," which means messages are persisted on disk . This ensures data is not lost even if a server goes down.
   4. Fast - Designed to be as fast as possible.
   5. Performance - Achieves high throughput for both publishing (producers) and subscribing (consumers).
   6. No data loss - Guarantees that messages are not lost once they are committed to Kafka.
   7. Zero down time - Designed for continuous operation without interruption.
   8. Reliability - Provides reliable message delivery.

---

> --- ***Kafka vs Traditional Messaging systems***

| **Feature**              | **Traditional Messaging System**                                                                                                | **Kafka Streaming Platform**                                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Message Persistence**  | The broker is responsible for keeping track of consumed messages and removing them when messages are read.                      | Messages are typically retained in Kafka topics for a configurable period of time, even after they have been consumed. Kafka offers message persistence, ensuring data durability. |
| **Scalability**          | Not a distributed system, so it is not possible to scale horizontally.                                                          | It is a distributed streaming system, so by adding more partitions, we can scale horizontally.                                                                                     |
| **Data Model**           | Primarily point-to-point (queues/topics) messaging model.                                                                       | Built around a publish-subscribe (logs) model, which enables multiple consumers to subscribe to topics and process data concurrently.                                              |
| **Ordering of Messages** | Message ordering can be guaranteed within a single queue or topic but may not be guaranteed across different queues or topics.  | Kafka maintains message order within a partition, ensuring that messages within a partition are processed in the order they were received.                                         |
| **Message Replay**       | Limited or no built-in support for message replay. Once consumed, messages may be lost unless custom solutions are implemented. | Supports message replay from a specified offset, allowing consumers to reprocess past data, which is valuable for debugging, analytics, and data reprocessing.                     |
| **Use Cases**            | Typically used for traditional enterprise messaging, remote procedure calls (RPC), and task queues.                             | Well-suited for real-time analytics, log aggregation, event sourcing, and building data-intensive, real-time applications.                                                         |

**------------------------------------------------------------------------------------------------------------**

### **Kafka Architecture**

![Steps](kafkaarc.svg)

> --- ***Message***

A message is a primary unit of data within Kafka. Messages sent from a producer consist of the following parts:

![Steps](message.svg)

1. Message Key (optional) - Keys are commonly used when ordering or grouping related data. For example, in a log processing system, we can use the user ID as the key to ensure that all log messages for a specific user are processed in the order they were generated.

2. Message Value - It contains the actual data we want to transmit. Kafka does not interpret the content of the value. It is received and sent as it is. It can be XML, JSON, String, or anything. Kafka does not care and stores everything. Many Kafka developers favor using Apache Avro, a serialization framework initially developed for Hadoop.

3. Timestamp (Optional) - We can include an optional timestamp in a message indicating its creation timestamp. It is useful for tracking when events occurred, especially in scenarios where event time is important.

4. Compression Type (Optional) - Kafka messages are generally small in size, and sent in a standard data format, such as JSON, Avro, or Protobuf. Additionally, we can further compress them into gzip, lz4, snappy or zstd formats.

5. Headers (Optional) - Kafka allows adding headers that may contain additional meta-information related to the message.

6. Partition and Offset Id - Once a message is sent into a Kafka topic, it also receives a partition number and offset id that is stored within the message.

> --- ***Kafka Cluster***

A Kafka cluster is a system of multiple interconnected Kafka brokers (servers). These brokers cooperatively handle data distribution and ensure fault tolerance, thereby enabling efficient data processing and reliable storage.

> --- ***Kafka Broker***

A Kafka broker is a server in the Apache Kafka distributed system that stores and manages the data (messages). It handles requests from producers to write data, and from consumers to read data. Multiple brokers together form a Kafka cluster.

> --- ***Kafka Zookeeper***

Apache ZooKeeper is a service used by Kafka for cluster coordination, failover handling, and metadata management. It keeps Kafka brokers in sync, manages topic and partition information, and aids in broker failure recovery and leader election.

> --- ***Kafka Producer***

In Apache Kafka, a producer is an application that sends messages to Kafka topics. It handles message partitioning based on specified keys, serializes data into bytes for storage, and can receive acknowledgments upon successful message delivery. Producers also feature automatic retry mechanisms and error handling capabilities for robust data transmission.

> --- ***Kafka Consumer***

A Kafka consumer is an application that reads (or consumes) messages from Kafka topics. It can subscribe to one or more topics, deserializes the received byte data into a usable format, and has the capability to track its offset (the messages it has read) to manage the reading position within each partition. It can also be part of a consumer group to share the workload of reading messages.

**------------------------------------------------------------------------------------------------------------**

### **Role of Zookeeper in Kafka**

Zookeeper is a critical component used to monitor Kafka clusters and coordinate with them.
It stores all the metadata information related to Kafka clusters, including the status of replicas and leaders.
This metadata is crucial for configuration information, cluster health, and leader election within the cluster.
Zookeeper nodes working together to manage distributed systems are known as a Zookeeper Cluster or Zookeeper Ensemble.

!!! Note
      If a Kafka server hosting a partition's leader fails, Zookeeper quickly identifies this and coordinates the election of a new leader from the available replicas, ensuring continuous operation.
      Zookeeper uses specific parameters and maintains various internal states to manage Kafka.

---

> --- ***Zookeeper Configuration Concepts***

1. initLimit - Defines the time in milliseconds that a Zookeeper follower node can take to initially connect to a leader. 

2. syncLimit - Defines the time in milliseconds that a Zookeeper follower can be out of sync with the leader.

3. clientPort - This is the port number (e.g., 2181) where Zookeeper clients connect. It refers to the data directory used to store client node server details.

4. maxClientCnxns - This parameter sets the maximum number of client connections that a single Zookeeper server can handle at once.

5. server.1, server.2, server.3 - These entries define the server IDs and their IP addresses/ports within the Zookeeper ensemble (e.g., server.1: 2888:3888). These are crucial for leader election among the Zookeeper servers.

---

> --- ***Kafka Partition States (as managed by Zookeeper)***

1. New Nonexistent Partition - This state indicates that a partition was either never created or was created and then subsequently deleted.
2. Nonexistent Partition (after deletion) - This state specifically means the partition was deleted.
3. Offline Partition - A partition is in this state when it should have replicas assigned but has no leader elected.
4. Online Partition - A partition enters this state when a leader is successfully elected for it. If all leader election processes are successful, the partition transitions from Offline Partition to Online Partition.

---

> --- ***Kafka Replica States (as managed by Zookeeper)***

1. New Replica - Replicas are created during topic creation or partition reassignment. In this state, a replica can only receive follower state change requests.
2. Online Replica - A replica is considered Online when it is started and has assigned replicas for its partition. In this state, it can either become a leader or become a follower based on state change requests.
3. Offline Replica - If a replica dies (becomes unavailable), it moves to this state. This typically happens when the replica is down.
4. Nonexistent Replica - If a replica is deleted, it moves into this state.

---

> --- ***What does ZooKeeper do in Kafka Cluster?***

1. Broker Management - It helps manage and coordinate the Kafka brokers, and keeps a list of them.
2. Topic Configuration Management - ZooKeeper maintains a list of topics, number of partitions for each topic, location of each partition and the list of consumer groups.
3. Leader Election - If a leader (the node managing write and read operations for a partition) fails, ZooKeeper can trigger leader election and choose a new leader.
4. Cluster Membership - It keeps track of all nodes in the cluster, and notifies if any of these nodes fail.
5. Synchronization - ZooKeeper helps in coordinating and synchronizing between different nodes in a Kafka cluster.

**------------------------------------------------------------------------------------------------------------**

###  **Partitions**

![Steps](partition.svg)

Topics are split into partitions.
All messages within a specific partition are ordered and immutable (meaning they cannot be changed after being written).
Each message within a partition has a unique ID called an Offset. This offset denotes the message's position within that specific partition.

!!! Example
      If your sports_news topic has three partitions (P0, P1, P2), articles related to football might go to P0, basketball to P1, and tennis to P2. Within P0, all football articles will appear in the exact order they were published, each with its unique offset.

---

> --- ***Partitions play a crucial role in Kafka's functionality and scalability*** 

1. Parallelism - Partitions enable parallelism. Since each partition can be placed on a separate machine (broker), a topic can handle an amount of data that exceeds a single server's capacity. This allows producers and consumers to read and write data to a topic concurrently, thus increasing throughput.

2. Ordering - Kafka guarantees that messages within a single partition will be kept in the exact order they were produced. However, if order is important across partitions, additional design considerations are needed.

3. Replication - Partitions of a topic can be replicated across multiple brokers based on the topic's replication factor. This increases data reliability and availability.

4. Failover - In case of a broker failure, the leadership of the partitions owned by that broker will be automatically taken over by another broker, which has the replica of these partitions.

5. Consumer Groups - Each partition can be consumed by one consumer within a consumer group at a time. If more than one consumer is needed to read data from a topic simultaneously, the topic needs to have more than one partition.

6. Offset - Every message in a partition is assigned a unique (per partition) and sequential ID called an offset. Consumers use this offset to keep track of their position in the partition.

---

> --- ***Kafka Partition Assignment Strategies***

When rebalancing happens, Kafka uses specific algorithms to determine how partitions are assigned to consumers.

Range Partitioner - The Range Partitioner assigns a contiguous "range" of partitions to each consumer. It sorts partitions numerically (e.g., 0, 1, 2, 3, 4, 5).It then divides the total number of partitions by the number of consumers to determine the number of partitions each consumer should handle. A contiguous block of partitions (a "range") is assigned to each consumer.This strategy ensures a relatively uniform distribution of the number of partitions per consumer, though not necessarily the load if data is skewed across partitions.

![Steps](range.svg)

!!! Example
    Suppose a topic has 6 partitions (0, 1, 2, 3, 4, 5) and there are 2 consumers in the group.
    Consumer 1 would be assigned partitions 0, 1, 2 (a range of three partitions).
    Consumer 2 would be assigned partitions 3, 4, 5 (the next range of three partitions).

Round Robin Partitioner - The Round Robin Partitioner distributes partitions among consumers in a rotating, round-robin fashion.It iterates through the sorted list of partitions (0, 1, 2, 3, 4, 5).It assigns the first partition to Consumer 1, the second to Consumer 2, the third back to Consumer 1, and so on.

![Steps](roundrobin.svg)

!!! Example
      Suppose a topic has 6 partitions (0, 1, 2, 3, 4, 5) and there are 2 consumers in the group.
      Consumer 1 would be assigned partitions 0, 2, 4.
      Consumer 2 would be assigned partitions 1, 3, 5.

This strategy aims for a more even distribution of partitions, which can sometimes lead to better load balancing if the message load per partition is relatively uniform.

---

> --- ***Kafka Cluster & Partition Reassignment***

1. Kafka Cluster Controller - In a Kafka cluster, one of the brokers is designated as the controller. This controller is responsible for managing the states of partitions and replicas and for performing administrative tasks such as reassigning partitions.
2. Partition Growth - It is important to note that the partition count of a Kafka topic can always be increased, but never decreased. This is because reducing partitions could lead to data loss.
3. Partition Reassignment Use Cases - Partition reassignment is used in several scenarios. Moving a partition across different brokers. Rebalancing the replicas of a partition to a specific set of brokers. Increasing the replication factor of a topic.

---

### **Replications**

![Steps](replicator.svg)

Replicas are essentially backups of partitions.
They are not directly read as raw data.
Their primary purpose is to prevent data loss and provide fault tolerance. If the server hosting an active partition fails, a replica can take over.

*Illustrative Example*: If the server hosting Partition P0 of sports_news crashes, a replica of P0 on another server immediately takes over, ensuring that no sports news articles are lost and the news feed remains continuous.

One broker is marked leader and other brokers are called followers for a specific partition. This designated broker assumes the role of the leader for the topic partition. On the other hand, any additional broker that keeps track of the leader partition is called a follower and it stores replicated data for that partition.

!!! Tip
    Note that the leader receives and serves all incoming messages from producers and serves them to consumers. Followers do not serve read or write requests directly from producers or consumers. Followers just act as backups and can take over as the leader in case the current leader fails.

Therefore, each partition has one leader and multiple followers.

---

***In-Sync Replicas (ISR)***
When a partition is replicated across multiple brokers, not all replicas are necessarily in sync with the leader at all times. The in-sync replicas represent the number of replicas that are always up-to-date and synchronized with the partition’s leader. The leader continuously sends messages to the in-sync replicas, and they acknowledge the receipt of those messages.

The recommended value for ISR is always greater than 1.

!!! Tip
    The ideal value of ISR is equal to the replication factor.

---

### **Offsets**
Offsets represent the position of each message within a partition and are uniquely identifiable, ever-increasing integers . There are three main variations of offsets :

   1. **Log End Offset**: This refers to the offset of the last message written to any given partition .
   2. **Current Offset**: This is a pointer to the last record that Kafka has already sent to the consumer in the current poll .
   3. **Committed Offset**: This indicates the offset of a message that a consumer has successfully consumed .
   4. **Relationship**: The committed offset is typically less than the current offset .

In Apache Kafka, consumer offset management – that is, tracking what messages have been consumed – is handled by Kafka itself.

When a consumer in a consumer group reads a message from a partition, it commits the offset of that message back to Kafka. This allows Kafka to keep track of what has been consumed, and what messages should be delivered if a new consumer starts consuming, or an existing consumer restarts.

Earlier versions of Kafka used Apache ZooKeeper for offset tracking, but since version 0.9, Kafka uses an internal topic named "**__consumer_offsets**" to manage these offsets. This change has helped to improve scalability and durability of consumer offsets.

Kafka maintains two types of offsets:

1.	**Current Offset** : The current offset is a reference to the most recent record that Kafka has already provided to a consumer. As a result of the current offset, the consumer does not receive the same record twice.
2.	**Committed Offset** : The committed offset is a pointer to the last record that a consumer has successfully processed. We work with the committed offset in case of any failure in application or replaying from a certain point in event stream.

---

***Committing an offset***

1. **Auto Commit**: By default, the consumer is configured to use an automatic commit policy, which triggers a commit on a periodic interval. This feature is controlled by setting two properties:
 enable.auto.commit & auto.commit.interval.ms

    Although auto-commit is a helpful feature, it may result in duplicate data being processed.
    
    Let’s have a look at an example.
    You’ve got some messages in the partition, and you’ve requested your first poll. Because you received ten messages, the consumer raises the current offset to ten. You process these ten messages and initiate a new call in four seconds. Since five seconds have not passed yet, the consumer will not commit the offset. Then again, you’ve got a new batch of records, and rebalancing has been triggered for some reason. 
    The first ten records have already been processed, but nothing has yet been committed. Right? The rebalancing process has begun. As a result, the partition is assigned to a different consumer. Because we don’t have a committed offset, the new partition owner should begin reading from the beginning and process the first ten entries all over again.
    A manual commit is the solution to this particular situation. As a result, we may turn off auto-commit and manually commit the records after processing them.

2. **Manual Commit**: With Manual Commits, you take the control in your hands as to what offset you’ll commit and when. You can enable manual commit by setting the enable.auto.commit property to false.
There are two ways to implement manual commits :
    1. **Commit Sync**: The synchronous commit method is simple and dependable, but it is a blocking mechanism. It will pause your call while it completes a commit process, and if there are any recoverable mistakes, it will retry. Kafka Consumer API provides this as a prebuilt method.
    2. **Commit Async**: The request will be sent and the process will continue if you use asynchronous commit. The disadvantage is that commitAsync does not attempt to retry. However, there is a legitimate justification for such behavior.
    Let’s have a look at an example.
    Assume you’re attempting to commit an offset as 70. It failed for whatever reason that can be fixed, and you wish to try again in a few seconds. Because this was an asynchronous request, you launched another commit without realizing your prior commit was still waiting. It’s time to commit-100 this time. Commit-100 is successful, however commit-75 is awaiting a retry. Now how would we handle this? Since you don’t want an older offset to be committed.
    This could cause issues. As a result, they created asynchronous commit to avoid retrying. This behavior, however, is unproblematic since you know that if one commit fails for a reason that can be recovered, the following higher level commit will succeed.

---

***What if AsyncCommit failure is non-retryable?***

Asynchronous commits can fail for a variety of reasons. For example, the Kafka broker might be temporarily down, the consumer may be considered dead by the group coordinator and kicked out of the group, the committed offset may be larger than the last offset the broker has, and so on.

When the commit fails with a non-retryable error, the commitAsync method doesn't retry the commit, and your application doesn't get a direct notification about it, because it runs in the background. However, you can provide a callback function that gets triggered upon a commit failure or success, which can log the error and you can take appropriate actions based on it.

But keep in mind, even if you handle the error in the callback, the commit has failed and it's not retried, which means the consumer offset hasn't been updated in Kafka. The consumer will continue to consume messages from the failed offset. In such scenarios, manual intervention or alerts might be necessary to identify the root cause and resolve the issue.

On the other hand, synchronous commits (commitSync) will retry indefinitely until the commit succeeds or encounters a 
non-retryable failure, at which point it throws an exception that your application can catch and handle directly. This is why it's often recommended to have a final synchronous commit when you're done consuming messages.

As a general strategy, it's crucial to monitor your consumers and Kafka infrastructure for such failures and handle them appropriately to ensure smooth data processing and prevent data loss or duplication.

---

***When to use SyncCommit vs AsyncCommit?***

Choosing between synchronous and asynchronous commit in Apache Kafka largely depends on your application's requirements around data reliability and processing efficiency.

Here are some factors to consider when deciding between synchronous and asynchronous commit:

1. **Synchronous commit (commitSync)**: Use it when data reliability is critical, as it retries indefinitely until successful or a fatal error occurs. However, it can block your consumer, slowing down processing speed.

2. **Asynchronous commit (commitAsync)**: Use it when processing speed is important and some data loss is tolerable. It doesn't block your consumer but doesn't retry upon failures.

**Combination**: Many applications use commitAsync for regular commits and commitSync before shutting down to ensure the final offset is committed. This approach balances speed and reliability.


---

***What is Out-of-Order Commit?***


Normally, you might expect offsets to be committed sequentially (e.g., commit for message 1, then 2, then 3, and so on). However, Kafka's design allows for *out-of-order commits*, meaning a consumer can commit a later offset even if earlier messages in the sequence haven't been explicitly committed.

**A Simple Scenario**

Consider a Kafka topic with messages 1, 2, 3, 4, 5, 6, etc..

1.  A consumer polls messages and receives 1, 2, 3, and 4.
2.  However, for some reason, the consumer only commits the offset for message 4 to the `__consumer_offset` topic. It does not send explicit commits for messages 1, 2, or 3.
3.  Then, the consumer goes down.

The Broker's Behavior

When the consumer spins up again, a crucial question arises: Will the Kafka broker re-send messages 1, 2, and 3 (for which no explicit commit was received), or will it start from message 5?

The correct answer is: The Kafka broker will not re-send messages 1, 2, or 3.

 1. The broker simply checks the `__consumer_offset` topic for the latest committed offset for that particular consumer group and topic.
 2. In our scenario, the latest committed offset is for message 4 (which means the next message to read is 5).
 3. The broker will then start sending messages from message 5 onwards (i.e., 5, 6, 7, etc.).
 3. Kafka assumes that all messages prior to the latest committed offset have been successfully processed, even if individual commits for those messages were not received. This committed offset acts like a "bookmark".

This behavior is termed "out-of-order commit" because, ideally, commits should be sequential (1, then 2, then 3, then 4). However, in this scenario, a commit for message 4 is received directly, without commits for messages 1, 2, or 3.

***Advantages of Out-of-Order Commit***

The primary advantage of out-of-order commit is reduced overhead.

 1. Committing an offset for every single message individually can produce a lot of overhead, as it's a complex operation.
 2. Instead, consumers can consume a batch of messages (e.g., 1, 2, 3, 4).
 3. After processing the entire batch, the consumer only needs to commit the offset of the last message in that batch (e.g., message 4).
 4. This way, the entire batch is effectively acknowledged, and the broker will not re-send any messages within that batch, understanding them as successfully processed. This significantly improves efficiency by reducing the number of commit operations.

***Disadvantages of Out-of-Order Commit***

While efficient, out-of-order commit has a significant disadvantage: potential message loss.

 1. Imagine a scenario where a consumer processes messages using multiple threads or a complex backend system.
 2. If messages 1, 2, and 3 fail during processing, but message 4 (which was processed by a separate, successful thread) is committed.
 3. Even though messages 1, 2, and 3 failed, because message 4's offset was committed, the broker will *not re-send* those failed messages when the consumer restarts.
 4. This can lead to *data loss* or inconsistent processing if not handled carefully at the application level.

---

### **Kafka Log Segments**
Kafka Log Segments are a powerful mechanism that allows Kafka to efficiently manage and store vast amounts of streaming data. By breaking down large logs into smaller, configurable segments, Kafka ensures high performance, manageability, and robust data retention policies.

All messages published by producers to a Kafka topic are stored within Kafka logs.

These logs are the primary location where messages reside, playing a vital role in enabling communication between producers and consumers via the Kafka cluster.

Traditionally, one might imagine all messages for a topic's partition being stored in a single, ever-growing log file. However, Kafka takes a more efficient approach:

   1.  Instead of creating one single, large log file for a particular partition, Kafka creates several smaller files to store all messages.
   2. These small, individual files within a partition on a server are called segments.

---

***Why Segments?***

Imagine a very large book that keeps growing infinitely. If you needed to find a specific page, or if the book became corrupted, managing one massive file would be incredibly difficult and inefficient.

Kafka segments address this by:

   1. **Managing Large Volumes of Data**: By breaking down a single massive log into smaller, manageable segments, Kafka can handle terabytes or petabytes of data more effectively.
   2. **Efficient Retention Policies**: Older segments can be easily deleted or archived without affecting the active segments where new messages are being appended.
   3. **Improved Recovery**: In case of corruption or failure, smaller segments are faster to recover or replicate.

***How New Segments are Created?***

Messages are continuously appended to the currently active log segment in a given partition.
Kafka is configured with a maximum size limit for each log segment file.
Once the current segment file reaches this configured size (in bytes), Kafka automatically creates a new, empty log segment file for subsequent messages. This ensures that no single log file becomes excessively large.


***Why Segmentation***
Kafka doesn't write all messages into a single, ever-growing log file. This would become unwieldy and inefficient for operations like deletion or replication. Instead, Kafka divides its log files into multiple smaller segments.

This segmentation is controlled by the `log.segment.bytes` property.
When a log segment reaches a configured size limit (e.g., 2000 bytes as set in a demo), Kafka closes the current segment and starts a new one.
Each segment has its own `.log`, `.index`, and `.timeindex` files.

*File Naming Convention*

   A key pattern to observe in Kafka's segmented logs is how the files are named.

   Each log file, its corresponding `.index` file, and `.timeindex` file within a partition directory will share a common name prefix.
   This prefix is actually the starting offset of the first message contained within that log segment.

   Examples:

   `00000000000000000000.log` indicates the segment starts from `offset 0`.
   `00000000000000000027.log` indicates the segment starts from `offset 27`.
   `00000000000000000090.log` indicates the segment starts from `offset 90`.

   This naming convention is crucial for quickly identifying which segment contains a particular message.

---

***How Lookup Works with Multiple Segments***
   When a consumer requests a message by offset in a multi-segment environment, Kafka follows a three-step process:

   1.  Locate the Segment File (by filename): The Kafka broker first determines which log segment contains the requested offset. It does this by checking the file names in the partition directory. Since file names indicate the starting offset of each segment, the broker can quickly identify the correct `.log` file without opening any files. For example, if `offset 100` is requested, the broker knows it must be in the `00000000000000000090.log` file because messages start from `offset 90` in this segment, and the next segment starts from `offset 109`.
   2.  Lookup in the Segment's `.index` File: Once the correct log segment (`.log` file) is identified, the broker then goes to its corresponding `.index` file (e.g., `00000000000000000090.index`). It performs a binary search within this specific `.index` file to find the nearest offset and its byte `position`.
   3.  Scan within the Segment's `.log` File: Finally, with the approximate byte `position` from the index file, the broker navigates to that position within the actual `.log` file and starts scanning from there to find the exact message(s) requested.

   This multi-step approach ensures that even with hundreds or thousands of gigabytes of messages, Kafka can locate any message with minimal disk I/O and latency.

---

***Dumping and Reading Contents of Log, Index, or TimeIndex Files***
This command helps you view the structured content of these binary files.
```bash
kafka-run-class.bat kafka.tools.DumpLogSegments --files [path_to_log_file.log] --print-data-log

Example for .log file:
kafka-run-class.bat kafka.tools.DumpLogSegments --files C:\kafka\kafka-logs\my-topic-0\00000000000000000000.log --print-data-log

Example for .index file:
kafka-run-class.bat kafka.tools.DumpLogSegments --files C:\kafka\kafka-logs\my-topic-0\00000000000000000000.index --print-data-log

Example for .timeindex file:
kafka-run-class.bat kafka.tools.DumpLogSegments --files C:\kafka\kafka-logs\my-topic-0\00000000000000000000.timeindex --print-data-log
```

---
