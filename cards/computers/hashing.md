Q: What is the main reason to use consistent hashing?
A: Minimizes key remapping when servers join or leave.

Q: Why does hash(key) % N cause trouble when the server count changes?
A: Changing N remaps most keys.

Q: On a consistent hash ring, which node owns a key?
A: The first node at or clockwise after the key's hash, wrapping around.

Q: In a consistent hash ring, what keys does a newly inserted node take over?
A: Keys between its predecessor and itself, previously owned by successor.

Q: When a node leaves a consistent hash ring, who takes over its key range?
A: Next surviving clockwise successor.

C: With balanced, equal capacity nodes, adding one node to N nodes remaps roughly [1/(N+1)] of the keys.

Q: What's a virtual node in consistent hashing?
A: One of several ring positions assigned to a physical server.

Q: Why use multiple virtual nodes per physical server?
A: To spread ownership more evenly across servers.

Q: Why don't virtual nodes solve a single hot key?
A: That key still maps to one owner.

Q: Does consistent hashing provide replication or read/write consistency?
A: No.

Q: When consistent hashing changes ownership, what must a durable store handle beyond routing?
A: Moving existing data while preserving concurrent reads and writes.
