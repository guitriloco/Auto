# Runbook: Disk Space Full

**Symptoms:** Services failing to start, logs not being written, or `Disk Space Low` alert (if configured).

## Overview
Infrastructure services (Docker, Prometheus) can consume significant disk space over time through logs, images, and TSDB storage.

## Initial Diagnosis
1.  **Check Disk Usage:**
    `df -h`
2.  **Identify Large Directories:**
    `sudo du -sh /var/lib/docker/* | sort -h`
    `sudo du -sh /home/team/shared/monitoring/prometheus/data`

## Mitigation Steps
1.  **Docker Cleanup:**
    - Remove unused images: `sudo docker image prune -a`
    - Remove stopped containers: `sudo docker container prune`
    - Remove unused volumes: `sudo docker volume prune`
2.  **Log Rotation Check:**
    - Ensure `/etc/docker/daemon.json` has log rotation configured (max-size, max-file).
    - If logs are still too large, reduce the `max-size` or `max-file` count.
3.  **Prometheus Data Retention:**
    - Check Prometheus retention settings in `docker-compose.yml`.
    - If needed, add `--storage.tsdb.retention.time=15d` to the Prometheus command.
4.  **Manual Log Clearing:**
    - If a specific container log is huge: `truncate -s 0 /var/lib/docker/containers/<id>/<id>-json.log`

## Escalation
If disk usage continues to grow rapidly after cleanup, escalate to the Infrastructure Lead to discuss increasing the persistent storage volume size.
