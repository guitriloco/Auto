# Runbook: Arbitrage Engine Issues

## 1. Overview
The Python Arbitrage Engine (`arbitrage_engine.py`) is responsible for autonomous scaling of infrastructure based on real-time SLIs.

## 2. Troubleshooting Steps

### 2.1 Engine Not Running
If the engine is not making decisions or logging updates:
1.  **Check Service Status:**
    ```bash
    sudo systemctl status arbitrage-engine
    ```
2.  **Inspect Logs:**
    ```bash
    cat /home/team/shared/scripts/arbitrage_engine.log
    ```
3.  **Restart Service:**
    ```bash
    sudo systemctl restart arbitrage-engine
    ```

### 2.2 Incorrect Scaling Decisions
If the engine is scaling up/down unexpectedly:
1.  **Verify Prometheus Metrics:** Check if Prometheus is reporting accurate SLIs at `http://localhost:9090`.
2.  **Adjust Thresholds:** Edit `/home/team/shared/scripts/arbitrage_engine.py` to refine `LATENCY_UPPER_THRESHOLD` or `AVAILABILITY_THRESHOLD`.
3.  **Manual Override:** Stop the engine service and scale manually using Docker Compose:
    ```bash
    sudo systemctl stop arbitrage-engine
    sudo docker compose -f /home/team/shared/sample-app/docker-compose.yml up -d --scale sample-app=<desired_replicas>
    ```

### 2.3 Connectivity Issues
If the engine fails to query Prometheus:
1.  **Check Prometheus Container:** `sudo docker ps | grep prometheus`.
2.  **Verify Network:** Ensure the engine host can reach the Prometheus endpoint.

## 3. Automation Contacts
- **Primary:** Automation Engineer (@agent-automation-engineer)
- **Secondary:** SRE Lead (@agent-lead)
