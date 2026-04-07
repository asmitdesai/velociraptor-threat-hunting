# Technical Documentation: DFIR & Threat Hunting Lab Setup

**Author:** Asmit Desai

**Date:** April 7, 2026


## 1\. Executive Summary

This document details the architectural deployment of a localized **Digital Forensics and Incident Response (DFIR)** lab. The environment is designed to simulate enterprise-grade endpoint monitoring and threat hunting using **Velociraptor**. By utilizing ARM64 virtualization, this setup demonstrates a high-performance, cost-effective approach to security research on modern hardware.

## 2\. Environment Specifications

### 2.1 Hardware & Hypervisor

*   **Host Machine:** MacBook Pro M4 Pro (Apple Silicon)
    
*   **Hypervisor:** Parallels Desktop (arm64)
    
*   **Architecture:** Aarch64 / ARM64
    

### 2.2 Network Topology

The lab operates within an isolated **Host-Only** network to prevent malware escape and ensure data integrity.

*   **Subnet:** `[Internal_Subnet].0/24`
    
*   **Default Gateway:** `[Internal_Gateway].1`
    

### 2.3 Asset Inventory

| Hostname | Operating System | Role | IP Address |
| --- | --- | --- | --- |
| DFIR-Server | Ubuntu 24.04 LTS (arm64) | Velociraptor Server | [Server_IP] |
| Win11-Endpoint | Windows 11 Pro (arm64) | Target/Client | [Client_IP] |

## 3\. Server Configuration: Ubuntu 24.04

### 3.1 Persistent Networking (Netplan)

To maintain constant communication with the Windows agents, the server requires a static IP assignment on the `enp0s5` interface.

1.  Access the Netplan configuration file:
    
        sudo nano /etc/netplan/01-netcfg.yaml
        
    
2.  Apply the following static configuration:
    
        network:
          version: 2
          renderer: networkd
          ethernets:
            enp0s5:
              addresses:
                - [Server_IP]/24
              routes:
                - to: default
                  via: [Internal_Gateway].1
              nameservers:
                addresses: [8.8.8.8, 1.1.1.1]
        
    
3.  Apply the network changes:
    
        sudo netplan apply
        
    

## 4\. Velociraptor Deployment & Orchestration

### 4.1 Server Initialization

Velociraptor is initialized via the interactive configuration wizard to establish the cryptographic root of trust and network listeners.

1.  Generate the server configuration:
    
        ./velociraptor-linux-arm64 config generate -i
        
    
2.  **Core Configuration Details:**
    
    *   **Frontend Port (C2):** `8000`
        
    *   **GUI Port (Management):** `8889`
        
    *   **Public IP/DNS:** `[Server_IP]`
        
3.  Execute the server:
    
        sudo ./velociraptor-linux-arm64 --config server.config.yaml server
        
    

## 5\. Endpoint Agent Deployment

### 5.1 Artifact Generation

Within the Velociraptor GUI (`https://[Server_IP]:8889`), the `Server.Utils.CreateMSI` artifact was utilized to generate a pre-configured ARM64/x64 installer.

### 5.2 Payload Delivery & Installation

The installer was transferred from the Linux server to the Windows client using a lightweight Python-based delivery mechanism.

1.  **On Ubuntu Server (Source):**
    
        # Serve the MSI file
        python3 -m http.server 8080
        
    
2.  **On Windows 11 Endpoint (Target):**
    
    Open Command Prompt as Administrator and run:
    
        msiexec /i http://[Server_IP]:8080/vel.msi /qn
        
    

## 6\. Verification & Validation

Success is confirmed when the `Win11-Endpoint` appears in the Velociraptor Dashboard under **"Show All Clients"**.

*   **Green "Connected" Status:** Confirms active WebSocket communication.
    
*   **Telemetry Verification:** Successfully executed `Windows.System.Pslist` to confirm remote collection capabilities.
    

## 7\. Security Note & Best Practices

**Important:** A professional DFIR lab must maintain strict configuration security.

The `server.config.yaml` file contains the CA private keys and administrative credentials. To prevent accidental exposure on GitHub, ensure your `.gitignore` is configured as follows:

    # Protect Private Keys and Configurations
    server.config.yaml
    *.pem
    *.key
    writeback.yaml
    logs/
    

_Always sanitize your environment and use unique credentials before taking your lab into a production or public-facing scenario._
