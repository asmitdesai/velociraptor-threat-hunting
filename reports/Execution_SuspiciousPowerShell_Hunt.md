# Hunt Report - Suspicious PowerShell Execution

## Objective
Detect highly obfuscated or hidden PowerShell processes typically used by downloaders, beacons, or ransomware.

## MITRE ATT&CK Mapping
- **Tactic:** Execution ([TA0002](https://attack.mitre.org/tactics/TA0002/))
- **Technique:** Command and Scripting Interpreter: PowerShell ([T1059.001](https://attack.mitre.org/techniques/T1059/001/))

## VQL Artifact
`Custom.Windows.Execution.SuspiciousPowerShell` - Flags flags like `-EncodedCommand`, `-WindowStyle Hidden`, and `-Bypass`.

## Findings
**Simulated Hit Detected:**
- **Process:** `powershell.exe`
- **Command Line:** `powershell.exe -WindowStyle Hidden -EncodedCommand RwBlAHQALQBQAHIAbwBjAGUAcwBzAA==`
- **Parent:** `cmd.exe`

## Analyst Commentary
The use of `-EncodedCommand` is a major red flag as it is a primary method for evading string-based command line monitoring. The decoded command was found to be a benign `Get-Process` call, but the *method* of execution is consistent with malicious behavior.
