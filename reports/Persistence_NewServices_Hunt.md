# Hunt Report - New Service Installation

## Objective
Monitor for the creation of new Windows Services, a common method for achieving "SYSTEM" level persistence.

## MITRE ATT&CK Mapping
- **Tactic:** Persistence ([TA0003](https://attack.mitre.org/tactics/TA0003/))
- **Technique:** Create or Modify System Process: Windows Service ([T1543.003](https://attack.mitre.org/techniques/T1543/003/))

## VQL Artifact
`Custom.Windows.Persistence.NewServices` - Flags any service running from a non-standard path (outside of Windows/System32).

## Findings
**No Anomaly Detected.** All current services are running from trusted paths (`C:\Windows\System32` or `C:\Program Files`). 

## Analyst Commentary
Establishing a clean baseline for services is critical. Future hunts will compare new snapshots against this baseline to identify "drift."
