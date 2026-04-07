# Hunt Report - CertUtil LOLBin Usage

## Objective
Detect "Living off the Land" (LotL) techniques where legitimate Windows binaries are abused to download malicious payloads.

## MITRE ATT&CK Mapping
- **Tactic:** Defense Evasion ([TA0005](https://attack.mitre.org/tactics/TA0005/))
- **Technique:** Protocol Impersonation / Ingress Tool Transfer ([T1105](https://attack.mitre.org/techniques/T1105/))

## VQL Artifact
`Custom.Windows.DefenseEvasion.CertUtilDownload` - Monitors for `certutil.exe` usage containing the `-urlcache` or `-split` arguments.

## Findings
**No Anomaly Detected.** No active or historical processes matching this signature were found during the collection window.

## Analyst Commentary
`CertUtil` is a high-fidelity indicator. Legitimate use of `-urlcache` in an enterprise environment is extremely rare, making any hit a high-confidence alert for investigation.
