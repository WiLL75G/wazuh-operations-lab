
# Module 05: Joint SOC × DFIR Investigations

**Project:** Wazuh Operations Lab  
**Environment:** Wazuh 4.14.6 | Docker | Windows 11 ARM64 | WIN-SOC  
**Investigation Date:** November 21, 2026  
**Case:** M05-001 — PowerShell Process Execution  
**Case Status:** Completed  
**Disposition:** Benign True Positive — Authorized Laboratory Activity

## 1. Objective

Investigate a Wazuh process execution alert by correlating endpoint telemetry, reconstructing process ancestry, evaluating the observed behavior, and documenting a defensible analyst disposition.

The exercise focused on applying SOC triage and DFIR process investigation reasoning to a controlled Windows activity.

## 2. Investigation Scenario

A controlled PowerShell command was executed on WIN-SOC:

```powershell
powershell.exe -NoProfile -Command "Write-Output 'M05-CASE001-BASELINE'; whoami.exe; hostname.exe"
```

The command produced account and hostname discovery activity.

These utilities can be used legitimately or by attackers. The purpose of the exercise was to evaluate the execution context rather than classify the activity solely by command name.

## 3. Detection and Evidence

Wazuh generated an alert associated with the PowerShell execution.

| Field | Verified observation |
|---|---|
| Endpoint | WIN-SOC |
| Agent ID | 004 |
| Windows event | Sysmon Event ID 1 |
| Wazuh rule | 92027 |
| Rule level | 4 |
| Rule description | Powershell process spawned powershell instance |
| Investigated PowerShell PID | 8368 |
| Executing account | WIN-SOC\James |
| Investigation marker | M05-CASE001-BASELINE |

### Evidence 01: Wazuh Detection

The Wazuh Threat Hunting results showed the matching process creation event and Rule 92027.

![Wazuh PowerShell detection](evidence/m05-01-wazuh-detection.png)

### Evidence 02: Parent Process Investigation

Sysmon process creation records identified the original PowerShell session and its reported Windows Terminal parent.

The parent process GUID from the investigated event matched the original PowerShell process creation record.

![Sysmon parent process evidence](evidence/m05-02-parent-process.png)

### Evidence 03: Child Process Correlation

Sysmon recorded `whoami.exe` and `HOSTNAME.EXE` as child processes of the controlled PowerShell instance.

Both records were correlated using the PowerShell parent process GUID.

![Sysmon child process correlation](evidence/m05-03-child-processes.png)

## 4. Process Ancestry

The reviewed records supported the following execution chain:

```text
WindowsTerminal.exe
    PID 4100
        |
        v
powershell.exe
    PID 7596
        |
        v
powershell.exe
    PID 8368
        |
        +-- whoami.exe
        |      PID 2916
        |
        +-- HOSTNAME.EXE
               PID 9160
```

The process ancestry was reconstructed from Sysmon Event ID 1 records and matching process identifiers.

The Windows Terminal relationship was reported in the parent PowerShell event.

## 5. Investigation Methodology

### Baseline

Executed a known authorized PowerShell command with a unique investigation marker.

### Observation

Located the controlled process execution using Sysmon Event ID 1.

### Triage

Reviewed the executable path, command line, user account, process identifiers, and parent process information.

### Investigation

Identified the original PowerShell process creation record using the reported parent process GUID.

### Correlation

Connected the parent and child process records and confirmed that Wazuh generated a matching alert.

### Assessment

Compared the observed execution chain with the known authorized test activity.

### Disposition

Classified the alert as a benign true positive because the detection accurately reflected the observed execution, while the activity itself was authorized.

### Documentation

Prepared an evidence-based case summary, disposition, limitations, and analyst handoff.

## 6. Findings

**Observed**

PowerShell launched a second PowerShell instance, which executed `whoami.exe` and `hostname.exe`.

**Correlated**

Sysmon confirmed the process creation events and ancestry. Wazuh Rule 92027 detected the PowerShell execution.

**Interpretation**

The reviewed activity matched the authorized laboratory test.

**Unknowns and Limitations**

The investigation did not assess the entire endpoint for compromise or establish whether unrelated suspicious activity occurred elsewhere.

The findings are limited to the reviewed process chain and investigation window.

## 7. Disposition

**Classification:** Benign True Positive  
**Activity:** Authorized Laboratory Test  
**Escalation:** Not Required  
**Case Status:** Closed

No malicious behavior was identified within the reviewed process chain.

This conclusion does not imply that the complete endpoint was assessed or confirmed free of threats.

## 8. SOC and DFIR Skills Demonstrated

- Sysmon Event ID 1 interpretation
- Wazuh alert triage
- Process ancestry reconstruction
- Process GUID correlation
- Command-line investigation
- Distinguishing detection accuracy from malicious intent
- Evidence-based disposition
- Analyst case documentation

All skills were demonstrated in a controlled home-lab scenario, not a production SOC environment.

## 9. Analyst Takeaway

A detection alert is the beginning of an investigation, not the conclusion.

Process names and discovery commands must be evaluated alongside command lines, execution context, user accounts, process ancestry, and authorization.

The goal is to reach a defensible disposition based on verified evidence.

## 10. Navigation

[Project Home](../README.md) | [Previous Module: File Integrity Monitoring](../module-04-file-integrity-monitoring/README.md) | [Continue to Module 06: Customer Operations](../module-06-customer-operations/README.md)
