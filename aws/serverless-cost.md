# Relative cost of AWS serverless services

| Service                         | Operation                                | Cost (USD) per 1M | Notes                                                 | Typical Response Time |
| ------------------------------- | ---------------------------------------- | ----------------- | ----------------------------------------------------- | --------------------- |
| Lambda                          | Invoke (128MB, 100ms)                    | \$0.408           | Excludes request cost; billed in 1ms increments       | \~1–100 ms cold start |
| Lambda                          | Invoke (512MB, 1s)                       | \$3.276           | Excludes request cost; higher memory incurs more cost | \~1–100 ms cold start |
| Lambda                          | Request Cost                             | \$0.200           | \$0.20 per 1 million requests                         | \~1–100 ms            |
| S3                              | GET (Read)                               | \$0.400           | Retrieval tier affects pricing                        | \~20–100 ms           |
| S3                              | PUT (Write)                              | \$5.000           | Varies by storage class and region                    | \~20–200 ms           |
| SNS                             | Publish (HTTP/S delivery)                | \$0.500           | Extra for SMS, Lambda, retries                        | \~50–200 ms           |
| CloudWatch Logs                 | PutLogEvents (ingest)                    | \$0.500           | Retention, storage, and export billed separately      | \~100–300 ms          |
| CloudWatch Logs                 | GetLogEvents (read)                      | \$0.010           | Querying and data scans may add cost                  | \~200–500 ms          |
| CloudWatch Metrics              | Custom Metric Writes                     | \$0.300           | Dimensions and retention time add cost                | \~50–200 ms           |
| EventBridge                     | PutEvents                                | \$1.000           | Rules, replay, and archiving are extra                | \~20–50 ms            |
| Step Functions                  | State Transitions (Standard)             | \$25.000          | Long-duration steps and history incur cost            | \~1s+ per transition  |
| Step Functions                  | State Transitions (Express)              | \$1.000           | Pricing increases with payload size                   | \~25–100 ms           |
| DynamoDB                        | Read Request Units (Strongly Consistent) | \$1.250           | Double cost vs eventually consistent reads            | \~5–20 ms             |
| DynamoDB                        | Write Request Units                      | \$6.500           | Storage, backups, and streams are extra               | \~5–20 ms             |
| API Gateway                     | REST API Call                            | \$3,500.000       | Caching, logging, and custom domain add cost          | \~100–300 ms          |
| API Gateway                     | HTTP API Call                            | \$100.000         | Authorizers, throttling, and TLS add cost             | \~20–100 ms           |
| Secrets Manager                 | GetSecretValue                           | \$5.000           | Plus \$0.40 per secret/month storage                  | \~20–200 ms           |
| Systems Manager Parameter Store | GetParameter (Advanced Tier)             | \$5.000           | \$0.05 per parameter/month + request cost             | \~30–200 ms           |
| KMS                             | Encrypt / Decrypt                        | $3.00             | $0.03 per 10K requests; $1/month per key              | ~5–50 ms              |
| SQS                             | Standard Queue Send                      | $0.40             | Charged after 1M free requests; billed per 64KB       | ~10–50 ms             |
| SQS                             | FIFO Queue Send                          | $0.50             | Guarantees order; higher cost                         | ~20–100 ms            |
| EventBridge                     | PutEvents                                | $1.00             | Charged after 100K free events                        | ~20–50 ms             |
