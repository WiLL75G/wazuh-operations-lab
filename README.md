# Wazuh Operations Lab

**Wazuh Deployment | Agent Troubleshooting | Windows Telemetry | SOC Investigations | Customer Operations**

An independent hands-on project documenting how I validate, troubleshoot, investigate, and operate a Wazuh security monitoring environment.

The project uses Wazuh 4.14.6, Docker, and a Windows 11 endpoint to develop practical skills in platform operations, log collection, incident investigation, evidence correlation, and simulated MSSP customer operations.

All technical findings are based on observed home-lab evidence. Simulations are identified clearly, and no production or commercial MSSP experience is claimed.

## 1. Lab Environment

| Component | Configuration |
|---|---|
| Host | MacBook Air M2 |
| Deployment | Wazuh 4.14.6 single-node Docker |
| Components | Manager, Indexer, Dashboard |
| Primary Endpoint | Windows 11 ARM64, WIN-SOC |
| Windows Agent | Wazuh Agent 4.14.9, ID 004 |
| Telemetry | Windows Event Logs, Sysmon, PowerShell, FIM |
| Investigation Interface | Wazuh Threat Hunting |
| Customer Operations | Fictional MSSP scenarios |

**Scope:** Windows endpoint operations, detection validation, incident investigation, and simulated customer handling.

An Ubuntu agent was visible as Active during Module 06 monitoring validation. Dedicated Linux endpoint investigations and production clustering were outside the tested module scope.

For additional deployment documentation, see [Architecture](architecture/README.md).

## 2. Project Modules

| Module | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Architecture and Deployment Validation | Completed | [Module 01](module-01-architecture-deployment/README.md) |
| 02 | Windows Agent Troubleshooting | Completed | [Module 02](module-02-agent-troubleshooting/README.md) |
| 03 | Windows Log Collection Troubleshooting | Completed | [Module 03](module-03-log-ingestion/README.md) |
| 04 | File Integrity Monitoring | Completed | [Module 04](module-04-file-integrity-monitoring/README.md) |
| 05 | Joint SOC × DFIR Investigations | Completed | [Module 05](module-05-soc-investigations/README.md) |
| 06 | Customer Operations and Reporting | Exercise Completed | [Module 06](module-06-customer-operations/README.md) |
| 07 | Blind Wazuh Operations Assessment | Not Started | Pending |

**Practical Progress: 6 of 7 modules documented.**

The completed modules reflect the laboratory exercises performed within their verified scopes.

Module 06 includes an unresolved investigation case. Completing the training exercise does not imply that the underlying security event received a conclusive disposition.

Module 07 remains the final independent practical assessment.

## 3. Verified Findings

### Module 01: Architecture and Deployment Validation

Verified the Wazuh Manager, Indexer, and Dashboard containers.

Confirmed Dashboard accessibility, WIN-SOC agent connectivity, and Windows Security Event ID 4688 processed by Wazuh Rule 67027.

**Key Finding:** The deployment was operational within the tested configuration.

**Limitation:** Running containers and an Active agent do not independently demonstrate complete monitoring coverage.

[View Module 01 Investigation](module-01-architecture-deployment/README.md)

### Module 02: Windows Agent Troubleshooting

Performed a controlled interruption of the Windows Wazuh agent service.

Verified the service Stopped state, restarted the service, and confirmed successful recovery.

The endpoint subsequently returned to Active status in Wazuh.

**Key Finding:** Local service interruption and recovery were demonstrated.

**Limitation:** The Manager-side Disconnected state was not captured during the controlled test.

[View Module 02 Investigation](module-02-agent-troubleshooting/README.md)

### Module 03: Windows Log Collection Troubleshooting

Identified a missing PowerShell Operational event-channel collector.

Corrected the Windows agent configuration and verified that PowerShell Operational events reached the Wazuh Manager.

Confirmed Event ID 40962 and Event ID 4104, Record ID 2805, in the collected telemetry.

Restored temporary Manager raw-event archiving to its original disabled configuration.

**Key Finding:** PowerShell Operational event ingestion was successfully demonstrated.

**Limitation:** Event ID 4104 alert generation was not demonstrated.

[View Module 03 Investigation](module-03-log-ingestion/README.md)

### Module 04: File Integrity Monitoring

Investigated file activity within the monitored directory:

`C:\NORTHSTAR-HR-Lab`

Performed controlled file creation, modification, and deletion operations.

Correlated the resulting FIM events with Wazuh detection rules.

| Activity | Wazuh Rule | Outcome |
|---|---|---|
| File creation | 554 | Detected |
| File modification | 550 | Detected |
| File deletion | 553 | Detected |

Validated the monitored file path, associated event details, and changes recorded by Wazuh.

