# Runbook: High Latency (Sample App)

**Alert:** `HighLatency`
**Severity:** Warning
**SLO Target:** p95 latency < 200ms

## Overview
This alert triggers when the 95th percentile of request latency for the Sample App exceeds 200ms over a 5-minute window.

## Initial Diagnosis
1.  **Check Metrics in Grafana:**
    Navigate to the Sample App dashboard (Port 3001) and identify if the latency spike correlates with:
    - Increase in request rate (Traffic spike)
    - Resource saturation (CPU/Memory)
2.  **Check Host Performance:**
    Run `top` or `htop` on the host server to check for CPU/Memory bottlenecks or high I/O wait.
3.  **App Logs:**
    Look for slow request logs or timeout errors in `sudo docker logs sample-app`.

## Mitigation Steps
1.  **Check for "Noisy Neighbors":**
    Identify other containers or processes on the host that might be consuming resources.
2.  **Optimize Resources:**
    If the app is CPU-bound, consider adjusting Docker resource limits or scaling the instance.
3.  **Application Profiling:**
    If traffic is normal but latency is high, the application may require code optimization or database query tuning.

## Escalation
Escalate to the development team if the latency issue is persistent and appears to be related to application logic or database performance.
