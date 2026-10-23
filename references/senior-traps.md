# Senior traps checklist

Use these to generate follow-up questions and to judge whether an answer reaches senior depth. Pick the ones that fit the role and stack; don't recite the list.

## System design
- Patterns before path; no sizing; over-engineering small volumes.
- Sync vs async: return 202 + job/order ID, then poll or webhook.
- Direction of connections: sensitive sites pull outbound-only; never expose them to customers.
- Idempotency = conditional write (compare-and-set), plus idempotency keys to external providers.
- Lease + heartbeat to detect dead workers; retry limit + dead-letter queue for poison tasks.
- Physical or external capacity can't be scaled: rate limit with headroom, prioritize, shed load.
- Ownership during network partitions: one owner per task, no blind redispatch, reconcile on reconnect.
- Strong consistency for allocation (seats, inventory), eventual consistency for display counters.
- Edge: static pages on CDN, polling with jitter, bots limited per account, not per IP.

## Reliability and observability
- "Up" is not "working": alert on user symptoms (oldest item age, p99, throughput per worker), not process liveness.
- Label cardinality; percentiles via histograms, not averages.
- HA Prometheus + Alertmanager dedup; dead man's switch to an external service.
- Actionable alerts only; SLO burn-rate alerting.
- Metrics say *that*, traces say *where*, logs say *why*; link them with the trace ID.

## Identity and security
- No long-lived keys. Workload Identity needs both the KSA annotation and the IAM binding (`workloadIdentityUser` on namespace/KSA).
- Signed URLs instead of exposing storage; authorization in one place (the API).
- Leaked key: disable the key (not the account), audit logs for created resources AND IAM changes (persistence), org policy to block key creation.
- TLS vs mTLS; zero trust inside private networks; service-to-service authorization policies.

## Networking
- Private nodes; Cloud NAT is outbound only; Private Google Access / Private Service Connect for cloud APIs; LB + WAF for inbound.
- DNS: resolver → root → TLD → authoritative, TTL caching. Then TCP → TLS → HTTP → load balancer → backend.

## Kubernetes
- Pending (no room) vs CrashLoopBackOff (container exits); `describe` for exit code, `logs --previous`.
- Rolling update waits for Ready, so old pods keep serving when new ones crash.
- Empty endpoints = selector mismatch or failing readiness; then targetPort, NetworkPolicy, app bound to localhost.
- CPU throttling hidden by averages; JVM off-heap memory (use `MaxRAMPercentage`).
- Pause pods with negative priority for over-provisioning; scheduled scaling for predictable peaks.
- Upgrades: deprecated APIs, one minor version at a time, staging first.

## Terraform and delivery
- Remote state with locking; apply only from CI; split state by environment and layer; scheduled drift detection.
- Canary with automated analysis and rollback.
- Schema changes via expand/contract so every step stays rollback-safe.
