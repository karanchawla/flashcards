Q: Across different kafka consumer groups, how many groups can consume the same topic-partition?
A: Many groups. Each group maintains its own independent offset.

Q: Does kafka provide built in consumer retries in the same way some queues do?
A: No. Implement using retry topics or DLQ.

Q: How are old kafka records eventually removed?
A: Retention policies or log compaction.

Q: How can a poorly chosen partition key create a hot partition in kafka?
A: High volume topics share the same key, they hash to the same partition.

Q: What's one solution for hot partitions in kafka?
A: Compound keys.

Q: How can compound keys reduce hot partitions?
A: Combining primary entity with another varying attribute such as region, user segment spreads traffic.

Q: How do kafka consumers use offsets?
A: Record progress; restart from committed offsets.

Q: Four ways to handle consumer failures in kafka?
A:
- Commit offsets after durable processing
- Idempotent consumers
- Allow rebalancing of consumers in a group
- Use retry/DLQ topics for poison messages

Q: How do you scale kafka?
A: Estimate load → choose enough partitions → add brokers → pick a balanced partition key, and mitigate hot partitions.

Q: How does adding brokers scale kafka?
A: Increases aggregate storage, serving capacity and fault tolerance.

Q: How does batching improve kafka performance?
A: Amortizes network and protocol overhead.

Q: How does compression improve kafka overhead?
A: Fewer bytes on the network and disk; higher CPU cost.

Q: How does kafka avoid message loss?
A:
- Partition replication
- Configuring kafka with `ack = all`
- Sufficient in-sync replicas
- Durable offset commits
- Idempotent consumers

Q: How does kafka choose a partition when a message has a key?
A: `hash(key)` modulo number of partitions.

Q: How does using no key help with hot partitions in kafka?
A: Uniform distribution across partitions.

Q: How much roughly can a single kafka broker store?
A: ~1 TB.

Q: In kafka, difference between adding consumers to a group vs adding consumer groups?
A:
- Add consumers to a group → increase parallelism
- Add consumer groups → fan-out to independent jobs

Q: Practical difference between using kafka as a stream vs a queue?
A:
- As a queue: one consumer group divides work
- As a stream: retained logs can be replayed and read independently by multiple groups

Q: Two common roles kafka plays in distributed systems?
A: Message queue or event stream / distributed commit log.

Q: What bounds useful consumer parallelism for one topic within one kafka consumer group?
A: Number of partitions.

Q: What can go wrong if related ordered events are randomly spread across kafka partitions?
A: Order is lost → consumers may observe impossible/illogical message orderings.

Q: Compression algorithms used by kafka?
A: Gzip, Snappy, LZ4.

Q: What determines ordering inside a kafka partition?
A: Offsets. Consumers read based on append order.

Q: What does "kafka is always available, sometimes consistent" mean?
A: Favors high availability through replication (partition replicas) and failover (leader promotion). Consistency depends on replica sync, ack, and failure timing.

Q: What does a replication factor of 3 mean in kafka?
A: Each partition: 1 leader, 2 followers.

Q: What does an idempotent kafka producer protect against?
A: Duplicate messages caused by producer retries for the same send attempt.

Q: What does kafka do when a producer sends records without keys?
A: Round robin.

Q: What does kafka's controller manage?
A: Cluster metadata and leadership changes.

Q: What does producer setting `acks=all` mean?
A: Producer receives ack only after all in-sync replicas have received the record.

Q: What does leader replica do in kafka?
A: Handles writes for the partition and usually serves reads.

Q: What failure causes kafka's default at least once behavior to reprocess a message?
A: Consumer dies after processing a message but before committing an offset.

Q: What fields can a kafka record contain?
A: Headers / Key / Value / Timestamp.

Q: What happens if there are more consumers than partitions in a kafka consumer group?
A: Consumers sit idle.

Q: What happens when a consumer in kafka consumer group fails?
A: Rebalances partitions among remaining consumers.

