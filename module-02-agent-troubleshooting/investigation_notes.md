# Module 02: Investigation Notes

**Project:** [Wazuh Operations Lab](../README.md)  
**Module:** [Windows Agent Management and Troubleshooting](README.md)  
**Environment:** Wazuh 4.14.6 | Docker | WIN-SOC  
**Date:** November 19, 2026  
**Status:** Completed  
**Assessment:** Demonstrated within the tested scope

## 1. Investigation Context

Performed a controlled Windows Wazuh agent service interruption to validate service troubleshooting, recovery, and Manager connectivity.

The exercise focused on WIN-SOC, Agent ID 004, running Wazuh Agent 4.14.9.

## 2. Verified Observations

1. WazuhSvc initially reported Running.
2. The service was intentionally stopped using Stop-Service.
3. A subsequent service query confirmed Stopped.
4. The service was restored using Start-Service.
5. A subsequent service query confirmed Running.
6. WIN-SOC Agent 004 subsequently reported Active in the Wazuh Dashboard.

## 3. Analyst Assessment

The controlled interruption and recovery were successfully demonstrated.

Windows service status and Wazuh Manager connectivity were verified separately.

The interruption was intentional and does not represent an unexpected production failure.

## 4. Evidence Gaps

The Manager-side Disconnected state was not captured during the interruption.

Exact disconnection and reconnection times were not established.

Continuous event collection during the interruption and complete telemetry recovery were not verified.

## 5. Disposition

**Classification:** Controlled operational troubleshooting  
**Result:** Completed  
**Escalation:** Not required for the planned laboratory exercise

WazuhSvc returned to Running, and WIN-SOC subsequently reported Active.

No unresolved service failure was identified at the conclusion of testing.

## 6. Analyst Handoff

**Current Known State:**

WIN-SOC Agent 004 was Active following service recovery. The Windows WazuhSvc service was Running at the final service check.

**Next Exercise:** Windows Log Collection and Troubleshooting.

**Next Analyst Actions:**

1. Confirm WIN-SOC remains Active.
2. Inspect the configured Windows event collectors in ossec.conf.
3. Compare intended log sources with actual collected telemetry.
4. Investigate any missing event channel.
5. Verify event reception at the Wazuh Manager after remediation.

**Important:** An Active agent does not guarantee complete Windows event collection.

**Handoff Status:** Ready for Module 03.
