# AWS Kinesis

## Features

| **Feature**                      | **Kinesis**                                       | **MSK (Kafka)**                             |
| -------------------------------- | ------------------------------------------------- | ------------------------------------------- |
| **Managed service**              | ✅ Fully managed                                  | ✅ Managed (you manage topics, scaling)     |
| **Custom consumer applications** | ✅ KCL, SDKs                                      | ✅ Kafka Consumer API                       |
| **Durability**                   | ✅ 7 days (default), up to 1 year                 | ✅ Configurable (log retention)             |
| **Ordering guarantees**          | ✅ Per-shard                                      | ✅ Per-partition                            |
| **Exactly-once semantics**       | ⚠️ No native exactly-once (at-least-once default) | ✅ With idempotent producers + transactions |
| **Auto-scaling**                 | ✅ With Kinesis On-Demand                         | ⚠️ Manual or external tool needed           |
| **Data replay**                  | ✅ Up to 1-year retention replayable              | ✅ Replay via offset                        |
| **Schema registry**              | ⚠️ Not native (use Glue/other registry)           | ✅ With Confluent/Schema Registry           |
| **Compression**                  | ⚠️ Not native (producer must compress)            | ✅ Built-in (Snappy, GZIP, etc.)            |
| **Throughput scaling**           | ✅ Via shards (manual or On-Demand)               | ✅ Via partitions                           |
| **Latency**                      | ✅ Low (\~70ms+)                                  | ✅ Very low (\~few ms)                      |
| **Partitioning**                 | ✅ By shard (partition key)                       | ✅ By topic partition                       |
| **Client libraries**             | ✅ AWS SDK, KCL                                   | ✅ Rich Kafka ecosystem                     |
| **Data transformation**          | ✅ With Lambda, Kinesis Analytics                 | ✅ Kafka Streams, Flink, etc.               |
| **Integration with AWS**         | ✅ Deep (IAM, CloudWatch, etc.)                   | ⚠️ Basic (IAM only via MSK IAM auth)        |
| **Multi-region replication**     | ⚠️ Not native                                     | ✅ With Kafka MirrorMaker                   |
| **Cost**                         | ✅ Pay-per-usage                                  | ⚠️ Pay for brokers (EC2-style billing)      |

## Cost

**Assumptions:**

- **Record size**: Assume 1 KB per record.
- **Total data**: 1 million records × 1 KB = **1 GB** per month.
- **Put payload units (PUs)**: Each 25 KB counts as 1 PU. So 1 KB = **1 PU per record**.

**Kinesis Data Streams Pricing (as of 2024):**

| **Item**                        | **Unit Price**              | **Usage**                | **Monthly Cost**              |
| ------------------------------- | --------------------------- | ------------------------ | ----------------------------- |
| **PUT Payload Units**           | \$0.014 per million PUs     | 1M PUs                   | **\$0.014**                   |
| **Shard Hour (optional)**       | \$0.015 per shard-hour      | Assume 1 shard, 24×30 hr | \$10.80                       |
| **Data retrieval (GetRecords)** | \$0.013 per million records | 1M                       | **\$0.013**                   |
| **Total (min cost)**            |                             |                          | **\~\$0.027** (no shard cost) |
| **Total (realistic min)**       |                             |                          | **\~\$10.84** (with 1 shard)  |
