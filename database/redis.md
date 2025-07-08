# Redis database

## Overview of the features of a Redis database

| **Redis Feature**                     | **Typical Use Cases**                                                               | **Alternative Solutions**                                                                                                                                                                  | **Evaluation of Alternatives**                                                                                                                                                                                                                                                 |
| ------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **In-Memory Data Storage**            | - Caching frequently accessed data<br>- Session storage<br>- Real-time analytics    | - **Memcached**: A high-performance, distributed memory object caching system.<br>- **Hazelcast**: An in-memory data grid for distributed computing.                                       | - **Memcached**: Offers fast, in-memory caching but lacks Redis's advanced data structures and persistence options.<br>- **Hazelcast**: Provides distributed caching with additional features but comes with increased complexity and resource requirements compared to Redis. |
| **Persistence**                       | - Data durability for recovery<br>- Event sourcing patterns                         | - **Cassandra**: A distributed NoSQL database designed for scalability and high availability.<br>- **MongoDB**: A document-oriented NoSQL database with optional in-memory storage engine. | - **Cassandra**: Ensures data durability and is suitable for large-scale deployments but may introduce latency due to disk I/O operations.<br>- **MongoDB**: Offers flexible data modeling and persistence but may not match Redis's performance for in-memory operations.     |
| **Advanced Data Structures**          | - Implementing queues, stacks, leaderboards<br>- Counting and real-time analytics   | - **Hazelcast**: Supports complex data structures and distributed computing.<br>- **Couchbase**: Combines document and key-value store capabilities.                                       | - **Hazelcast**: Offers advanced data structures with distributed processing but may be more complex to set up and manage.<br>- **Couchbase**: Provides versatile data storage options but may not be as performant as Redis for in-memory operations.                         |
| **Replication and High Availability** | - Ensuring data redundancy<br>- Load balancing read operations across replicas      | - **etcd**: A distributed key-value store providing strong consistency and reliable data distribution.<br>- **Hazelcast**: Offers built-in replication and high availability features.     | - **etcd**: Provides strong consistency and is well-suited for distributed systems coordination but may have higher latency compared to Redis.<br>- **Hazelcast**: Ensures high availability with automatic failover but may require more resources.                           |
| **Clustering and Scalability**        | - Scaling out to distribute data across multiple nodes<br>- Handling large datasets | - **Cassandra**: Designed for horizontal scalability across many nodes.<br>- **MongoDB**: Supports sharding for distributing data across clusters.                                         | - **Cassandra**: Excels in scalability and fault tolerance but may be complex to manage.<br>- **MongoDB**: Offers flexible scalability options but may not achieve the same performance as Redis for in-memory workloads.                                                      |
| **Pub/Sub Messaging**                 | - Real-time messaging systems<br>- Event notification services                      | - **Apache Kafka**: A distributed event streaming platform capable of handling high throughput.<br>- **RabbitMQ**: A message broker supporting multiple messaging protocols.               | - **Apache Kafka**: Suitable for high-throughput messaging but is more complex and has higher latency compared to Redis.<br>- **RabbitMQ**: Provides reliable messaging with support for complex routing but may not match Redis's speed for simple pub/sub use cases.         |
| **Geospatial Indexing**               | - Location-based services<br>- Geofencing applications                              | - **PostGIS**: A spatial database extender for PostgreSQL, offering advanced geospatial capabilities.                                                                                      | - **PostGIS**: Provides comprehensive geospatial features but may not perform as quickly as Redis for in-memory geospatial queries.                                                                                                                                            |

## Redis commands that change the data

