# Module 03: Investigation Notes

**Project:** [Wazuh Operations Lab](../README.md)  
**Module:** [Windows Log Collection and Troubleshooting](README.md)  
**Environment:** Wazuh 4.14.6 | Docker | WIN-SOC  
**Date:** November 19, 2026  
**Status:** Completed  
**Assessment:** Demonstrated within the tested scope

## 1. Investigation Context

Investigated a missing PowerShell Operational event collector on WIN-SOC, Agent 004.

The objective was to correct the collection configuration and verify actual event reception by the Wazuh Manager.

## 2. Verified Observations

1. The Windows PowerShell Operational channel was enabled and contained existing events.
2. The Windows agent configuration lacked an explicit collector for this channel.
3. Added the required eventchannel configuration to the agent's ossec.conf.
4. Validated XML syntax and restarted WazuhSvc.
5. The agent log confirmed initialization of the PowerShell Operational collector.
6. Wazuh Manager received PowerShell Operational Event ID 40962 from Agent 004.
7. Enabled PowerShell Script Block Logging on WIN-SOC for controlled validation.
8. Windows generated Event ID 4104, Record ID 2805, containing the validation marker WAZUH-M03-4104-VALIDATION.
9. The Manager archive contained the matching Event ID 4104 and Record ID 2805 from WIN-SOC.
10. Restored Manager logall_json to no and successfully restarted Wazuh Manager.

## 3. Analyst Assessment

The missing collector was corrected, and ingestion of the tested PowerShell Operational events was confirmed.

Matching the agent identity, event channel, Event ID, and Record ID established that the controlled Windows Event ID 4104 reached the Manager.

Event collection and alert generation were assessed separately.

No Event ID 4104 alert generation was demonstrated.

## 4. Evidence Gaps

Complete PowerShell telemetry coverage was not established.

Long term ingestion reliability and delivery under failure conditions were not tested.

Previously archived raw event records were not separately verified as deleted.

PowerShell Script Block Logging remains enabled on WIN-SOC for laboratory monitoring.

## 5. Disposition

**Classification:** Windows event collection configuration issue  
**Result:** Resolved within the tested scope  
**Escalation:** Not required for the completed laboratory exercise

The configured PowerShell Operational collector successfully delivered the tested events.

The temporary Manager raw archiving configuration was restored to its original disabled state.

## 6. Analyst Handoff

**Current Known State:**

WIN-SOC has the PowerShell Operational event collector configured.

Event IDs 40962 and 4104 were independently observed in the Wazuh Manager archive during validation.

Manager raw event archiving was disabled after testing, and the Manager restart completed successfully.

**Next Exercise:** File Integrity Monitoring.

**Next Analyst Actions:**

1. Confirm WIN-SOC connectivity before beginning the next exercise.
2. Inspect the existing File Integrity Monitoring configuration.
3. Establish the monitored directory baseline.
4. Perform controlled file activity and verify the resulting Wazuh events.
5. Distinguish confirmed file changes from unsupported user attribution.

**Important:** Event ingestion does not guarantee alert generation. Maintain separate validation of collector configuration, actual event reception, and detection rule processing.

**Handoff Status:** Ready for Module 04.
