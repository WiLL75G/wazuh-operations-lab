# Module 06: Investigation Notes

**Project:** [Wazuh Operations Lab](../README.md)  
**Module:** [Customer Operations](README.md)  
**Environment:** Wazuh 4.14.6 | Docker | WIN-SOC  
**Date:** November 22, 2026  
**Status:** Laboratory Exercise Completed  
**Assessment:** Partially demonstrated within the tested scope

## 1. Case Overview

**Case ID:** M06-001  
**Customer:** CORP-A (Fictional)  
**Endpoint:** WIN-SOC  
**Agent ID:** 004  
**Detection:** Application Compatibility Database launched  
**Wazuh Rule:** 92058  
**Rule Level:** 12  
**Priority:** Medium, Provisional  
**Case Status:** Open, Pending Validation

## 2. Objective

Investigate repeated application compatibility database execution and practise MSSP customer operations.

The exercise included alert intake, severity assessment, simulated SLA management, technical investigation, internal handoff, and customer communication.

No actual customer tenancy or production MSSP activity was involved.

## 3. Monitoring Baseline

WIN-SOC was confirmed Active in Wazuh.

The endpoint was generating security events through Windows Sysmon.

The initial 24 hour search returned 1,478 matching alerts.

A focused Rule 92058 search returned 12 matching alerts.

## 4. Verified Observations

**Process:** C:\Windows\System32\sdbinst.exe  
**Command Line:** C:\WINDOWS\System32\sdbinst.exe -m -bg  
**Executing Account:** NT AUTHORITY\SYSTEM  
**Integrity Level:** System  
**Parent Process:** C:\Windows\System32\svchost.exe  
**Parent PID:** 1204  
**Event Source:** Sysmon Event ID 1

Two reviewed executions showed matching executable paths, command line arguments, account context, parent process GUIDs, and SHA256 hashes.

The executions represented separate process instances.

## 5. Process and Service Correlation

The running svchost.exe process associated with PID 1204 had a creation time of November 19, 2026, 12:29:43 PM.

Its executable path and command line matched the parent information recorded in Wazuh.

The process hosted multiple Windows services, including PcaSvc.

The Program Compatibility Assistant Service was confirmed Running under LocalSystem.

Its service host configuration matched the investigated parent process context.

The exact service responsible for initiating sdbinst.exe was not established.

## 6. Executable Integrity

Windows reported a valid Microsoft digital signature for sdbinst.exe.

The SHA256 calculated from the executable matched the hash recorded in the reviewed Sysmon events.

**SHA256:**

70AB02A19B185C122C1555F6C02B2001A6DC245E83BD9B5BC82B4D9909BC4675

These findings supported executable identity.

They did not independently establish execution authorization.

## 7. Simulated Severity and SLA

**Customer Priority:** Medium, Provisional  
**Acknowledgment Target:** 2 hours  
**Assumed Intake Time:** 11:00 AM  
**Calculated Deadline:** 1:00 PM  
**Example Acknowledgment:** 11:20 AM  
**Example SLA Outcome:** Target Met

All case timing and acknowledgment values were hypothetical training examples.

No real customer ticket was acknowledged.

Actual SLA performance was not demonstrated.

## 8. Analyst Findings

**Observed**

Wazuh detected repeated sdbinst.exe executions under SYSTEM.

The reported parent process was svchost.exe.

**Correlated**

Two reviewed events shared consistent execution characteristics.

The current executable had a valid Microsoft signature and a SHA256 hash matching the original telemetry.

The parent process hosted several Windows services, including PcaSvc.

**Interpretation**

The evidence supported a plausible legitimate Windows compatibility operation.

No malicious activity was confirmed within the reviewed scope.

**Unknowns**

The exact initiating service remained unverified.

Execution authorization was not independently confirmed.

The original parent process creation record was unavailable.

A discrepancy between displayed and source event timestamps remained unresolved.

## 9. Internal SOC Handoff

The reviewed Rule 92058 alerts involved repeated execution of sdbinst.exe on WIN-SOC.

The executable identity was supported by digital signature verification and SHA256 correlation.

The reported parent process was svchost.exe running under SYSTEM.

The running parent process hosted multiple Windows services, including PcaSvc.

The available telemetry did not establish the precise initiating service or confirm authorization.

**Next Analyst Action:** Validate the expected execution purpose before recommending case closure.

## 10. Customer Communication

A simulated nontechnical customer update was prepared for CORP-A.

The update explained that repeated Windows system activity had been detected.

It communicated that the reviewed evidence was consistent with possible normal Windows operations, while acknowledging that the exact origin remained unverified.

No confirmed compromise was reported.

No real customer communication was performed.

## 11. Case Disposition

**Classification:** Unresolved  
**Case Status:** Open, Pending Validation  
**Confirmed Compromise:** Not Established  
**Confirmed Authorization:** Not Established  
**Escalation:** Reassess if further evidence indicates unauthorized activity or customer impact

The technical exercise was completed within the available evidence.

The customer case remained open because the investigation did not conclusively establish the execution's authorization.

## 12. Lessons Learned

1. Wazuh rule level alone does not establish customer severity or compromise.
2. Process creation investigations require command line, account, parent process, and correlation analysis.
3. Matching executable hashes and valid signatures support file identity, not execution intent.
4. A service sharing a parent PID does not prove that it initiated a child process.
5. Evidence gaps must remain visible in analyst handoffs.
6. Customer updates should distinguish confirmed findings from unresolved questions.
7. Simulated SLA calculations must not be presented as real operational performance.

## 13. Final Assessment

**Module Exercise:** Completed  
**Case M06-001:** Open, Pending Validation  
**MSSP Competencies:** Partially demonstrated within the tested scope

The exercise demonstrated alert investigation, evidence correlation, guided internal reporting, customer communication, and simulated SLA reasoning.

Real customer operations, actual SLA acknowledgment, and genuine multi-customer case handling were not tested.
