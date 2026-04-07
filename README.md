# Velociraptor Threat Hunting 

A hands-on threat hunting environment built with Velociraptor, deployed across Ubuntu (server) and Windows (client) VMs. This project demonstrates end-to-end DFIR capability — from infrastructure deployment to custom VQL artifact development and documented hunt findings.

## Architecture

```
┌─────────────────────────────────────┐
│         MacBook M4 Pro (Host)        │
│                                     │
│  ┌──────────────┐  ┌─────────────┐  │
│  │ Ubuntu 24.04 │  │  Windows 11 │  │
│  │   (Server)   │◄─┤  (Client)   │  │
│  │ 10.211.55.10 │  │ 10.211.55.11│  │
│  └──────────────┘  └─────────────┘  │
│       Parallels Desktop (arm64)      │
└─────────────────────────────────────┘
```

- **Server:** Velociraptor v0.75.x on Ubuntu 24.04 (arm64), static IP, self-signed TLS
- **Client:** Velociraptor agent on Windows 11, enrolled via MSI package
- **Network:** Host-only virtual network, no external dependencies

## Repository Structure

```
velociraptor-threat-hunting/
├── README.md
├── server/
│   └── server.config.yaml.example     # Sanitized — no keys or certs
├── client/
│   └── client.config.yaml.example
├── artifacts/
│   └── *.yaml                         # Custom VQL artifacts
├── hunts/
│   └── *.yaml                         # Exported hunt definitions
├── reports/
│   └── *.md                           # Hunt findings and analysis
└── docs/
    └── setup.md                       # Full deployment walkthrough
```

> **Note:** Never commit real `server.config.yaml` — it contains private keys and certificate material. Use the `.example` files as reference.

## Hunts & Findings

| Hunt | Tactic | MITRE ATT&CK | Result | Report |
|------|--------|--------------|--------|--------|
| CertUtil LOLBin Download | Defense Evasion | [T1105](https://attack.mitre.org/techniques/T1105/) | No anomaly detected — clean baseline established | [Report](./reports/DefenseEvasion_CertUtil_Hunt.md) |
| Network Share Enumeration | Discovery | [T1135](https://attack.mitre.org/techniques/T1135/) | **Finding:** Non-standard share `Backup_Internal` in public path — violates least privilege | [Report](./reports/Discovery_NetworkShares_Hunt.md) |
| Suspicious PowerShell Execution | Execution | [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | **Simulated hit:** `-EncodedCommand` + `-WindowStyle Hidden` detected via `cmd.exe` parent | [Report](./reports/Execution_SuspiciousPowerShell_Hunt.md) |
| New Service Installation | Persistence | [T1543.003](https://attack.mitre.org/techniques/T1543/003/) | No anomaly detected — all services running from trusted paths; baseline captured | [Report](./reports/Persistence_NewServices_Hunt.md) |

Hunt reports live in [`/reports`](./reports/). Each report includes the VQL artifact used, raw findings, and analyst commentary.

## Custom VQL Artifacts

All artifacts are in [`/artifacts`](./artifacts/), bundled into a single fleet-wide hunt via [`hunts/Global_Threat_Hunt.yaml`](./hunts/Global_Threat_Hunt.yaml).

| Artifact | Purpose |
|----------|---------|
| `Custom.Windows.Persistence.RunKeys` | Flags binaries in Run/RunOnce registry keys executing from Temp/AppData paths |
| `Custom.Windows.Persistence.NewServices` | Detects services running outside standard Windows paths |
| `Custom.Windows.Execution.SuspiciousPowerShell` | Identifies PowerShell with obfuscation/bypass flags |
| `Custom.Windows.Discovery.NetworkShares` | Enumerates non-default network shares |
| `Custom.Windows.DefenseEvasion.CertUtilDownload` | Detects `certutil.exe` abused for payload download |

## Setup

Full deployment walkthrough in [`docs/setup.md`](./docs/setup.md), covering:

- Parallels network configuration (host-only, static IPs)
- Velociraptor server installation (Ubuntu arm64)
- Config generation and admin user creation
- Windows client packaging and MSI deployment
- Firewall rules and service configuration

## Skills Demonstrated

- Velociraptor server/client deployment from scratch
- Custom VQL artifact development for threat hunting
- DFIR workflow: hunt design → collection → triage → reporting
- MITRE ATT&CK technique mapping across Persistence, Execution, Discovery, and Defense Evasion tactics

## References

- [Velociraptor Documentation](https://docs.velociraptor.app)
- [VQL Reference](https://docs.velociraptor.app/vql_reference/)
- [Velociraptor Artifact Exchange](https://docs.velociraptor.app/exchange/)