**Key Finding:** Real-time file integrity monitoring detected the controlled file operations.

**Limitation:** File ownership metadata did not establish the identity of the human who performed the deletion.

[View Module 04 Investigation](module-04-file-integrity-monitoring/README.md)

### Module 05: Joint SOC × DFIR Investigations

**Case ID:** M05-001

**Investigation:** PowerShell Process Execution

Executed a controlled PowerShell command containing a unique investigation marker.

Investigated the resulting telemetry using Sysmon Event ID 1 and Wazuh Threat Hunting.

Correlated the execution of PowerShell with its parent process and associated child processes.

The reconstructed process chain was:

```text
WindowsTerminal.exe
    |
    v
powershell.exe
    |
    v
powershell.exe
    |
    +--> whoami.exe
    |
    +--> hostname.exe
```

Wazuh Rule 92027 detected the PowerShell execution.

Process identifiers, process GUIDs, command lines, account information, and Sysmon records supported the investigation.

**Case Classification:** Benign True Positive

**Authorization:** Confirmed Laboratory Activity

**Escalation:** Not Required

**Key Finding:** A valid detection can represent authorized behavior rather than a security incident.

**Limitation:** The investigation established the context of the controlled process chain, not the complete security state of the endpoint.

[View Module 05 Investigation](module-05-soc-investigations/README.md)

### Module 06: Customer Operations and Reporting

**Case ID:** M06-001

**Customer:** CORP-A (Fictional)

**Investigation:** Application Compatibility Database Execution

Investigated repeated Wazuh Rule 92058 alerts involving `sdbinst.exe`.

The original 24-hour search returned 12 matching alerts.

The reviewed evidence showed:

| Field | Verified Observation |
|---|---|
| Executable | sdbinst.exe |
| Command Line | sdbinst.exe -m -bg |
| Executing Account | NT AUTHORITY\SYSTEM |
| Parent Process | svchost.exe |
| Parent PID | 1204 |
| Wazuh Rule | 92058 |
| Rule Level | 12 |
| Sysmon Event | Event ID 1 |

Investigated the reported parent process and its Windows service context.

Identified the Program Compatibility Assistant Service, PcaSvc, among the services hosted by the parent process.

Compared two process creation events and identified consistent executable, command-line, account, parent process, and hash characteristics.

Verified that the current executable had a valid Microsoft digital signature.

Confirmed that its SHA256 matched the hash recorded in the reviewed Sysmon events.

**Key Finding:** The evidence supported a plausible legitimate Windows compatibility operation.

**Evidence Limitation:** The exact initiating service and execution authorization were not established.

**Case Classification:** Unresolved

**Case Status:** Open, Pending Validation

**Confirmed Compromise:** Not Established

**Confirmed Authorization:** Not Established

The exercise also included simulated customer case intake, priority assessment, SLA calculation, internal analyst handoff, and customer communication.

#### Simulated SLA Management

| Priority | Acknowledgment Target |
|---|---|
| Critical | 15 minutes |
| High | 30 minutes |
| Medium | 2 hours |
| Low | 8 hours |

The case used a provisional Medium priority.

An assumed intake time of 11:00 AM produced a calculated acknowledgment deadline of 1:00 PM.

The example acknowledgment time was hypothetical.

No real customer ticket was acknowledged.

#### Customer Reporting

Prepared an internal technical handoff preserving the investigation findings, correlation results, and unresolved evidence gaps.

Prepared a nontechnical customer summary explaining the detected activity without claiming a confirmed compromise or confirmed benign outcome.

**Key Finding:** Customer communication must distinguish observed evidence, analyst interpretation, and unresolved questions.

**Limitation:** Customer records, SLA timing, and communication were simulated. Genuine multi-customer operations were not demonstrated.

[View Module 06 Investigation](module-06-customer-operations/README.md)

## 4. Module 07: Blind Wazuh Operations Assessment

**Status:** Not Started

The final module will assess the ability to independently investigate an unfamiliar security scenario using the established Wazuh environment.

The assessment will cover:

1. Endpoint monitoring validation.
2. Security alert intake and initial triage.
3. Process and event investigation.
4. Evidence correlation.
5. Hypothesis testing and identification of evidence gaps.
6. Simulated severity and escalation decisions.
7. Simulated SLA reasoning.
8. Internal analyst reporting.
9. Customer communication.
10. Evidence-based case disposition.

The blind assessment will require independent analyst decisions before guided feedback.

Only verified actions and observed results will count as demonstrated evidence.

The final assessment will distinguish independent performance from tasks completed with guidance.

## 5. Investigation and Troubleshooting Workflow

The project follows an evidence-based investigation methodology.