| Command          | Description                                   | Example                                                         |
| ---------------- | --------------------------------------------- | --------------------------------------------------------------- |
| SET              | Set the string value of a key                 | `SET mykey "hello"`                                             |
| SETEX            | Set the value and expiration of a key         | `SETEX mykey 10 "value"`                                        |
| PSETEX           | Set the value and expiration in milliseconds  | `PSETEX mykey 1500 "value"`                                     |
| SETNX            | Set value only if key doesn't exist           | `SETNX mykey "value"`                                           |
| APPEND           | Append value to existing string               | `APPEND mykey " world"`                                         |
| MSET             | Set multiple keys at once                     | `MSET key1 "val1" key2 "val2"`                                  |
| MSETNX           | Set multiple keys if none exist               | `MSETNX key1 "val1" key2 "val2"`                                |
| DEL              | Delete one or more keys                       | `DEL key1 key2`                                                 |
| INCR             | Increment value by 1 (string must be integer) | `INCR counter`                                                  |
| INCRBY           | Increment value by specific amount            | `INCRBY counter 5`                                              |
| INCRBYFLOAT      | Increment float value                         | `INCRBYFLOAT temp 0.5`                                          |
| DECR             | Decrement value by 1                          | `DECR counter`                                                  |
| DECRBY           | Decrement value by specific amount            | `DECRBY counter 3`                                              |
| LPUSH            | Push element(s) to head of list               | `LPUSH mylist "a"`                                              |
| RPUSH            | Push element(s) to tail of list               | `RPUSH mylist "b"`                                              |
| LPOP             | Remove and return first element               | `LPOP mylist`                                                   |
| RPOP             | Remove and return last element                | `RPOP mylist`                                                   |
| LSET             | Set value at specific index                   | `LSET mylist 0 "newval"`                                        |
| LINSERT          | Insert element before/after pivot             | `LINSERT mylist BEFORE "a" "x"`                                 |
| LREM             | Remove elements matching value                | `LREM mylist 2 "x"`                                             |
| LTRIM            | Trim list to specified range                  | `LTRIM mylist 0 1`                                              |
| RPOPLPUSH        | Atomically pop tail and push to another list  | `RPOPLPUSH list1 list2`                                         |
| LMOVE            | Move element from one list to another         | `LMOVE list1 list2 LEFT RIGHT`                                  |
| SADD             | Add one or more members to set                | `SADD myset "a" "b"`                                            |
| SREM             | Remove member(s) from set                     | `SREM myset "a"`                                                |
| SPOP             | Remove and return random member               | `SPOP myset`                                                    |
| SMOVE            | Move member from one set to another           | `SMOVE set1 set2 "a"`                                           |
| ZADD             | Add/update member in sorted set               | `ZADD zset 1.0 "a"`                                             |
| ZREM             | Remove member(s) from sorted set              | `ZREM zset "a"`                                                 |
| ZINCRBY          | Increment score of member                     | `ZINCRBY zset 2 "a"`                                            |
| ZPOPMAX          | Remove and return max score member            | `ZPOPMAX zset`                                                  |
| ZPOPMIN          | Remove and return min score member            | `ZPOPMIN zset`                                                  |
| ZREMRANGEBYRANK  | Remove by index range                         | `ZREMRANGEBYRANK zset 0 1`                                      |
| ZREMRANGEBYSCORE | Remove by score range                         | `ZREMRANGEBYSCORE zset 0 10`                                    |
| ZREMRANGEBYLEX   | Remove by lex range                           | `ZREMRANGEBYLEX zset [a [z`                                     |
| HSET             | Set field in hash                             | `HSET user name "Alice"`                                        |
| HSETNX           | Set field only if it doesn't exist            | `HSETNX user age 30`                                            |
| HMSET            | Set multiple fields in hash (deprecated)      | `HMSET user name "Bob" age 25`                                  |
| HDEL             | Delete field(s) from hash                     | `HDEL user name`                                                |
| HINCRBY          | Increment integer field in hash               | `HINCRBY user age 1`                                            |
| HINCRBYFLOAT     | Increment float field in hash                 | `HINCRBYFLOAT user weight 0.5`                                  |
| SETBIT           | Set or clear bit at offset                    | `SETBIT bits 7 1`                                               |
| BITFIELD         | Bitfield operations on string                 | `BITFIELD mykey INCRBY i5 100 1`                                |
| PFADD            | Add element(s) to HyperLogLog                 | `PFADD hll "user1"`                                             |
| XADD             | Add entry to stream                           | `XADD mystream * sensor-id 1234`                                |
| XDEL             | Delete entry from stream                      | `XDEL mystream 1234567890-0`                                    |
| XTRIM            | Trim stream length                            | `XTRIM mystream MAXLEN 1000`                                    |
| XGROUP           | Manage consumer groups                        | `XGROUP CREATE mystream mygroup $`                              |
| XACK             | Acknowledge message in stream                 | `XACK mystream mygroup 1234567890-0`                            |
| XCLAIM           | Claim ownership of message                    | `XCLAIM mystream mygroup consumer 0 1234`                       |
| PUBLISH          | Publish message to a channel                  | `PUBLISH mychannel "hello"`                                     |
| EXPIRE           | Set key TTL in seconds                        | `EXPIRE mykey 60`                                               |
| PEXPIRE          | Set key TTL in milliseconds                   | `PEXPIRE mykey 1500`                                            |
| PERSIST          | Remove TTL from key                           | `PERSIST mykey`                                                 |
| RENAME           | Rename key                                    | `RENAME oldkey newkey`                                          |
| RENAMENX         | Rename key if new key doesn't exist           | `RENAMENX old new`                                              |
| FLUSHDB          | Delete all keys in current DB                 | `FLUSHDB`                                                       |
| FLUSHALL         | Delete all keys in all DBs                    | `FLUSHALL`                                                      |
| MOVE             | Move key to another DB                        | `MOVE mykey 1`                                                  |
| UNLINK           | Delete key asynchronously                     | `UNLINK mykey`                                                  |
| EVAL             | Run Lua script                                | `EVAL "return redis.call('SET', KEYS[1], ARGV[1])" 1 key1 val1` |
| EVALSHA          | Run cached Lua script                         | `EVALSHA <sha1> 1 key1 val1`                                    |
| MULTI            | Start transaction                             | `MULTI`                                                         |
| EXEC             | Execute transaction                           | `EXEC`                                                          |
| DISCARD          | Cancel transaction                            | `DISCARD`                                                       |
| WATCH            | Watch key(s) for changes before transaction   | `WATCH mykey`                                                   |
| UNWATCH          | Unwatch all keys                              | `UNWATCH`                                                       |
