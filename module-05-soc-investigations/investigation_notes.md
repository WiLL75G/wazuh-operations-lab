# Module 05: Investigation Notes

**Project:** [Wazuh Operations Lab](../README.md)  
**Module:** [Joint SOC × DFIR Investigations](README.md)  
**Environment:** Wazuh 4.14.6 | Docker | WIN-SOC  
**Date:** November 21, 2026  
**Status:** Completed  
**Assessment:** Demonstrated within the tested scope

## Case M05-001: PowerShell Process Execution Investigation

### 1. Objective

Investigate a PowerShell process execution alert using Sysmon Event ID 1 and Wazuh telemetry.

Determine what executed, reconstruct the process ancestry, correlate the evidence, and establish an appropriate incident disposition.

### 2. Scope

**Endpoint:** WIN-SOC  
**Wazuh Agent:** 004  
**Event Source:** Microsoft-Windows-Sysmon/Operational  
**Event ID:** 1  
**Wazuh Rule:** 92027  
**Rule Level:** 4  
**Activity:** Controlled, authorized laboratory test

### 3. Baseline

Executed the following command:

`powershell.exe -NoProfile -Command "Write-Output 'M05-CASE001-BASELINE'; whoami.exe; hostname.exe"`

The expected activity was a PowerShell process launching `whoami.exe` and `hostname.exe`.

The command was intentionally executed for investigation and detection validation.

### 4. Verified Observations

- Sysmon recorded PowerShell process creation under Record ID 255340.
- The investigated PowerShell process had PID 8368.
- The executing account was WIN-SOC\James.
- The command line contained the expected investigation marker.
- The process integrity level was High.
- Wazuh generated Rule 92027 for the matching PowerShell execution.
- The parent PowerShell process had PID 7596 and Sysmon Record ID 254744.
- Its reported parent was WindowsTerminal.exe, PID 4100.
- Sysmon recorded `whoami.exe`, PID 2916, under Record ID 255341.
- Sysmon recorded `HOSTNAME.EXE`, PID 9160, under Record ID 255343.
- Both child processes were associated with PowerShell PID 8368.

### 5. Process Correlation

Reconstructed execution chain:

WindowsTerminal.exe (PID 4100)  
→ powershell.exe (PID 7596)  
→ powershell.exe (PID 8368)  
→ whoami.exe (PID 2916) and HOSTNAME.EXE (PID 9160)

The relevant process GUID relationships supported the correlation.

The command lines, account, and process ancestry matched the authorized test.

### 6. Analyst Interpretation

PowerShell launched another PowerShell instance and executed account and hostname discovery commands.

These utilities can be used for legitimate administration or attacker reconnaissance.

In this case, the observed execution matched the known authorized laboratory activity.

Wazuh correctly detected the process behavior. The detection itself did not establish malicious intent.

### 7. Evidence Gaps

- No comprehensive endpoint compromise investigation was performed.
- Unrelated suspicious activity outside the reviewed process chain was not assessed.
- No evidence of an actual intrusion was established.

These limitations do not prevent disposition of the controlled test case.

### 8. Findings

**Observation:** Sysmon recorded the expected PowerShell and child process execution.

**Correlation:** Sysmon process GUIDs, command lines, account details, and Wazuh Rule 92027 supported reconstruction of the activity.

**Interpretation:** The behavior was consistent with the authorized laboratory test.

**Unknown:** The security state of the endpoint outside this investigation scope.

### 9. Disposition

**Classification:** Benign True Positive  
**Authorization:** Confirmed Laboratory Test  
**Escalation:** Not Required  
**Case Status:** Closed

No malicious behavior was identified within the reviewed process chain.

### 10. Analyst Handoff

The controlled PowerShell execution was investigated using Sysmon Event ID 1 and Wazuh Threat Hunting.

Process ancestry and child process correlation confirmed that the observed activity matched the authorized test.

The case was closed as a benign true positive.

No further action is required for this controlled scenario.

### 11. Lessons Learned

- A process name alone does not determine malicious intent.
- Command-line arguments provide important execution context.
- Parent-child process relationships help reconstruct activity.
- Process GUIDs strengthen correlation between Sysmon records.
- A valid detection can represent authorized behavior.
- Incident dispositions must reflect the evidence and investigation scope.

**Final Assessment:** Case M05-001 completed within the tested scope.
