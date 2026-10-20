# Bastion JumpBridge v2.1

**Your Windows workspace for Azure Bastion, virtual machines, and remote connections.**

Bastion JumpBridge v2.1 brings Windows RDP and Linux SSH connections together with Azure inventory, VM diagnostics, credential management, reusable scripts, and session tracking in a redesigned desktop interface. Built for Azure administrators, CloudOps teams, and support engineers who move between subscriptions and virtual machines throughout the day.

![Platform](https://img.shields.io/badge/platform-Windows_x64-blue.svg)
![Framework](https://img.shields.io/badge/.NET_Framework-4.7.2-blueviolet.svg)
![Azure](https://img.shields.io/badge/Azure-Bastion-informational.svg)
![Connections](https://img.shields.io/badge/connections-RDP_%2B_SSH-5391FE.svg)

> This repository distributes compiled releases of JumpBridge. It does not contain the application source code. The features below describe version 2.1.0; older releases may differ.

## Download

**[Download the Latest Release](../../releases/latest)**

Download the release package, extract it to a folder on your PC, and run **Bastion JumpBridge.exe**. Keep any accompanying files and folders together. No application installer is required, but the prerequisites below must be available.

## What's New in 2.1.0

- **Script Library:** Create PowerShell, Bash, and Batch scripts, assign them to VMs, and share saved libraries through JSON import/export.
- **Flexible Script Execution:** Run scripts interactively through RDP or SSH, or use Azure Run Command without an open remote session.
- **A More Flexible Workspace:** Choose your inventory refresh interval, resize the VM list and details panels, and use additional theme presets.
- **Easier Session Navigation:** Double-click VMs to connect, return to SSH sessions through floating widgets, and identify connected VMs with clearer status indicators.

## Features

### A Redesigned Workspace

Dedicated pages for **Dashboard**, **Bastions**, **Virtual Machines**, **Credential Manager**, **Script Library**, **Activity Log**, **Settings**, and **About** keep everyday tasks within reach. The dashboard summarizes your selected workspace and inventory, with charts and shortcuts to common tasks.

Choose a theme preset or customize the application's colors with a live preview. Save your preferences and return to your remembered window position and selected Bastion on the next launch. Resize the VM list and details panels using the divider; JumpBridge remembers the split and preserves VM selection and scroll position when reloading the page.

### Azure Context and Inventory

- Load saved Azure PowerShell contexts and switch between available subscriptions for the loaded account, including contexts in different tenants.
- Refresh the saved context or validate sign-in from the Azure menu.
- Discover Bastion hosts and inspect their SKU, network configuration, native client setting, resource IDs, and tags.
- Browse Windows and Linux VMs with filters for name or resource group, power state, and operating system.
- Restore saved Bastion and VM inventories across restarts, with indicators showing cached data and refresh times.
- Choose automatic inventory refresh on context selection and at a configurable interval, or refresh manually when needed. The default interval is **10 minutes**, adjustable from **1 to 1,440 minutes** in Settings.

Inventory visibility does not guarantee that a VM is reachable through the selected Bastion. Connections still depend on your Azure permissions and network configuration.

### Windows RDP and Linux SSH

**Windows:** Select a running Windows VM and choose **Connect** to launch Windows Remote Desktop through Azure Bastion. JumpBridge prepares the RDP connection using a descriptive VM-based filename and tracks matching Remote Desktop windows. You can also double-click a VM to connect.

**Linux:** Import an SSH private key, select a running Linux VM, and choose **Connect** to open a Windows OpenSSH terminal through a Bastion tunnel. JumpBridge manages the tunnel and session cleanup. Linux SSH requires a **Standard or Premium Bastion with Native Client enabled**.

SSH uses key-based authentication. Passphrase-protected private keys are not supported in this version. On first connection, verify the server fingerprint before accepting it; subsequent connections check the saved host key. An open terminal alone does not confirm a successful login.

### Credentials in One Place

- Apply a Windows username and password to one or more selected Windows VMs.
- Retrieve a password from Azure Key Vault and save it for use with RDP.
- Import a local SSH private key or a Key Vault secret containing a private key for selected Linux VMs.
- Review saved credential status and remove stored credentials when they are no longer needed.
- Keep an imported copy of SSH keys, including source and fingerprint metadata, so connections do not depend on the original key file remaining in place.

Key Vault imports are saved copies. Re-import an SSH key when the source secret changes.

### VM Details, Diagnostics, and Power Controls

View VM power and provisioning state, OS, size, private IP address, image, agent information, availability zones, tags, and Azure resource details. Colored status badges and bold text for connected VMs make active sessions easier to find. Load expanded VM agent details on demand.

Open **Resource Health** to inspect current status and recent issues, or view the **Boot Diagnostic Screenshot**, which refreshes while open when available.

Enable **Power Actions** in Settings to use **Start**, **Stop**, **Restart**, and **Redeploy** from the VM detail panel. Stop deallocates the VM. These actions and diagnostics require the corresponding Azure permissions and feature availability.

### Reusable Script Library

Keep common administration tasks in **Script Library** instead of repeatedly finding and copying script files.

- Create and edit **PowerShell**, **Bash**, and **Batch** scripts with syntax highlighting, formatting, and familiar editing shortcuts.
- Assign saved scripts to compatible VMs for quick access from the VM details panel.
- Import `.ps1`, `.sh`, `.bat`, or `.cmd` files. Choose **Replace Content** to retain a selected script's settings and VM assignments, or **Import As New** to create a separate, unassigned script.
- Export your saved scripts and assignments as a JSON catalog, then import them into another JumpBridge library. Matching script IDs are updated; unrelated scripts are retained.

Choose **Save Script** to keep your edits and assignments. Importing a script or catalog does not execute it.

| Execution Mode | How It Works |
| --- | --- |
| Interactive | Runs through an existing RDP or SSH session. Enable **Run Elevated** to request Windows administrator approval or Linux sudo authentication. |
| Non-Interactive | Uses Azure Run Command on a standalone VM without an open RDP or SSH session. Runs as SYSTEM on Windows or root on Linux and returns output to JumpBridge. |

Interactive RDP delivery checks remote readiness and script receipt; receipt does not confirm successful completion. Non-interactive execution requires a working VM agent and Azure Run Command permissions. Scripts in this mode cannot prompt for input, and returned output is limited to the last 4 KB. VM scale set script execution is not supported. **Stop Waiting** stops local monitoring; it does not cancel an accepted Azure execution.

### Session Widgets

Floating widgets keep VM and session details close at hand while you work in RDP or SSH. See the VM, subscription, Bastion, private IP, tags, and other context, then double-click a widget to return to its session window.

Choose whether widgets appear always, only when JumpBridge is minimized, or remain off.

### Searchable Activity History

Review discovery, authentication, connection, and VM-operation activity across application restarts. Search saved history, filter by subscription, date, severity, or category, inspect the steps behind an operation, copy event details, and export activity to CSV.

### In-App Updates

JumpBridge checks for updates at startup. You can also check from **About** and use **Install Update** when a newer release is available. Updates verify the signed release manifest and package contents before installation, then restart the application. You can defer installation when sessions or Azure work are active.

In-app installation depends on a published signed update feed. If update information is unavailable, use the release download link above.

## Requirements

| Component | Requirement |
| --- | --- |
| Operating System | 64-bit Windows with Windows Remote Desktop available for RDP |
| Runtime | .NET Framework 4.7.2 or later |
| PowerShell | Windows PowerShell 5.1 with `Az.Accounts` |
| Azure Sign-In | A saved Azure PowerShell context for the account and subscriptions you use |
| Azure Resources | An existing Bastion and target VMs, with suitable permissions and network connectivity |
| Linux SSH | Windows OpenSSH Client (`ssh.exe` and `ssh-keygen.exe`), an imported private key, and a Standard or Premium Bastion with Native Client enabled |
| Key Vault Imports | `Az.KeyVault` and permission to list/read the required vault secrets |
| Interactive Scripts | A connected RDP or SSH session; RDP delivery also requires clipboard redirection |
| Non-Interactive Scripts | A standalone Azure VM with a working VM agent and Azure Run Command permissions |

JumpBridge reports missing prerequisites; it does not automatically install PowerShell modules or Windows features. The v2 discovery workflow uses `Az.Accounts`; `Az.Compute` and `Az.Network` are not required for inventory discovery.

## Getting Started

1. **Prepare Azure PowerShell.** If needed, install the account module in **Windows PowerShell 5.1**, then sign in:

   ```powershell
   Install-Module Az.Accounts -Scope CurrentUser -Force
   Connect-AzAccount
   ```

   JumpBridge reads saved contexts. If your environment disables context autosave, the context must be saved before JumpBridge can load it.

2. **Open JumpBridge.** Choose **Azure > Refresh Context** after signing in, then select the subscription you want to work with.
3. **Choose a Bastion.** Open **Bastions**, refresh the inventory if needed, and select the host to use.
4. **Load your VMs.** Open **Virtual Machines** and choose **Load VMs** or **Refresh VMs** as needed. Select a VM to inspect its details.
5. **Set up credentials.** In **Credential Manager**, apply Windows credentials or assign an SSH key to your selected VMs. For Key Vault imports, install `Az.KeyVault` if it is not already available.
6. **Connect.** Return to **Virtual Machines** and choose **Connect** on a running VM. Windows opens in Remote Desktop; Linux opens in an SSH terminal.
7. **Save reusable scripts.** Open **Script Library**, create or import a script, choose its execution mode, assign compatible VMs, and select **Save Script**. Run assigned scripts from the VM details panel when needed.
8. **Make it yours.** Use **Settings** to choose the refresh interval, widget visibility, theme presets or custom colors, and whether power actions are available.

## Local Data and Credential Storage

JumpBridge uses your Azure PowerShell sign-in; you do not enter your Azure account password into JumpBridge.

- **Windows VM credentials** are stored in Windows Credential Manager, including passwords imported from Key Vault.
- **Imported SSH private keys** are encrypted with Windows DPAPI for the current Windows user and stored in the local application database. OpenSSH sessions use temporary private-key files with restricted access, which JumpBridge cleans up after use.
- **Inventory, activity history, dashboard snapshots, and the script library** are stored locally, normally in `%LOCALAPPDATA%\BastionJumpBridge\cache.db`. If that location cannot be initialized because of a storage-access problem, the app may use a database beside the EXE. Saved inventory and activity can contain resource names, IDs, and operational details.
- **Saved scripts and exported catalogs** contain script content and VM assignments. Script content is not encrypted by the library; avoid embedding passwords, tokens, or private keys, and review catalogs before sharing them.
- **Application preferences** are saved locally in the Windows registry. History remains available across restarts and has no automatic expiration.

## Support and Feedback

Use this repository's **Issues** tab for bug reports and feature requests. For a bug report, include your JumpBridge version, the affected feature, steps to reproduce, and relevant Activity Log messages. Remove credentials, private keys, tokens, and sensitive resource details before sharing logs or screenshots.

## 📄 License
MIT License

Copyright (c) 2025 NullByte Creations

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights 
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell 
copies of the Software, and to permit persons to whom the Software is 
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in  
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR  
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,  
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE  
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER  
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,  
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN  
THE SOFTWARE.
