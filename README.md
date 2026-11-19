
# Wazuh Operations Lab

**Wazuh Deployment | Agent Troubleshooting | Windows Telemetry | SOC Operations**

An independent hands-on project documenting how I validate, troubleshoot, and operate a Wazuh security monitoring environment.

The project uses Wazuh 4.14.6, Docker, and a Windows 11 endpoint to develop practical skills in platform operations, log collection, incident investigation, and evidence-based troubleshooting.

All technical findings are based on observed home-lab evidence. Simulations are identified clearly, and no production or commercial MSSP experience is claimed.

---

## Lab Environment

| Component | Configuration |
|---|---|
| Host | MacBook Air M2 |
| Deployment | Wazuh 4.14.6 single-node Docker |
| Components | Manager, Indexer, Dashboard |
| Endpoint | Windows 11 ARM64, WIN-SOC |
| Agent | Wazuh Agent 4.14.9, ID 004 |
| Telemetry | Windows Event Logs, Sysmon, PowerShell, FIM |

**Scope:** One Windows endpoint. Linux endpoint testing and production clustering are excluded.

For architecture details, see [Architecture](architecture/README.md) once published.

---

## Project Modules

| Module | Focus | Status |
|---|---|---|
| 01 | Architecture and Deployment Validation | Completed |
| 02 | Windows Agent Troubleshooting | Completed |
| 03 | Windows Log Collection Troubleshooting | Completed |
| 04 | File Integrity Monitoring | Pending |
| 05 | Joint SOC × DFIR Investigations | Pending |
| 06 | Customer Operations and Reporting | Pending |
| 07 | Blind Wazuh Operations Assessment | Pending |

**Practical progress: 3 of 7 modules completed.**

Each module will contain its own technical README, relevant observations, verified results, limitations, and selected genuine evidence.

Module documentation and navigation links will be published progressively as the corresponding files are created.

---

## Verified Findings

### Module 01 — Deployment Validation

Verified running Wazuh containers, Dashboard accessibility, WIN-SOC agent connectivity, and Windows Security Event ID 4688 processed by Wazuh Rule 67027.

**Key distinction:** Running containers and an Active agent do not independently prove comprehensive monitoring health.

### Module 02 — Agent Troubleshooting

Performed a controlled Windows Wazuh agent service interruption, verified the Stopped state, restarted the service, and confirmed Running and Active status.

**Key limitation:** Manager-side Disconnected status was not captured.

### Module 03 — Log Ingestion

Identified a missing PowerShell Operational event-channel collector, corrected the Windows agent configuration, and verified end-to-end ingestion.

Confirmed PowerShell Operational Event ID 40962 and independently validated Event ID 4104, Record ID 2805, reaching the Wazuh Manager.

Restored temporary Manager raw-event archiving to its original disabled configuration.

**Key distinction:** Event ingestion was verified; Event ID 4104 alert generation was not demonstrated.

---

## Investigation and Troubleshooting Workflow

Each practical module follows:

1. Establish baseline
2. Identify problem
3. Investigate evidence
4. Apply targeted remediation
5. Verify actual outcome
6. Document findings and limitations

The goal is not simply to execute commands, but to demonstrate that the expected operational result occurred.

---

## Evidence and Assessments

Genuine evidence is stored with the module where it was collected.

Evidence may include selected screenshots, configuration excerpts, event metadata, and observed command outputs.

The `assessments/` directory will consolidate competency results using:

- Demonstrated
- Partially demonstrated
- Not yet tested

Final assessments will reference original module evidence rather than duplicating screenshots.

---

## Scope and Limitations

This project represents independent laboratory training, not production Wazuh administration or professional MSSP experience.

Future customer scenarios will use fictional organizations and explicitly simulated operational conditions.

No real multi-tenancy, production SLAs, enterprise availability, or commercial customer-support experience is claimed.

Sensitive credentials and unnecessary personal information will not be included in public evidence.

---

## Author

**William James**

SOC Investigations | Wazuh Operations | Detection Engineering

[LinkedIn](https://linkedin.com/in/WilliamInCyber) | [GitHub](https://github.com/WiLL75G)

*Built through independent, evidence-based cybersecurity training.*

