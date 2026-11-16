# Bastion JumpBridge v2.3

**Your Windows workspace for Azure Bastion, virtual machines, and remote connections.**

> [!NOTE]
> I originally built JumpBridge for my day-to-day Azure VM management and thought others could benefit from it too. Some features, particularly sending scripts to VMs, may trigger antivirus false positives (sorry for the inconvenience!).

Bastion JumpBridge v2.3 brings Windows RDP, Linux SSH, and everyday Azure VM administration together in one desktop workspace for Azure administrators, CloudOps teams, and support engineers.

### Your Azure Inventory, Without the Manual Setup

JumpBridge takes the heavy lifting out of remote connection management by discovering your live VM inventory directly from Azure. Select your subscription and Bastion to work with the resources available to your signed-in identity, without manually building and maintaining a catalog of connection entries.

Your organization stays in control through Azure role-based access control (RBAC). Live discovery and Azure operations use your existing permissions for resources such as Bastions, virtual machines, and Key Vaults. JumpBridge does not grant additional access: viewing a resource does not automatically authorize connecting to it, changing it, or reading its secrets.

This approach supports centrally managed access and least-privilege practices that can contribute to an organization's SOC 2 control requirements. Using JumpBridge does not, by itself, establish SOC 2 compliance.

![Platform](https://img.shields.io/badge/platform-Windows_x64-blue.svg)
![Framework](https://img.shields.io/badge/.NET_Framework-4.7.2-blueviolet.svg)
![Azure](https://img.shields.io/badge/Azure-Bastion-informational.svg)
![Connections](https://img.shields.io/badge/connections-RDP_%2B_SSH-5391FE.svg)

> This repository distributes compiled releases of JumpBridge. It does not contain the application source code. The features below describe version 2.3.7; older releases may differ.

## Download

**[Download the Latest Release](../../releases/latest)**

Download the release package, extract it to a folder on your PC, and run **Bastion JumpBridge.exe**. Keep any accompanying files and folders together. No application installer is required, but the prerequisites below must be available.

## What's New in 2.3.7

- **Session Dock:** Keep active RDP and SSH sessions in one compact dock, with quick window switching, session details, and a collapsible icon rail.
- **RDP Capture:** Save PNG screenshots or record GIF and MP4 clips directly from a connected RDP session.
- **More Reliable Script Delivery:** Verify the remote Start menu before typing, track delivery progress, and stop when the target window or visual state cannot be confirmed.
- **Faster Script Management:** Assign or clear scripts across the visible VM selection and copy script content to the clipboard.
- **Smoother Everyday Use:** Continue working during automatic inventory refresh, with improved window activation, theme colors, icons, and rendering.

## Features

### Workspace and Customization

Navigate between **Dashboard**, **Bastions**, **Virtual Machines**, **Credential Manager**, **Script Library**, **Activity Log**, **Settings**, and **About**. The dashboard provides inventory summaries and shortcuts to common tasks.

Choose a theme preset, including the new Silver and Pearl options, or customize application and icon colors with a live preview. Save your preferences and return to your remembered window position and selected Bastion on the next launch. Resize the VM list and details panels using the divider; JumpBridge remembers the split and preserves VM selection and scroll position when reloading the page.

### VM Resource Monitoring

The Dashboard includes **VM Resource Monitoring (Azure Preview)** for the selected subscription. View CPU, used memory where Azure provides the metric, disk read/write activity, and network traffic. Choose **All**, **CPU**, **Mem**, **Disk**, or **Network**, and switch between the last **1, 4, or 24 hours**. Hover over the chart for values and timestamps; peak markers help identify spikes.

Monitoring summarizes reporting VMs across the subscription. It requires Azure metric-read access, and unavailable metrics are not treated as zero usage. Saved snapshots remain distinguishable from live data. Automatic monitoring pauses after one hour; choose **Resume** to start another monitoring session.

### Azure Context and Inventory

- Load saved Azure PowerShell contexts and switch between available subscriptions for the loaded account, including contexts in different tenants.
- Refresh the saved context or validate sign-in from the Azure menu.
- Inspect Bastion SKU, network configuration, native client setting, resource IDs, and tags.
- Browse Windows and Linux VMs with filters for name or resource group, power state, and operating system.
- Configure **Special Tags** in Settings to show selected Azure tag values directly in the VM list, and reorder the fields to suit your workflow.
- Restore saved Bastion and VM inventories across restarts, with indicators showing cached data and refresh times.
- Choose automatic inventory refresh on context selection and at a configurable interval, or refresh manually when needed. The default interval is **10 minutes**, adjustable from **1 to 1,440 minutes** in Settings. Automatic refresh runs in the background so you can continue working; user actions take priority.

Saved inventory can reflect earlier access and resource state. Refresh to retrieve current data; connecting also requires a valid network path from the selected Bastion to the VM.

### Windows RDP and Linux SSH

**Windows:** Select a running Windows VM and choose **Connect** to launch Windows Remote Desktop through Azure Bastion. JumpBridge prepares the RDP connection using a descriptive VM-based filename and tracks matching Remote Desktop windows. You can also double-click a VM to connect.

**Linux:** Import an SSH private key, select a running Linux VM, and choose **Connect** to open a Windows OpenSSH terminal through a Bastion tunnel. JumpBridge manages the tunnel and session cleanup. Linux SSH requires a **Standard or Premium Bastion with Native Client enabled**.

SSH uses key-based authentication. Passphrase-protected private keys are not supported in this version. On first connection, verify the server fingerprint before accepting it; subsequent connections check the saved host key. An open terminal alone does not confirm a successful login.

### Credentials in One Place

- Apply a Windows username and password to one or more selected Windows VMs.
- Retrieve a password from Azure Key Vault and save it for use with RDP.
- Import a local SSH private key or a Key Vault secret containing a private key for selected Linux VMs.
- Review saved credential status and remove stored credentials when they are no longer needed. Selecting a VM can prefill its assigned credential details, and Linux key indicators in the VM list help identify configured keys.
- Keep an imported copy of SSH keys, including source and fingerprint metadata, so connections do not depend on the original key file remaining in place.

Key Vault imports are saved copies. Re-import an SSH key when the source secret changes.

### VM Details, Diagnostics, and Power Controls

View VM power and provisioning state, OS, size, private IP address, image, agent information, availability zones, tags, and Azure resource details. Colored status badges and bold text for connected VMs make active sessions easier to find. Load expanded VM agent details on demand.

Open **Resource Health** to inspect current status and recent issues, or view the **Boot Diagnostic Screenshot**, which refreshes while open when available.

Enable **Power Actions** in Settings to use **Start**, **Stop**, **Restart**, and **Redeploy** from the VM detail panel. Stop deallocates the VM. Power actions can run on different VMs concurrently, with progress tracked per VM. Stopping local monitoring does not cancel an operation already accepted by Azure. These actions and diagnostics require the corresponding Azure permissions and feature availability.

### Linux File Transfer and Editing

Right-click a running Linux VM and choose **File Transfer** to open local and remote file panes. JumpBridge uses the VM's assigned SSH username/key and a dedicated Bastion tunnel; an SSH terminal does not need to be open.

- Upload and download files or folders, with a transfer queue, byte progress, speed, cancellation, and **Retry Selected** for failed or cancelled transfers.
- Navigate with Back, Forward, Up, and Home; enter paths directly, show hidden files, and filter the current folder with matching file/folder counts.
- Inspect file sizes, dates, types, and owners. Rename or permanently delete selected files and folders with confirmation.
- Resolve destination conflicts with **Skip**, **Keep Both**, or **Replace**, including case-sensitive Linux names downloaded to Windows.
- View and edit supported text files up to **4 MB**, preserving supported encoding and line endings. Remote saves check whether the file changed since it was opened.
- Create ZIP archives locally and supported archives remotely when the required `tar` or `zip` utility is available.

File transfers require an SFTP subsystem on the Linux VM and use SSH host-key verification. If a remote text save fails because of permissions, JumpBridge can retry using non-interactive sudo and Python 3 when the VM's existing sudo policy permits it. It cannot prompt for a sudo password during this retry.

Completed transfers remain after cancellation. Interrupted transfers may leave a partial file; reconnect and retry explicitly. Transfer resume, folder synchronization, drag-and-drop, and binary editing are not included.

### Reusable Script Library

Keep common administration tasks in **Script Library** instead of repeatedly finding and copying script files.

- Create and edit **PowerShell**, **Bash**, and **Batch** scripts with syntax highlighting, formatting, and familiar editing shortcuts.
- Assign saved scripts to compatible VMs for quick access from the VM details panel. Bulk selection applies to the visible VM list, making filtered assignments easier.
- Copy the current script content to the clipboard for use elsewhere.
- Import `.ps1`, `.sh`, `.bat`, or `.cmd` files. Choose **Replace Content** to retain a selected script's settings and VM assignments, or **Import As New** to create a separate, unassigned script.
- Export your saved scripts and assignments as a JSON catalog, then import them into another JumpBridge library. Matching script IDs are updated; unrelated scripts are retained.

Enable **Interactive**, **Non-Interactive**, or both for a script; when both are enabled, choose the mode when executing. VM status badges show script activity.

Choose **Save Script** to keep your edits and assignments. Importing a script or catalog does not execute it.

| Execution Mode | How It Works |
| --- | --- |
| Interactive | Runs through an existing RDP or SSH session. Enable **Run Elevated** to request Windows administrator approval or Linux sudo authentication. |
| Non-Interactive | Uses Azure Run Command on a standalone VM without an open RDP or SSH session. Runs as SYSTEM on Windows or root on Linux and returns output to JumpBridge. |

For Bash scripts, choose a new terminal or **Existing Terminal**. Existing-terminal execution uses its current directory and exported environment; start from an idle, empty command line and leave keyboard and mouse input alone during delivery.

Interactive RDP delivery visually verifies a supported remote Start menu before typing, checks clipboard readiness and receipt, and reports delivery stages. It supports windowed and full-screen sessions without installing a helper on the VM. Keep the target RDP window focused during delivery. Unsupported or unclear layouts stop delivery rather than allowing unverified typing; not every Windows layout or language is supported. Receipt confirms delivery, not successful script completion. Non-interactive execution requires a working VM agent and Azure Run Command permissions. Scripts in this mode cannot prompt for input, and returned output is limited to the last 4 KB. VM scale set script execution is not supported. **Stop Waiting** stops local monitoring; it does not cancel an accepted Azure execution.

### Session Dock and Widgets

Choose **Session Dock Mode** in Settings to bring active RDP and SSH sessions into one panel. Double-click a session to return to its window, use the information icon for details, and collapse the dock to an icon rail when space is limited. Pin it expanded or let it collapse when idle; its position and display state are remembered.

Prefer separate floating windows? Choose **Widget Mode** to show VM, subscription, Bastion, private IP, tags, and session context beside your work. Both modes follow the visibility preference: always, only when JumpBridge is minimized, or off.

### RDP Screenshots and Recordings

Right-click a connected Windows VM and choose **Capture > Screenshot** or **Capture > Record**. Capture is also available from an RDP widget. If a VM has multiple RDP windows, choose the one to capture.

- Save screenshots as **PNG**, or record video-only clips as **GIF** or **MP4**.
- Choose recording presets from **5 seconds to 5 minutes**, stop early to save a shorter clip, or cancel to discard it.
- See capture and saving progress, then open the saved file or its folder. Completed recordings show duration and file size.
- Capture the selected remote desktop window rather than the surrounding local desktop. Saved images and recordings include a subtle JumpBridge watermark.

Capture restores and focuses the selected RDP window. Keep it focused and at the same size; switching to another application, resizing, or closing the target aborts capture. The recorder's Stop and Cancel controls remain usable.

Screenshots and recordings are saved locally under **Capture/Screenshots** and **Capture/Recordings** beside the application by default. An alternative destination can be selected when needed. Capture requires Windows 10 version 1903 or later; MP4 also requires Windows Media Foundation and H.264 support. No capture software is installed on the VM.

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
| Linux File Transfer | Linux SSH prerequisites plus an SFTP subsystem; remote archive creation requires the relevant archive utilities |
| VM Monitoring | Azure permissions to read VM inventory and metrics; metric availability depends on the VM |
| Key Vault Imports | `Az.KeyVault` and permission to list/read the required vault secrets |
| Interactive Scripts | A connected RDP or SSH session; RDP delivery also requires clipboard redirection |
| RDP Capture | Windows 10 version 1903 or later; MP4 requires Media Foundation and H.264 support |
| Non-Interactive Scripts | A standalone Azure VM with a working VM agent and Azure Run Command permissions |

JumpBridge reports missing prerequisites but does not install them automatically. Inventory discovery does not require `Az.Compute` or `Az.Network`.

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
8. **Manage Linux files.** Right-click a running Linux VM and choose **File Transfer** to browse, transfer, or edit files using its assigned SSH key.
9. **Review monitoring.** Open **Dashboard** after validating Azure access to inspect resource activity and choose a time range.
10. **Make it yours.** Use **Settings** to choose the refresh interval, session dock or widgets, visibility, theme presets or custom colors, Special Tags and their display order, and whether power actions are available.

## Local Data and Credential Storage

JumpBridge uses your Azure PowerShell sign-in; you do not enter your Azure account password into JumpBridge.

- **Windows VM credentials** are stored in Windows Credential Manager, including passwords imported from Key Vault.
- **Imported SSH private keys** are encrypted with Windows DPAPI for the current Windows user and stored in the local application database. OpenSSH sessions use temporary private-key files with restricted access, which JumpBridge cleans up after use.
- **Inventory, activity history, dashboard and monitoring snapshots, and the script library** are stored locally, normally in `%LOCALAPPDATA%\BastionJumpBridge\cache.db`. If that location cannot be initialized because of a storage-access problem, the app may use a database beside the EXE. Saved inventory and activity can contain resource names, IDs, and operational details.
- **Saved scripts and exported catalogs** contain script content and VM assignments. Script content is not encrypted by the library; avoid embedding passwords, tokens, or private keys, and review catalogs before sharing them.
- **Screenshots and recordings** are saved locally and contain the remote desktop content you capture. Review them before sharing.
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
