# Relative cost of AWS serverless services

| Service                         | Operation                                | Cost (USD) per 1M | Notes                                                                  | Typical Response Time |
| ------------------------------- | ---------------------------------------- | ----------------- | ---------------------------------------------------------------------- | --------------------- |
| Lambda                          | Invoke (128MB, 100ms)                    | $0.408            | Price scales with memory and duration; excludes request costs.         | ~1–100 ms cold start  |
| Lambda                          | Invoke (512MB, 1s)                       | $3.276            | Higher memory and duration; billed in 1ms increments.                  | ~1–100 ms cold start  |
| S3                              | GET (Read)                               | $0.400            | Cost applies to standard retrievals; frequent reads can add up.        | ~20–100 ms            |
| S3                              | PUT (Write)                              | $5.000            | Higher than GET; costs vary by storage class and region.               | ~20–200 ms            |
| SNS                             | Publish (HTTP/S delivery)                | $0.500            | Extra costs for delivery retries, protocols (e.g., SMS).               | ~50–200 ms            |
| CloudWatch Logs                 | PutLogEvents (ingest)                    | $0.500            | Cost based on ingestion; retention and storage incur extra fees.       | ~100–300 ms           |
| CloudWatch Logs                 | GetLogEvents (read)                      | $0.010            | Querying large logs can become costly; retrievals charged per request. | ~200–500 ms           |
| CloudWatch Metrics              | Custom Metric Writes                     | $0.300            | **Each metric dimension adds cost**; standard metrics are free.        | ~50–200 ms            |
| EventBridge                     | PutEvents                                | $1.000            | Additional costs for rules, archive, or replay usage.                  | ~20–50 ms             |
| Step Functions                  | State Transitions (Standard)             | $25.000           | Suitable for long-running workflows; higher durability and cost.       | ~1s+ per transition   |
| Step Functions                  | State Transitions (Express)              | $1.000            | Cheaper but optimized for short-lived, high-volume events.             | ~25–100 ms            |
| DynamoDB                        | Read Request Units (Strongly Consistent) | $1.250            | Strong consistency doubles the cost vs eventually consistent reads.    | ~5–20 ms              |
| DynamoDB                        | Write Request Units                      | $6.500            | Writes are more expensive; batch operations may reduce cost.           | ~5–20 ms              |
| API Gateway                     | REST API Call                            | $3,500.000        | Very high; **includes caching**, stages; best for full-featured APIs.  | ~100–300 ms           |
| API Gateway                     | HTTP API Call                            | $100.000          | Cheaper, faster; limited features vs REST API.                         | ~20–100 ms            |
| Secrets Manager                 | GetSecretValue                           | $400.000          | $0.40 per 10,000 API calls; also $0.40 per secret per month storage.   | ~20–200 ms            |
| Systems Manager Parameter Store | GetParameter (Advanced Tier)             | $500.000          | Advanced parameters cost $0.05 each/month plus per-request fees.       | ~30–200 ms            |
