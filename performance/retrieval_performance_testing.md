## Test Cases

| ID | Description | Steps | Test Data | Pre-condition | Expected Output | Passed/Failed |
|---|---|---|---|---|---|---|
| LOC-RT-01 | Retrieval Locust baseline | 1. Start Retrieval service<br>2. Run Locust with 1 user<br>3. Capture text and late-fusion latency | Retrieval query corpus and synthetic image path | Retrieval service is healthy | Text and late-fusion searches complete with 0 failures | Passed |
| LOC-RT-02 | Retrieval Locust concurrent throughput | 1. Start Retrieval service<br>2. Run Locust with 4 users<br>3. Capture throughput and failure rate | Retrieval query corpus | Retrieval service is healthy | Throughput is recorded and failure rate remains near 0% | Passed with note |
| PROM-RT-01 | Retrieval Prometheus metrics snapshot | 1. Run Retrieval Locust with exporter port 9303<br>2. Scrape `/metrics` while Locust is running<br>3. Save metrics snapshot | Locust-generated metrics | Locust exporter is running | Prometheus latency and request-count metrics are captured | Passed |
| PERF-RT-001 | Text search p95 latency | 1. Seed retrieval index<br>2. Send repeated text searches<br>3. Record latency | Seeded product records | Retrieval app and FAISS index are available | Text search p95 is recorded | Passed |
| PERF-RT-002 | Late-fusion p95 latency | 1. Load CLIP model<br>2. Seed text/image products<br>3. Run late-fusion searches | Tiny JPEG and product text | CLIP weights are available | Late-fusion p95 is recorded | Passed |
| PERF-RT-003 | Retrieval throughput observation | 1. Seed index<br>2. Execute 120 requests using 4 workers<br>3. Record RPS and latency | Seeded product records | Retrieval app is running | Requests complete without worker errors | Passed |
| PERF-RT-004 | Retrieval memory footprint | 1. Warm retrieval endpoint<br>2. Sample RSS twice<br>3. Calculate drift | Idle retrieval process | psutil is available | RSS remains below 2 GB and drift below 50 MB | Passed |