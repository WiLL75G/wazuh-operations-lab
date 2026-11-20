# Module 04: Investigation Notes

**Project:** [Wazuh Operations Lab](../README.md)  
**Module:** [File Integrity Monitoring](README.md)  
**Environment:** Wazuh 4.14.6 | Docker | WIN-SOC  
**Date:** November 20, 2026  
**Status:** Completed  
**Assessment:** Demonstrated within the tested scope

## 1. Investigation Context

Validated real time File Integrity Monitoring on WIN-SOC, Agent 004, through controlled file creation, modification, and deletion.

The objective was to confirm event detection, assess file integrity changes, and establish the limits of user attribution.

**Monitored Directory:** `C:\NORTHSTAR-HR-Lab`  
**Test File:** `M04-FIM-TEST.txt`

## 2. Verified Observations

1. The Windows agent configuration contained an enabled FIM section with `realtime="yes"` and `check_all="yes"` for the monitored directory.
2. The directory existed and was initially empty.
3. File creation generated Wazuh Rule 554, level 5, with `syscheck.event` reporting `added`.
4. File modification generated Wazuh Rule 550, level 7, with `syscheck.event` reporting `modified`.
5. The modification changed the recorded file size from 47 to 93 bytes, together with MD5, SHA1, SHA256, and modification time.
6. File deletion generated Wazuh Rule 553, level 7, with `syscheck.event` reporting `deleted`.
7. All three alerts identified the controlled test file and reported real time monitoring.
8. The final directory inspection returned no files or folders.

## 3. Analyst Assessment

Wazuh successfully detected all three controlled file operations within the monitored directory.

The file paths, event types, timestamps, and integrity metadata were consistent with the authorized PowerShell activity.

The alerts represented valid detections of expected laboratory changes.

No malicious activity was established.

The recorded owner, `Administrators`, did not identify the account or person responsible for the operations.

## 4. Evidence Gaps

The available FIM events did not independently establish the acting user or process.

Attribution would require correlated Windows file access auditing or process telemetry.

Complete FIM coverage, continuous detection reliability, and production performance were not assessed.

## 5. Disposition

**Classification:** Authorized laboratory file activity  
**Detection Result:** True positive for the observed file changes  
**Security Incident:** Not established  
**Escalation:** Not required for the controlled exercise

All three expected file operations were detected.

The controlled test file was removed, and the directory returned to its initial empty state.

## 6. Analyst Handoff

**Current Known State:**

WIN-SOC has real time FIM configured for `C:\NORTHSTAR-HR-Lab`.

Wazuh Rules 554, 550, and 553 were verified for the controlled test file.

The monitored directory was empty at the final inspection.

**Next Module:** Joint SOC × DFIR Investigations.

**Next Analyst Actions:**

1. Confirm the current WIN-SOC monitoring state before beginning a new investigation.
2. Establish the scope and expected baseline for the next scenario.
3. Review relevant Wazuh alerts and correlate available Windows telemetry.
4. Separate observed activity from interpretation and unresolved attribution.
5. Record the investigation disposition and any required follow-up.

**Important:** FIM confirms monitored file changes but does not automatically establish the responsible user, process, or malicious intent.

**Handoff Status:** Ready for Module 05.