### Step 01: Establish Baseline

Identify the expected environment, process behavior, and available telemetry.

### Step 02: Observe

Identify the alert, event, or operational issue requiring investigation.

### Step 03: Triage

Review the relevant event fields, affected endpoint, process, command line, account, and severity.

### Step 04: Investigate

Examine the activity and determine what additional evidence is necessary.

### Step 05: Correlate

Connect related events using process GUIDs, process ancestry, timestamps, account information, and supporting telemetry.

### Step 06: Interpret

Separate observed evidence from assumptions and competing explanations.

### Step 07: Disposition

Determine whether the reviewed evidence supports expected activity, suspicious activity, confirmed malicious activity, or an unresolved outcome.

### Step 08: Document

Record the investigation scope, evidence, findings, limitations, and recommended next action.

The purpose is to understand why an event occurred and determine what the evidence supports, rather than simply execute commands or accept alert descriptions as conclusions.

## 6. Evidence and Assessments

Each module maintains its investigation documentation and selected genuine laboratory evidence.

Evidence may include screenshots, configuration excerpts, event metadata, command outputs, and verified investigation results.

The project follows three assessment classifications:

| Classification | Meaning |
|---|---|
| Demonstrated | Verified evidence supports successful execution within the stated scope |
| Partially demonstrated | Some required elements were completed, but limitations remain |
| Not yet tested | No sufficient hands-on evidence has been collected |

The `assessments/` directory is intended to consolidate competency results and reference the supporting module evidence.

### Current Competency Summary

| Competency | Assessment |
|---|---|
| Wazuh Deployment Validation | Demonstrated |
| Windows Agent Service Recovery | Demonstrated |
| Manager Disconnect Detection | Not yet tested successfully |
| Windows Log Collection Troubleshooting | Demonstrated |
| PowerShell Event 4104 Ingestion | Demonstrated |
| PowerShell Event 4104 Alert Generation | Not yet tested |
| File Integrity Monitoring | Demonstrated |
| Human Attribution for File Deletion | Not yet demonstrated |
| Sysmon Process Investigation | Demonstrated with guidance |
| Process Ancestry Correlation | Demonstrated with guidance |
| Wazuh Detection Investigation | Demonstrated with guidance |
| Simulated Severity Assessment | Partially demonstrated |
| Simulated SLA Calculation | Demonstrated |
| Actual SLA Acknowledgment | Not yet tested |
| Internal Analyst Handoff | Demonstrated with guidance |
| Customer Communication | Demonstrated with guidance |
| Fictional Multi-Customer Case Handling | Not yet tested |
| Independent Blind Assessment | Not yet tested |

The final competency assessment will be updated after Module 07.

No competency will be marked Demonstrated without supporting evidence.

## 7. Scope and Limitations

This project represents independent cybersecurity laboratory training.

It does not represent production Wazuh administration, professional SOC employment, or commercial MSSP experience.

The primary practical endpoint is WIN-SOC.

Dedicated Linux investigations, production clustering, enterprise availability testing, and large-scale distributed deployment were outside the completed module scope.

Customer scenarios use fictional organizations and explicitly simulated operational conditions.

No real customer multitenancy, contractual SLA compliance, or commercial customer support experience is claimed.

Modules may contain unresolved findings when the available telemetry does not support a conclusive disposition.

Investigation results are limited to the evidence collected and the activity actually tested.

Sensitive credentials and unnecessary personal information will not be included in public evidence.

## 8. Repository Structure

```text
wazuh-operations-lab/
│
├── README.md
│
├── architecture/
│
├── assessments/
│
├── module-01-architecture-deployment/
│
├── module-02-agent-troubleshooting/
│
├── module-03-log-ingestion/
│
├── module-04-file-integrity-monitoring/
│
├── module-05-soc-investigations/
│
├── module-06-customer-operations/
│
└── module-07-blind-assessment/
```

Module 07 is planned and will be added after its practical assessment.

Module documentation includes individual README files, investigation notes, and relevant evidence.

## 9. Project Status

**Modules Documented:** 6 of 7

**Completed Practical Modules:** 01, 02, 03, 04, 05, 06

**Remaining Module:** 07 Blind Wazuh Operations Assessment

**Final Project Assessment:** Pending

Module 06 is counted as a completed laboratory exercise, while its underlying investigation remains unresolved.

The project will be assessed as a whole after the final blind assessment and evidence review.

## 10. Author

**William Gokah**

SOC Investigations | Wazuh Operations | Detection Engineering

[LinkedIn](https://linkedin.com/in/WilliamInCyber) | [GitHub](https://github.com/WiLL75G)

*Built through independent, evidence-based cybersecurity training.*

