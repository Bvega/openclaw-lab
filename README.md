# OpenClaw Lab Environment

This repository documents the setup, configuration, and custom scripts for my home AI lab running OpenClaw.

## Hardware Inventory

*   **Primary Server (The "Brain")**: Mac Mini M4
    *   **OS**: macOS
    *   **IP Address**: `192.168.254.117`
    *   **Role**: Hosts the core OpenClaw instance and local LLMs.

*   **Workstation (Control Center)**: Windows Laptop
    *   **Role**: Used for development, monitoring, and interacting with the agent.

*   **Edge Device (The "Specialist")**: Jetson Orin Nano 8GB Dev Kit
    *   **Role**: Dedicated for vision processing and robotics tasks.

*   **Storage (The "Library")**: QNAP TS-219P NAS
    *   **IP Address**: `192.168.254.79`
    *   **MAC Address**: `00:08:9B:C4:C4:2E`
    *   **Role**: Centralized storage for knowledge base, datasets, and backups. Mounted to the Mac Mini.

## Network Architecture

*   **Type**: Local Ethernet LAN
*   **Subnet**: 192.168.254.x (Assumed based on Mac IP)

## Getting Started

1.  **Repository Setup**:
    *   The OpenClaw repository is cloned locally in `./openclaw`.
    *   To keep it updated: `cd openclaw && git pull`

2.  **SSH Access (Control Center to Brain)**:
    *   An SSH key pair has been generated in `ssh_keys/`.
    *   **Action Required**: Append the content of `ssh_keys/id_ed25519.pub` to `~/.ssh/authorized_keys` on the Mac Mini (`192.168.254.117`).
    *   **Connect**:
        ```bash
        ssh -i ssh_keys/id_ed25519 user@192.168.254.117
        ```

3.  **Configuration**:
    *   Copy `.env.example` to `.env` (if applicable) and fill in your API keys.

## Useful Commands

*   **Connect to Mac Mini**: `ssh -i ssh_keys/id_ed25519 user@192.168.254.117`
*   **Access OpenClaw Dashboard**: `http://192.168.254.117:18789`
