# Runbook: Low Availability (Sample App)

**Alert:** `LowAvailability`
**Severity:** Critical
**SLO Target:** 99.9% availability (non-5xx responses)

## Overview
This alert triggers when the percentage of successful (non-5xx) requests to the Sample App drops below 99.9% over a 5-minute window.

## Initial Diagnosis
1.  **Check Service Status:**
    Run `sudo docker ps` to see if the `sample-app` container is running.
2.  **Check Container Logs:**
    Run `sudo docker logs sample-app` to look for application crashes or 5xx errors in the output.
3.  **Check Upstream Dependencies:**
    Verify if any external services or databases that the app depends on are reachable.

## Mitigation Steps
1.  **Restart Container:**
    If the app is unresponsive or in a bad state, try restarting it:
    `sudo docker compose restart sample-app` (from `/home/team/shared/sample-app`)
2.  **Rollback:**
    If the issue started after a recent deployment, rollback to the previous known good image tag.
3.  **Scale Up:**
    If the issue is due to high traffic volume, consider adding more replicas if supported by the orchestration layer.

## Escalation
If the service remains unavailable after a restart and no obvious application error is found, escalate to the SRE Lead.
