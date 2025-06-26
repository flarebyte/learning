# Database implementations

### Database Comparison Table

Assuming:

- 5 concurrent connections, ~100 req/s peak

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

### 💰 Monthly Cost Estimates

Assuming:

- 5 concurrent connections, ~100 req/s peak
- **db.t3.medium (2 vCPU, 4 GB RAM)** for RDS and the similar **t3.medium** for EC2/ECS.

| Database       | Managed                                                              | EC2 (Self‑managed)                           | ECS (Docker‑Cloud)            |
| -------------- | -------------------------------------------------------------------- | -------------------------------------------- | ----------------------------- |
| **MySQL**      | \$34 + storage & I/O (\~+ \$10) ≈ **\$44**                           | \$30.40 + EBS (\~\$5) = **\$35.40**          | \~\$42 (x1.2 \* EC2)          |
| **MariaDB**    | Similar to MySQL ≈ **\$44**                                          | **\$35.40**                                  | **\$42**                      |
| **PostgreSQL** | \~10% higher: \$37 + \$10 = **\$47**                                 | **\$35.40**                                  | **\$42**                      |
| **Redis**      | ElastiCache Redis (\~\$0.031/hr principal) → \~\$23/mo               | EC2 with Redis: **\$35.40**                  | Docker Redis on ECS: **\$42** |
| **Memcached**  | ElastiCache Memcached \~ \$0.025/hr → **\$18**                       | EC2 memcached: **\$35.40**                   | ECS memcached: **\$42**       |
| **OpenSearch** | Amazon OpenSearch Service (t3.medium.data) \~\$0.08/hr → **\$58/mo** | EC2: **\$35.40** + EBS (+\$10) = **\$45.40** | **\$54**                      |
| **InfluxDB**   | RDS not supported; EC2: **\$35.40** + EBS: \~\$45.40                 | Same as EC2                                  | **\$54**                      |
| **Neo4j**      | RDS not supported; EC2: **\$35.40** + EBS (+\$10) = **\$45.40**      | Same as EC2                                  | **\$54**                      |

### ⚡ Estimated Response Time Comparison (in milliseconds)

Assuming:

- All services are in the **same AWS region & VPC** with **security best practices** (VPC networking, IAM, encryption in transit).
- Simple query = basic key/value get or indexed `SELECT`.

| Database       | EC2 → DB   | ECS → DB   | Lambda → DB (warm) | Lambda → DB (cold) | Notes                                                |
| -------------- | ---------- | ---------- | ------------------ | ------------------ | ---------------------------------------------------- |
| **MySQL**      | \~1–3 ms   | \~1–3 ms   | \~3–6 ms           | \~200–600 ms       | Fast over TCP; Lambda cold start hurts               |
| **PostgreSQL** | \~1–4 ms   | \~1–4 ms   | \~4–8 ms           | \~200–600 ms       | TLS handshake + JDBC adds minor overhead             |
| **MariaDB**    | \~1–3 ms   | \~1–3 ms   | \~3–6 ms           | \~200–600 ms       | Same TCP behavior as MySQL                           |
| **Redis**      | \~0.5–1 ms | \~0.5–1 ms | \~1–3 ms           | \~200–500 ms       | Extremely low latency in warm path                   |
| **Memcached**  | \~0.5–1 ms | \~0.5–1 ms | \~1–3 ms           | \~200–500 ms       | Minimal protocol overhead                            |
| **OpenSearch** | \~5–10 ms  | \~5–10 ms  | \~10–15 ms         | \~250–700 ms       | JSON/HTTP overhead makes it slower                   |
| **InfluxDB**   | \~2–4 ms   | \~2–4 ms   | \~4–7 ms           | \~250–700 ms       | Fast if data is in memory; depends on storage engine |
| **Neo4j**      | \~4–7 ms   | \~4–7 ms   | \~6–10 ms          | \~300–800 ms       | Bolt protocol adds some latency                      |
