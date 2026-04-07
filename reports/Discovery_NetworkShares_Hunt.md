# Hunt Report - Network Share Enumeration

## Objective
Identify unauthorized or overly permissive network shares that could be used for lateral movement or data exfiltration.

## MITRE ATT&CK Mapping
- **Tactic:** Discovery ([TA0007](https://attack.mitre.org/tactics/TA0007/))
- **Technique:** Network Share Discovery ([T1135](https://attack.mitre.org/techniques/T1135/))

## VQL Artifact
`Custom.Windows.Discovery.NetworkShares` - Filters out standard system shares (C$, ADMIN$, IPC$) to highlight user-created shares.

## Findings
The hunt identified a non-standard share on **ASMITDESAI86C5**:
- **Share Name:** `Backup_Internal`
- **Path:** `C:\Users\Public\Documents\Backups`
- **Remark:** "Temporary backup for migration"

## Analyst Commentary
While this share is not explicitly malicious, it contains user-writable data in a public directory, which violates the Principle of Least Privilege. In a real-world breach, an attacker could use this as a staging area.
