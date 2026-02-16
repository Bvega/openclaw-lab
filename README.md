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

antigravity
1.  **Repository Setup**:
    *   The OpenClaw repository is cloned locally in `./openclaw`.
    *   To keep it updated: `cd openclaw && git pull`

2.  **SSH Access (Control Center to Brain)**:
    *   An SSH key pair has been generated in `ssh_keys/`.
    *   **Action Required**: Append the content of `ssh_keys/id_ed25519.pub` to `~/.ssh/authorized_keys` on the Mac Mini (`192.168.254.117`).
    *   **Connect**:
        ```bash
        ssh -i ssh_keys/id_ed25519 miniboli@192.168.254.117
        ```

3.  **Configuration**:
    *   Copy `.env.example` to `.env` (if applicable) and fill in your API keys.

## Installation Log (Fresh Start - 2026-02-14)

### 1. Cleanup
*   Removed existing `openclaw` directory.
*   Stopped all old Node processes (`pm2`, `node`).
*   Verified clean state.

### 2. Dependencies & Build
*   Installed global tools: `npm install -g pnpm pm2`
*   Cloned fresh repository: `git clone https://github.com/openclaw/openclaw.git openclaw`
*   Installed dependencies: `cd openclaw && pnpm install`
*   Built the project: `pnpm ui:build && pnpm build` (This ensures the UI and backend are compiled).

### 3. Verification
*   Started gateway: `pm2 start scripts/run-node.mjs --name openclaw -- gateway --port 18789 --verbose`
*   Verified listening port: `lsof -i :18789` (shows `node` listening).
*   Logs check: verified `~/.pm2/logs/openclaw-error.log` and `openclaw-out.log`.

## Testing / Verification Guide

To interact with your Mac Mini (Brain) from your Windows Laptop (Control Center), follow these steps:

### 1. Direct LAN Access
The OpenClaw Gateway is configured to listen on the LAN interface `0.0.0.0` (port 18789) on the Mac Mini. You can access it directly from your Windows Laptop without an SSH tunnel.

**Dashboard URL:**
[http://192.168.254.117:18789](http://192.168.254.117:18789)

*(Note: Local access requires authentication. Use the token provided in your `.env` file or use the Magic Link generated during setup.)*

### 2. Configure Local Client (Windows CLI)
To use the `openclaw` command-line tool from Windows:

1.  Install the CLI globally:
    ```powershell
    npm install -g openclaw
    ```
2.  Configure CLI to point to the remote gateway:
    ```powershell
    # Set the remote URL in your local config or environment
    $env:OPENCLAW_GATEWAY_URL="http://192.168.254.117:18789"
    $env:OPENCLAW_GATEWAY_TOKEN="<your-token>"
    ```

### 3. Verify Connection
Once configured:

1.  **Browser Check**: Open [http://192.168.254.117:18789](http://192.168.254.117:18789). You should see the OpenClaw dashboard.
2.  **CLI Check**:
    ```powershell
    openclaw agent --message "System check: are you online?"
    ```

## Useful Commands

*   **Connect to Mac Mini**: `ssh -i ssh_keys/id_ed25519 miniboli@192.168.254.117`
*   **Access OpenClaw Dashboard**: `http://192.168.254.117:18789`
