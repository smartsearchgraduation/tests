
## Test Cases

| ID | Description | Steps | Test Data | Pre-condition | Expected Output | Passed/Failed |
|---|---|---|---|---|---|---|
| LOC-BE-01 | Backend Locust baseline | 1. Start Backend, Correction, and Retrieval services<br>2. Run Backend Locust with 1 user<br>3. Capture CSV and HTML report | Backend mixed workload | All services are healthy | Locust completes with 0 failures and records latency | Passed |
| LOC-BE-02 | Backend Locust concurrent observation | 1. Start services<br>2. Run Backend Locust with 4 users<br>3. Capture throughput and latency | Backend mixed workload | All services are healthy | Locust completes and records throughput | Passed |
| PROM-BE-01 | Backend Prometheus metrics snapshot | 1. Run Backend Locust with exporter port 9301<br>2. Scrape `/metrics` while Locust is running<br>3. Save metrics snapshot | Locust-generated metrics | Locust exporter is running | Prometheus latency and request-count metrics are captured | Passed |
| PERF-BE-001 | Search endpoint latency | 1. Send repeated `POST /api/search` requests<br>2. Record p50 and p99 latency | Search query data | Backend test app and database are running | Search responses return successfully and latency is recorded | Passed |
| PERF-BE-002 | Product listing latency | 1. Seed 100 products<br>2. Send repeated `GET /api/products` requests<br>3. Record latency | 100 product records | Products exist in database | Product list returns successfully | Passed |
| PERF-BE-003 | Product creation with image | 1. Upload product with JPEG image<br>2. Record latency | 200 KB JPEG-like image | Upload folder is available | Product is created with image | Passed |
| PERF-BE-004 | Product image update | 1. Seed product with images<br>2. Replace images using `PUT /api/products/<id>`<br>3. Record latency | 200 KB JPEG-like image | Product exists | Product image list is replaced | Passed |
| PERF-BE-005 | Product deletion latency | 1. Seed product<br>2. Delete product<br>3. Verify removal | Product record | Product exists | Product is deleted successfully | Passed |
| PERF-BE-006 | DB fallback versus FAISS | 1. Run FAISS-backed search<br>2. Run DB fallback search<br>3. Compare medians | Product dataset | Search and fallback endpoints exist | FAISS and DB fallback medians are recorded | Passed |
| PERF-BE-007 | File image upload throughput | 1. Upload file images repeatedly<br>2. Record throughput and latency | 500 KB JPEG-like image | Product exists | File upload throughput is recorded | Passed |
| PERF-BE-008 | Base64 image upload throughput | 1. Upload base64 images repeatedly<br>2. Record throughput and latency | Base64 image data | Product exists | Base64 upload throughput is recorded | Passed |
| PERF-BE-009 | Memory usage under repeated search | 1. Record initial RSS<br>2. Execute repeated searches<br>3. Record final RSS | 200 search requests | psutil is available | RSS growth remains below limit | Passed |
| PERF-BE-010 | Connection-pool exhaustion behavior | 1. Execute concurrent search workers<br>2. Record status distribution and errors | Concurrent search requests | Flask test client and SQLite fixture are active | No worker exception and at least one request succeeds | Failed |
| PERF-BE-011 | Correction overhead | 1. Inject 50 ms correction delay<br>2. Compare correction on/off medians | Search query | Correction service is mocked | Median delta reflects correction overhead | Passed |
| PERF-BE-012 | Analytics persistence overhead | 1. Run searches with persistence<br>2. Run searches without SearchTime persistence<br>3. Compare medians | 30 requests per batch | SearchTime table exists | Persistence overhead is recorded | Passed |
| PERF-BE-013 | Cached search retrieval latency | 1. Seed cached search with 20 products<br>2. Send repeated `GET /api/search/<id>` requests | Cached search record | SearchQuery and Retrieve rows exist | Cached result returns successfully | Passed |
