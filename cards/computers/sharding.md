C: [Partitioning] divides a dataset into smaller pieces, within one server or across servers.

C: Horizontal partitioning splits a table by [rows].

C: Vertical partitioning splits a table by [columns], retaining a key to reconnect them.

Q: In a row-oriented database, why separate large, rarely read columns from frequently read ones?
A: To reduce data read by common queries.

C: [Sharding] is horizontal partitioning across multiple machines.

Q: What two placement decisions do you make when sharding data?
A: Choose a shard key and a strategy for mapping keys to shards.

Q: What's a shard key?
A: The field or fields used to determine a record's shard.

Q: Why is a boolean field usually a poor shard key on its own?
A: Only two values limit how finely data can be distributed.

C: A shard key's number of distinct values is its [cardinality].

Q: Why can a high-cardinality tenant ID still produce a hot shard?
A: One tenant may dominate the data or traffic.

Q: Why should a shard key align with common queries?
A: So those queries can target fewer shards.

Q: How does range based sharding work?
A: Groups records by a contiguous range of values.

Q: Which queries benefit most from range-based sharding?
A: Range scans on the shard key.

Q: Why can range sharding by increasing timestamps create a write hotspot?
A: New writes concentrate in the newest range.

C: Hash-based sharding places records using a [hash of the shard key].

Q: Why can hashing reduce write hotspots from sequential shard keys?
A: It scatters neighboring keys across shards.

Q: Why can hashing the shard key make range queries more expensive?
A: Neighboring key values may land on different shards.

Q: What should guide the choice of sharding strategy?
A: Query patterns and data/load distribution.

Q: What is directory based sharding?
A: A lookup maps shard keys or key ranges to shards.

Q: What makes directory-based sharding flexible when moving data?
A: Placement can change by updating the mapping after migration.

Q: How can directory-based sharding avoid a remote lookup for every request?
A: Cache the routing map.

Q: After moving data between shards, what must happen to cached routing entries?
A: Refresh or invalidate stale mappings.

Q: Why replicate the routing directory in directory-based sharding?
A: To keep routing available if one directory node fails.

Q: What are three common sharding strategies?
A: Range-based, hash-based, and directory-based.

Q: What does isolating a hot tenant on a dedicated shard accomplish?
A: Protects other tenants from its load.

Q: How can a compound shard key spread one tenant's data across shards?
A: Add a varying field so subsets can be placed separately.

Q: Why might `(user_id, date)` fail to spread a hot user's current writes?
A: All today's writes for that user still share one key.

Q: When can splitting a hot shard spread its load across servers?
A: When hot records span separable key ranges that can be moved to other servers.

Q: Why can a cross-shard query cost more than a single-shard query?
A: It contacts multiple shards and combines their results.

Q: How can caching reduce the cost of repeated cross-shard queries?
A: Reuse cached results instead of querying the shards again.

Q: How can denormalization reduce cross-shard joins?
A: Store copies of frequently joined data together on the same shard.

Q: What consistency cost does denormalization introduce?
A: Keeping duplicated values up to date.

Q: What data placement choice can avoid a cross-shard transaction?
A: Keep records updated together on the same shard.

Q: What can provide all-or-nothing commits across shards when the database supports it?
A: A distributed transaction protocol, such as two-phase commit.

Q: What is the saga pattern?
A: A sequence of local transactions with compensating actions for failures.

Q: What's a compensating action in a saga?
A: A new operation that counteracts an earlier committed step, such as a refund.

Q: Why doesn't a saga automatically provide isolation across its steps?
A: Other operations can observe intermediate committed changes.

Q: What if a saga's compensating action fails?
A: Retry or reconcile it; consistency isn't restored automatically.
