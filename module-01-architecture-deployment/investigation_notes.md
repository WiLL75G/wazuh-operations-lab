# Module 01: Investigation Notes

**Project:** [Wazuh Operations Lab](../README.md)  
**Module:** [Architecture and Deployment Validation](README.md)  
**Environment:** Wazuh 4.14.6 | Docker | WIN-SOC  
**Date:** November 19, 2026  
**Status:** Completed  
**Assessment:** Demonstrated within the tested scope

## 1. Objective

Establish a verified operational baseline for the Wazuh deployment by examining container availability, Dashboard accessibility, Windows agent connectivity, and event processing.

## 2. Baseline and Observations

| Check | Observed Result | Evidence |
|---|---|---|
| Container runtime | Manager, Indexer, and Dashboard reported Up | [M01-E01](evidence/m01-01-wazuh-containers-running.png) |
| Dashboard | Monitoring interfaces accessible | [M01-E02](evidence/m01-02-win-soc-agent-active.png) |
| Agent connectivity | WIN-SOC, Agent 004, reported Active with recent keepalive | [M01-E02](evidence/m01-02-win-soc-agent-active.png) |
| Event ingestion | Windows Security Event ID 4688, Record ID 81093, retrieved | [M01-E03](evidence/m01-03-windows-event-4688.png) |
| Rule processing | Wazuh Rule 67027, level 3, processed the event | [M01-E04](evidence/m01-04-wazuh-rule-67027.png) |

## 3. Technical Analysis

**Observation:** All three required Wazuh containers were running, and the Dashboard was accessible.

**Interpretation:** The deployment supported the operational checks performed. Container status alone did not establish complete application health.

**Observation:** WIN-SOC Agent 004 reported Active.

**Interpretation:** Endpoint connectivity was confirmed at inspection. Complete telemetry coverage remained unverified.

**Observation:** Windows Security Event ID 4688 was decoded using `windows_eventchannel` and associated with Rule 67027.

**Interpretation:** The inspected process creation event reached Wazuh, was processed by a rule, and was retrievable through Threat Hunting.

## 4. Findings

1. Container runtime availability was confirmed.
2. Dashboard accessibility was confirmed.
3. WIN-SOC agent connectivity was confirmed.
4. Windows Security event ingestion was demonstrated.
5. Wazuh rule processing was demonstrated.

No malicious activity was established by the inspected process creation event.

## 5. Evidence Gaps

The exercise did not establish comprehensive application health, complete Windows telemetry coverage, continuous availability, or production readiness.

Agent connectivity and event ingestion were therefore assessed separately.

## 6. Disposition

**Result:** Baseline validation completed.

**Assessment:** Demonstrated within the tested scope.

**Operational conclusion:** The existing Wazuh deployment supported the tested monitoring functions. The verified baseline provides a reference for subsequent agent troubleshooting.

**Next investigation:** Controlled Windows agent service interruption and recovery in Module 02.

## 7. Documentation References

[Module 01 README](README.md) | [Project Home](../README.md) | [Continue to Module 02](../module-02-agent-troubleshooting/README.md)