Q: What hash function does kafka commonly use by default for key hashing?
A: Murmur2.

Q: What's a kafka consumer group?
A: Consumers in a group that process topic partitions. One consumer processes one (topic, partition) in a group.

Q: What's a kafka offset?
A: `id` representing a record's position within a partition.

Q: What's an in-sync replica in kafka?
A: Replica sufficiently caught up to the leader and eligible to satisfy strong ack / leader failover.

Q: What's a hot partition in kafka?
A: A partition receiving disproportionate traffic, becoming the topic's bottleneck.

Q: What log design strategy kafka uses?
A: Append only logs.

Q: What's kafka's consumer model: push or pull?
A: Pull based: consumers poll brokers at a rate they control.

Q: What's kafka's replication model for partitions?
A: Leader–follower. 1 leader, one or more replicas.

Q: What is random salting for kafka partition keys?
A: Adding a random / time varying component to a key so one logical entry spreads across multiple partitions.

Q: Difference between kafka topic and partition?
A:
- Topic: Logical grouping of data
- Partition: Physical ordered log storing part of that data

Q: What is the downside of random salting of keys in kafka?
A: Complicates consumer side aggregation and weakens per entity ordering guarantees.

Q: What is the durability trade off of weaker kafka producer acks?
A: Higher throughput at the risk of message loss.

Q: What's the kafka record key used for?
A: Partition selection, keeping related records together and preserving per-key ordering.

Q: Main downside of sending kafka records without keys?
A: Losing per-entity ordering.

Q: What's the preferred pattern for using kafka with large files?
A: Store large files in blob storage; put their location/ID in kafka messages.

Q: What's the role of kafka record headers?
A: Carry metadata as KV pairs similar to HTTP headers.

Q: What's the rough single broker kafka throughput?
A: Approximately 1000 MiB/sec on 8xlarge machines.

Q: What message size guideline is useful for kafka in system design?
A: Keep messages under 1 MB.

Q: What ordering guarantee does kafka provide?
A: Messages are ordered within a partition, not across partitions.

Q: What performance knob often matters more than micro optimizations to kafka settings?
A: Choosing the partition key correctly.

Q: What performance optimizations you can make in kafka?
A:
1. Batch messages in the producer
2. Compress messages
3. Rethink message key
4. Producer backpressure

Q: What scaling strategy to first focus on when thinking about kafka design?
A: Partition strategy: partition key and hot partition mitigation.

Q: What trade off comes with increasing kafka retention?
A: Longer replay window and higher storage cost.

Q: What's the tradeoff when deciding when a kafka consumer should commit offsets?
A:
- Early → Risk losing work
- Late → Risk duplicate processing after failure

Q: When is kafka useful in preserving ordering?
A: When related events are keyed to the same partition.

Q: When might SQS be preferable to kafka for a work queue?
A: When you require built in retries and DLQ behavior.

Q: When to use kafka?
A: **A FISH**
- Async work
- Fan out
- In order message processing
- Streaming at scale
- Handoff/decoupling

Q: Why are partitions central to kafka scalability?
A: Spread a topic's data across brokers for parallel consumption.

Q: Why does `acks=all` improve kafka durability?
A: Reduces chance of an ack'd message disappearing after leader failover.

Q: Why does append only design help kafka perf?
A: Fewer random disk seeks → data locality. Simpler sequential I/O, batching, replication, and recovery.

Q: Why does kafka use pull based consumer model?
A: Lets consumers control rate to avoid being overwhelmed and simplify failure handling.

Q: Why might adding kafka brokers fail to improve throughput?
A: Under-partitioned topics lack enough partitions to spread load across new brokers.

Q: Why should kafka consumers do small units of work?
A: Less work to redo after a crash before committing offsets.

Q: Within one kafka consumer group, how many consumers can actively consume the same topic-partition?
A: At most one: prevents duplicate processing.

C: [Partitions] are kafka's unit of parallelism and ordering.
