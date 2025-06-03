# Relative cost of AWS serverless services

| Service            | Operation                                | Cost (USD) per 1M |
| ------------------ | ---------------------------------------- | ----------------- |
| Lambda             | Invoke (128MB, 100ms)                    | $0.408            |
| Lambda             | Invoke (512MB, 1s)                       | $3.276            |
| S3                 | GET (Read)                               | $0.400            |
| S3                 | PUT (Write)                              | $5.000            |
| SNS                | Publish (HTTP/S delivery)                | $0.500            |
| CloudWatch Logs    | PutLogEvents (ingest)                    | $0.500            |
| CloudWatch Logs    | GetLogEvents (read)                      | $0.010            |
| CloudWatch Metrics | Custom Metric Writes                     | $0.300            |
| EventBridge        | PutEvents                                | $1.000            |
| Step Functions     | State Transitions (Standard)             | $25.000           |
| Step Functions     | State Transitions (Express)              | $1.000            |
| DynamoDB           | Read Request Units (Strongly Consistent) | $1.250            |
| DynamoDB           | Write Request Units                      | $6.500            |
| API Gateway        | REST API Call                            | $3,500.000        |
| API Gateway        | HTTP API Call                            | $100.000          |
