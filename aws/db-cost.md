# Cost of databases on AWS

| DB Type                             | Tier / Instance       | vCPU | Memory (GiB) | Est. Monthly Compute ($) | Rough Requests/sec\* | Notes                                                |
| ----------------------------------- | --------------------- | ---- | ------------ | ------------------------ | -------------------- | ---------------------------------------------------- |
| Relational – RDS PostgreSQL         | db.t4g.small (single) | 2    | 2.0          | ≈ $23.36                 | ~100 – 500           | for light workloads; CPU credit constrained          |
| Relational – RDS PostgreSQL         | db.r6g.large (single) | 2    | 16.0         | ≈ $164.25                | ~500 – 2,000         | depends on query complexity, I/O, caching            |
| Key-value – ElastiCache (Redis OSS) | cache.t4g.small       | 2    | 1.37         | ≈ $23.36                 | ~50,000 – 100,000    | in-memory, simple GET/SET                            |
| Key-value – ElastiCache (Redis OSS) | cache.m7g.large       | 2    | 6.38         | ≈ $59.86                 | ~200,000 – 400,000   | has headroom; depends on latency & client pipelining |
| Search – Amazon OpenSearch Service  | t3.small.search       | 2    | 2.0          | ≈ $26.28                 | ~100 – 300           | small dev node, limited throughput                   |
| Search – Amazon OpenSearch Service  | m7g.large.search      | 2    | 8.0          | ≈ $98.55                 | ~1,000 – 5,000       | depends on shard count, query complexity             |
| Graph – Amazon Neptune              | db.r5.large (single)  | 2    | 16.0         | ≈ $253.56                | ~500 – 2,000         | “simple graph queries” ballpark                      |
| Graph – Amazon Neptune              | db.r5.xlarge (single) | 4    | 32.0         | ≈ $507.12                | ~1,000 – 4,000       | more headroom for multi-hop queries                  |
| Document – Amazon DocumentDB        | db.r5.large (single)  | 2    | 16.0         | ≈ $202.21                | ~300 – 1,200         | simple document reads/writes                         |
| Document – Amazon DocumentDB        | db.r5.xlarge (single) | 4    | 32.0         | ≈ $400.38                | ~800 – 3,000         | depends on index usage, document size                |
