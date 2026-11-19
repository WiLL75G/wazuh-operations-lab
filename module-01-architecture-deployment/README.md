
# Module 01 — Architecture and Deployment Validation

**Project:** [Wazuh Operations Lab](../README.md)  
**Environment:** Wazuh 4.14.6 | Docker | Windows 11  
**Status:** Completed  
**Assessment:** Demonstrated within the tested scope

---

## 1. Objective

Validate the existing Wazuh deployment and establish a monitoring baseline before performing operational troubleshooting.

The goal was to independently verify:

- Wazuh container runtime status
- Dashboard accessibility
- Windows agent connectivity
- Windows Security event ingestion
- Retrieval of a processed security event

A running container or connected agent was not treated as sufficient proof of complete platform health.

---

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Host | MacBook Air M2 |
| Container platform | Docker |
| Wazuh deployment | Single-node, version 4.14.6 |
| Wazuh components | Manager, Indexer, Dashboard |
| Endpoint | Windows 11 ARM64, WIN-SOC |
| Agent | Wazuh Agent 4.14.9 |
| Agent ID | 004 |
| Endpoint IP | 192.168.64.20 |

### Architecture

```text
MacBook Air M2
    |
    +-- Docker
    |    +-- Wazuh Manager
    |    +-- Wazuh Indexer
    |    +-- Wazuh Dashboard
    |
    +-- Windows 11 VM: WIN-SOC
         +-- Wazuh Agent 004
         +-- Windows Event Logs
         +-- Sysmon
```

The Windows agent communicates with the Wazuh Manager, which processes collected events for security monitoring.

---

## 3. Baseline Validation

### Check 01 — Docker Container Status

**Action:** Inspected the running Wazuh containers using:

```bash
docker ps
```

**Observed:**

The following containers reported a running state:

- `single-node-wazuh.manager-1`
- `single-node-wazuh.indexer-1`
- `single-node-wazuh.dashboard-1`

**Interpretation:**

The required Wazuh containers were running during the inspection.

**Limitation:**

A Docker `Up` state does not independently establish internal service health or production readiness.

### Check 02 — Dashboard Accessibility

**Action:** Accessed the Wazuh Dashboard.

**Observed:**

The Dashboard interface was accessible.

**Interpretation:**

The web interface was available for monitoring and investigation at the time of testing.

**Limitation:**

Dashboard accessibility alone does not prove that every backend service or data pipeline is healthy.

### Check 03 — Windows Agent Connectivity

**Action:** Inspected the WIN-SOC agent in the Wazuh Dashboard.

**Observed:**

| Field | Result |
|---|---|
| Agent name | WIN-SOC |
| Agent ID | 004 |
| Agent IP | 192.168.64.20 |
| Agent status | Active |
| Keepalive | Recent at inspection |

**Interpretation:**

The Wazuh Manager reported the Windows endpoint as connected.

**Limitation:**

An Active agent does not prove that every required log source is configured or collected.

### Check 04 — Windows Security Event Ingestion

**Action:** Examined security event records through Wazuh Threat Hunting.

**Observed:**

| Field | Verified result |
|---|---|
| Endpoint | WIN-SOC |
| Windows Event ID | 4688 |
| Event category | Process creation |
| Wazuh Rule ID | 67027 |
| Investigation interface | Threat Hunting |

**Interpretation:**

Windows Security process-creation telemetry was collected, processed by a Wazuh rule, and retrievable for investigation.

This provided stronger evidence of event ingestion than agent connectivity alone.

**Limitation:**

This observation establishes ingestion and processing for the inspected event, not comprehensive coverage of all Windows Security events.

---

## 4. Evidence Summary

| Evidence ID | Validation | Finding |
|---|---|---|
| M01-E01 | Docker runtime | Three required Wazuh containers running |
| M01-E02 | Dashboard access | Dashboard accessible |
| M01-E03 | Agent connectivity | WIN-SOC Agent 004 reported Active |
| M01-E04 | Security telemetry | Event ID 4688 processed by Rule 67027 |

Evidence references identify verified observations. Supporting screenshots will be linked here only after the corresponding genuine files have been added to the repository.

No screenshots have been reconstructed or generated for this module.

---

## 5. Findings

The exercise established a practical operational baseline across four separate checkpoints:

1. The Wazuh containers were running.
2. The Wazuh Dashboard was accessible.
3. WIN-SOC was reporting Active.
4. A Windows Security Event ID 4688 was processed and retrievable through Threat Hunting.

These observations collectively support baseline monitoring functionality within the tested environment.

They do not establish production readiness or comprehensive event coverage.

---

## 6. Operational Lessons

**Container availability and application health are different checks.**

A running container establishes runtime status, not necessarily full service health.

**Agent connectivity and telemetry collection are different checks.**

An Active endpoint can still have missing event-channel collectors.

**Actual event retrieval provides stronger ingestion evidence.**

Inspecting Event ID 4688 and its Wazuh rule association established that the tested Windows Security event was processed and available for investigation.

---

## 7. Outcome and Assessment

**Result:** Completed.

**Demonstrated:**

- Docker container runtime inspection
- Wazuh Dashboard accessibility verification
- Windows agent connectivity verification
- Windows Security event ingestion validation
- Evidence-based operational interpretation

**Not established:**

- Comprehensive Wazuh component health
- Production availability or resilience
- Complete Windows telemetry coverage
- High availability or clustering

**Final assessment:** Demonstrated within the tested scope.

---

## 8. Related Modules

**Next practical exercise:** Windows Agent Management and Troubleshooting.

The next module builds on this baseline by testing a controlled Windows agent service interruption and recovery.

---

[← Back to Wazuh Operations Lab](../README.md)
