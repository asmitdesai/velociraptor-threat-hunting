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

| Hunt | Technique | MITRE ATT&CK | Report |
|------|-----------|--------------|--------|
| *(in progress)* | | | |

Hunt reports live in [`/reports`](./reports/). Each report includes the VQL used, raw findings, and analyst commentary.

## Custom VQL Artifacts

Custom artifacts are in [`/artifacts`](./artifacts/). Each artifact targets a specific threat behavior and is designed to be portable across deployments.

## Setup

Full deployment walkthrough in [`docs/setup.md`](./docs/setup.md), covering:

- Parallels network configuration (host-only, static IPs)
- Velociraptor server installation (Ubuntu arm64)
- Config generation and admin user creation
- Windows client packaging and MSI deployment
- Firewall rules and service configuration

## Skills Demonstrated

- Velociraptor server/client deployment from scratch
- VQL artifact development for threat hunting
- DFIR workflow: hunt design → collection → triage → reporting
- MITRE ATT&CK technique mapping

## References

- [Velociraptor Documentation](https://docs.velociraptor.app)
- [VQL Reference](https://docs.velociraptor.app/vql_reference/)
- [Velociraptor Artifact Exchange](https://docs.velociraptor.app/exchange/)
