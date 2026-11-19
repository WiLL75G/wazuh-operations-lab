
# Wazuh Operations Lab

**Deployment Validation | Agent Troubleshooting | Windows Log Collection | Security Operations**

A hands-on Wazuh Operations project focused on deploying, managing, troubleshooting, and validating endpoint security monitoring.

This repository documents practical exercises performed in my independent home lab using **Wazuh 4.14.6, Docker, and Windows 11**.

My goal is to develop the operational skills required to support reliable Wazuh deployments, diagnose monitoring failures, and communicate technical findings clearly.

All findings are based on actual lab observations. Simulated failures are identified as simulations, and conclusions are limited to what the evidence supports.

---

## Lab Architecture

| Component | Environment | Purpose |
|---|---|---|
| Host | MacBook Air M2, macOS | Lab workstation |
| Container platform | Docker | Wazuh deployment |
| Wazuh Manager | 4.14.6 | Event processing and detection rules |
| Wazuh Indexer | Single-node deployment | Security event indexing and storage |
| Wazuh Dashboard | Single-node deployment | Monitoring and investigation |
| Endpoint | Windows 11 ARM64, WIN-SOC | Monitored Windows system |
| Wazuh Agent | 4.14.9, Agent ID 004 | Endpoint telemetry collection |
| Telemetry | Windows Event Logs, Sysmon, PowerShell Operational | Security monitoring |

**Current scope:** One monitored Windows endpoint. Linux endpoint testing, production clustering, and real customer environments are outside this project's initial scope.

### Monitoring Architecture

```text
MacBook Air M2
└── Docker — Wazuh 4.14.6
    ├── Wazuh Manager
    ├── Wazuh Indexer
    └── Wazuh Dashboard
             ▲
             │ Agent communication
             ▼
Windows 11 — WIN-SOC
└── Wazuh Agent 4.14.9
    ├── Windows Security Events
    ├── Windows System and Application Events
    ├── Sysmon Operational Events
    ├── PowerShell Operational Events
    └── File Integrity Monitoring
```

---

## Practical Training Modules

| Module | Focus | Practical status |
|---|---|---|
| 01 | Architecture and Deployment Validation | Completed |
| 02 | Windows Agent Management and Troubleshooting | Completed |
| 03 | Windows Log Collection and Troubleshooting | Completed |
| 04 | File Integrity Monitoring | Pending in this project |
| 05 | Joint SOC × DFIR Investigations | Pending |
| 06 | Customer Operations and Reporting | Pending |
| 07 | Blind Wazuh Operations Assessment | Pending |

The status reflects hands-on training progress. Module documentation and evidence will be added progressively.

---

## Verified Technical Work

### Module 01 — Architecture and Deployment Validation

**Objective:** Verify that the existing Wazuh platform and Windows endpoint monitoring are operational.

**Activities performed:**
- Inspected Docker runtime status for the Wazuh Manager, Indexer, and Dashboard.
- Verified access to the Wazuh Dashboard.
- Confirmed WIN-SOC, Agent 004, reported Active with a recent keepalive.
- Inspected individual Windows security event records.
- Verified Windows Security Event ID 4688 was processed by Wazuh Rule 67027 and retrievable through Threat Hunting.

**Outcome:** Demonstrated container runtime availability, Dashboard access, agent connectivity, and Windows Security event ingestion.

**Limitation:** Docker container status alone does not establish comprehensive application or production readiness.

### Module 02 — Windows Agent Troubleshooting

**Objective:** Practice identifying and recovering from a controlled Windows Wazuh agent service interruption.

**Activities performed:**
- Established the Windows agent service baseline.
- Intentionally stopped the WazuhSvc service.
- Confirmed the service entered the Stopped state.
- Restarted the service and verified Running status.
- Confirmed WIN-SOC returned to Active status in the Wazuh Dashboard.

**Outcome:** Demonstrated controlled service interruption, recovery, and endpoint reconnection verification.

**Limitation:** A Manager-side disconnected state was not captured during the interruption.

### Module 03 — Windows Log Collection Troubleshooting

**Objective:** Identify and correct a missing Windows event-channel collector.

**Problem:** The Wazuh agent configuration did not contain an explicit collection entry for Microsoft-Windows-PowerShell/Operational.

**Activities performed:**
- Inspected the Windows agent's ossec.conf configuration.
- Verified the Windows PowerShell Operational channel was enabled and contained events.
- Added the missing eventchannel localfile configuration.
- Validated XML syntax and restarted the Windows agent.
- Confirmed the agent initialized the PowerShell Operational collector.
- Temporarily enabled Manager-side JSON raw-event archiving to verify ingestion.
- Retrieved an archived PowerShell Operational Event ID 40962 from WIN-SOC, Agent 004.
- Restored Manager raw-event archiving to its original disabled state and restarted the Manager.

**Outcome:** Confirmed end-to-end reception of a PowerShell Operational event by the Wazuh Manager.

**Limitation:** This exercise did not separately establish Event ID 4104 collection or alert generation for every PowerShell event.

---

## Operational Troubleshooting Method

Every exercise follows an evidence-first workflow:

1. **Baseline:** Establish the expected working state.
2. **Problem:** Identify the reported issue or controlled failure.
3. **Investigation:** Examine the relevant configuration, services, and logs.
4. **Resolution:** Make a targeted change supported by the findings.
5. **Verification:** Confirm recovery or event reception using actual evidence.
6. **Documentation:** Record the outcome, unresolved questions, and operational lessons.

A successful command is not automatically proof of a healthy service. A connected agent is not automatically proof of complete telemetry coverage.

---

## Repository Organization

```text
wazuh-operations-lab/
├── README.md
├── architecture/
├── module-01-architecture-deployment/
├── module-02-agent-troubleshooting/
├── module-03-log-ingestion/
├── module-04-file-integrity-monitoring/
├── module-05-soc-investigations/
├── module-06-customer-operations/
├── module-07-blind-assessment/
└── assessments/
```

Each module will contain a focused technical README and selected genuine evidence. Detailed investigation notes will be included where they add value.

The assessments folder will track competencies as Demonstrated, Partially demonstrated, or Not yet tested.

---

## Skills Demonstrated

- Wazuh single-node Docker deployment verification
- Wazuh architecture and component responsibilities
- Windows agent monitoring and service troubleshooting
- Agent connectivity and recovery verification
- Windows Security event ingestion
- PowerShell Operational event-channel configuration
- Wazuh agent and Manager log inspection
- Raw-event ingestion verification
- Configuration backup, validation, and rollback
- Evidence-based technical documentation

---

## Scope and Evidence Integrity

This project is an **independent home-lab training environment**, not a production Wazuh deployment or commercial MSSP engagement.

All customer scenarios introduced later will be fictional and clearly labeled. No production availability, enterprise scale, or professional customer support experience is claimed from these exercises.

Only genuine logs, screenshots, observed configurations, and verified results are used as technical evidence.

Sensitive credentials, authentication tokens, and unnecessary personal information are excluded from public documentation.

---

## Author

**William Gokah**

Cybersecurity | SOC Investigations | Wazuh Operations | Detection Engineering

- LinkedIn: https://linkedin.com/in/WilliamInCyber
- GitHub: https://github.com/WiLL75G

*Built through independent, practical cybersecurity training.*
