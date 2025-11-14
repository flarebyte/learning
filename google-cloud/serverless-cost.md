# Google cloud serverless cost

## **Ⅰ. Compute & Application Hosting**

| Service                           | Operation        | Cost per 1M (approx)                 | Notes                                 | Typical Response Time              |
| --------------------------------- | ---------------- | ------------------------------------ | ------------------------------------- | ---------------------------------- |
| **Cloud Functions (1st/2nd Gen)** | Invocation       | ~$0.40 per 1M                        | CPU/memory billed separately; 2M free | 50–200ms warm; cold starts vary    |
| **Cloud Run (Fully Managed)**     | HTTP requests    | ~$0.40 per 1M (request portion only) | CPU/memory billed per second          | 10–50ms warm; cold starts possible |
| **App Engine Standard**           | HTTP requests    | No per-1M pricing                    | Instance-hour pricing                 | ~100ms typical                     |
| **Cloud Run Jobs**                | Job execution    | No per-1M                            | Billed by CPU/memory/time             | Depends on job duration            |
| **Firebase Hosting**              | CDN HTTP request | Typically negligible/request         | Billed on storage + egress            | <50–100ms globally via CDN         |

---

## **Ⅱ. Messaging, Events & Orchestration**

| Service                      | Operation           | Cost per 1M                             | Notes                             | Typical Response Time    |
| ---------------------------- | ------------------- | --------------------------------------- | --------------------------------- | ------------------------ |
| **Cloud Pub/Sub**            | Message delivery    | Not per-1M (charged per TiB throughput) | SNS/SQS equivalent                | Milliseconds             |
| **Cloud Tasks**              | Task enqueue        | ~$0.40 per 1M                           | Similar to SQS with HTTP callback | Fast enqueueing          |
| **Eventarc**                 | Event routing       | ~$0.50 per 1M events                    | Triggers Cloud Run/Functions      | Real-time routing        |
| **Cloud Scheduler**          | Cron job trigger    | Not per-1M; monthly per job             | EventBridge schedule equivalent   | Minutes/seconds accuracy |
| **Cloud Workflows**          | Orchestration steps | ~$10 per 1M steps                       | Step Functions equivalent         | Step-dependent           |
| **Firebase Cloud Messaging** | Push notifications  | Free                                    | For mobile/web messaging          | Fast global fan-out      |

---

## **Ⅲ. Storage & Databases**

| Service                             | Operation        | Cost per 1M                                | Notes                          | Typical Response Time |
| ----------------------------------- | ---------------- | ------------------------------------------ | ------------------------------ | --------------------- |
| **Cloud Storage (S3 equivalent)**   | Object ops       | Operations are per-10k; not per-1M         | Storage per-GB, ops very cheap | Low ms                |
| **Cloud Firestore (Native)**        | Reads/Writes     | Reads ~$0.30 per 1M; Writes ~$0.90 per 1M  | DynamoDB-like                  | Very low latency      |
| **Cloud Datastore (Legacy)**        | Read/write       | Similar to Firestore pricing               | Being phased into Firestore    | Low ms                |
| **Firebase Realtime Database**      | Data operations  | Not priced per-1M; billed by GB downloaded | Real-time sync database        | Sub-100ms updates     |
| **BigQuery**                        | Query            | Not per-1M; per-TB scanned                 | Serverless analytic warehouse  | Seconds to minutes    |
| **Bigtable**                        | Read/write       | No per-1M; node-hour pricing               | Low-latency NoSQL              | <10ms typically       |
| **AlloyDB Omni / Serverless**       | SQL queries      | Not per-1M                                 | Postgres-compatible            | Milliseconds          |
| **MemoryStore for Redis/Memcached** | Cache operations | Not per-1M; per-node hour                  | Fully managed in-memory cache  | Sub-millisecond       |

---

## **Ⅳ. Identity, Secrets & Security**

| Service                                | Operation         | Cost per 1M                 | Notes                      | Typical Response Time |
| -------------------------------------- | ----------------- | --------------------------- | -------------------------- | --------------------- |
| **Secret Manager**                     | Access secret     | ~$0.03 per 10k (~$3 per 1M) | Secure secret storage      | Very low latency      |
| **IAM (Identity & Access Management)** | Policy checks     | Free                        | Serverless access control  | Milliseconds          |
| **Cloud KMS**                          | Crypto operations | ~$0.03 per 10k (~$3 per 1M) | Key management and signing | Low ms                |

---

## **Ⅴ. Networking, API & Edge**

| Service                   | Operation       | Cost per 1M                              | Notes                     | Typical Response Time |
| ------------------------- | --------------- | ---------------------------------------- | ------------------------- | --------------------- |
| **API Gateway**           | API calls       | ~$3 per 1M                               | Serverless API front end  | Low ms                |
| **Cloud CDN**             | CDN request     | Fractional per-1M; mostly egress charges | Global edge caching       | <50ms globally        |
| **Serverless VPC Access** | Connector use   | No per-1M; per-instance hour             | Connect serverless to VPC | N/A                   |
| **Cloud Load Balancing**  | HTTP(S) request | ~fractions of a cent per 10k             | Global L7 load balancer   | Very low latency      |

---

## **Ⅵ. DevOps, CI/CD, Logging & Monitoring**

| Service               | Operation     | Cost per 1M                   | Notes                        | Typical Response Time  |
| --------------------- | ------------- | ----------------------------- | ---------------------------- | ---------------------- |
| **Cloud Build**       | Build minutes | Not per-1M                    | Container/serverless builds  | Seconds to minutes     |
| **Artifact Registry** | Storage + ops | No per-1M                     | Stores containers/packages   | Very fast              |
| **Cloud Logging**     | Log writes    | Per-GB ingest                 | Serverless logging           | Real-time              |
| **Cloud Monitoring**  | Metric writes | Very low per-1M               | Observability system         | Real-time              |
| **Error Reporting**   | Error event   | Free for typical workloads    | Auto-groups unhandled errors | Real-time              |
| **Cloud Trace**       | Trace spans   | Free for most; storage billed | Distributed tracing          | Low latency            |
| **Cloud Profiler**    | Samples       | Free                          | CPU/memory profiling         | As background sampling |

---

## **Ⅶ. AI / ML Serverless (Fully Managed)**

| Service                       | Operation             | Cost per 1M                                                 | Notes                       | Typical Response Time |
| ----------------------------- | --------------------- | ----------------------------------------------------------- | --------------------------- | --------------------- |
| **Vertex AI Predictions**     | Prediction request    | Not per-1M; per-node-hour or per-request depending on model | Serverless ML inference     | Milliseconds–seconds  |
| **Vertex AI Matching Engine** | Vector search queries | Not per-1M                                                  | High-performance ANN search | Milliseconds          |
| **Vertex AI Workbench**       | Interactive compute   | Not per-1M                                                  | Not strict serverless       | N/A                   |
