# Database implementations

### Database Comparison Table

| Database       | Memory Usage (est.) | CPU Usage (est.) | Written In | Notes                                                           |
| -------------- | ------------------- | ---------------- | ---------- | --------------------------------------------------------------- |
| **MySQL**      | \~200–400 MB        | Low–Moderate     | C, C++     | Stable; efficient under light-to-moderate workloads.            |
| **PostgreSQL** | \~300–600 MB        | Moderate         | C          | Slightly heavier than MySQL; more features and extensibility.   |
| **MariaDB**    | \~200–400 MB        | Low–Moderate     | C, C++     | MySQL fork; lighter in some cases; drop-in MySQL replacement.   |
| **Redis**      | \~50–150 MB         | Low              | C          | In-memory; blazingly fast; excellent for caching, pub/sub, etc. |
| **Memcached**  | \~20–100 MB         | Very Low         | C          | Lightweight caching only; simpler than Redis; memory-bound.     |
| **OpenSearch** | \~1–2 GB+           | High             | Java       | Heavy; search and analytics engine; Elasticsearch fork.         |
| **InfluxDB**   | \~100–300 MB        | Moderate         | Go         | Optimized for time-series data; efficient under regular load.   |
| **Neo4j**      | \~500 MB – 1 GB     | Moderate–High    | Java       | Graph DB; heavier runtime but performant for graph queries.     |

### Technical Modules in Databases

| Module                  | Category       | Problem It Solves                                                            | Complexity      | Used In                                                                |
| ----------------------- | -------------- | ---------------------------------------------------------------------------- | --------------- | ---------------------------------------------------------------------- |
| **Inverted Index**      | Search         | Fast full-text search by mapping terms to document locations                 | ⭐️⭐️⭐️⭐️    | OpenSearch, InfluxDB (limited), Neo4j (for paths), Redis (modules)     |
| **B-Tree / B+Tree**     | Storage Engine | Balanced tree for fast range queries and key lookups                         | ⭐️⭐️⭐️       | MySQL, PostgreSQL, MariaDB                                             |
| **LSM Tree**            | Storage Engine | Write-optimized structure, good for time-series and logs                     | ⭐️⭐️⭐️⭐️    | InfluxDB, OpenSearch, Redis (some variants)                            |
| **Sharding**            | Scalability    | Horizontal partitioning to distribute data across multiple nodes             | ⭐️⭐️⭐️⭐️    | OpenSearch, Redis (Cluster), MongoDB, (partially PostgreSQL via Citus) |
| **Replication**         | Availability   | Keeps data copies in sync across servers for redundancy and load balancing   | ⭐️⭐️⭐️       | MySQL, MariaDB, PostgreSQL, Redis, OpenSearch, InfluxDB                |
| **Query Planner**       | Optimization   | Decides most efficient way to execute a query (e.g. use index vs. full scan) | ⭐️⭐️⭐️⭐️    | PostgreSQL, MySQL, Neo4j, OpenSearch                                   |
| **Write-Ahead Log**     | Durability     | Ensures crash recovery by logging changes before applying to disk            | ⭐️⭐️⭐️       | PostgreSQL, MySQL, InfluxDB, Neo4j                                     |
| **Caching Layer**       | Performance    | Avoids repeated disk reads by storing hot data in memory                     | ⭐️⭐️          | Redis, Memcached, PostgreSQL (shared buffers), MySQL                   |
| **Graph Engine**        | Data Model     | Stores and traverses graph relationships efficiently                         | ⭐️⭐️⭐️⭐️⭐️ | Neo4j                                                                  |
| **Time-Series Engine**  | Data Model     | Stores and queries metrics, aggregates efficiently over time intervals       | ⭐️⭐️⭐️       | InfluxDB                                                               |
| **Document Store**      | Data Model     | Flexible, schema-less data storage (JSON/XML)                                | ⭐️⭐️⭐️       | OpenSearch, Redis (JSON), PostgreSQL (JSONB)                           |
| **Pub/Sub**             | Messaging      | Real-time message broadcasting to subscribers                                | ⭐️⭐️⭐️       | Redis, InfluxDB (Kapacitor/Telegraf)                                   |
| **Columnar Storage**    | Optimization   | Optimized for analytics by storing data by columns, not rows                 | ⭐️⭐️⭐️⭐️    | OpenSearch (Lucene), InfluxDB                                          |
| **Compression**         | Optimization   | Reduces storage size and speeds up I/O                                       | ⭐️⭐️          | All (especially InfluxDB, OpenSearch, Redis)                           |
| **ACID Transactions**   | Consistency    | Guarantees atomicity, consistency, isolation, durability of operations       | ⭐️⭐️⭐️⭐️    | PostgreSQL, MySQL, MariaDB, Neo4j                                      |
| **Concurrency Control** | Consistency    | Manages concurrent access to ensure data integrity                           | ⭐️⭐️⭐️⭐️    | PostgreSQL, MySQL, Neo4j                                               |
