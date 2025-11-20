# Bastion JumpBridge
**A lightweight Windows desktop application that automates secure RDP access to Azure Virtual Machines through Azure Bastion. Built with C# WinForms (.NET Framework 4.7.2) and powered by PowerShell 5.1 for all Azure interactions.**

![Platform](https://img.shields.io/badge/platform-Windows_10%2F11-blue.svg)
![Framework](https://img.shields.io/badge/.NET-4.7.2-blueviolet.svg)
![Azure](https://img.shields.io/badge/Azure-Bastion-informational.svg)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1-5391FE.svg)

> **Note:** This repository distributes the compiled application **only**.  

---

## 📥 Download
Download the latest version here:

👉 **[Latest Release](../../releases/latest)**

Place the EXE anywhere on your system and run it directly — no installation required.

---

## 🚀 Overview
**Bastion JumpBridge** is a Windows utility that streamlines access to Azure-based Windows Virtual Machines by automating the entire Azure Bastion workflow. Instead of manually navigating through the Azure Portal, downloading RDP files, switching tenants, or managing credentials, JumpBridge provides an optimized, secure, operator-friendly interface.

It is designed for:

- CloudOps / DevOps engineers  
- MSP / multi-tenant support teams  
- Azure administrators  
- Environments with strict RDP governance  
- Users who frequently access VMs through Bastion and need faster workflows  

---

## ✨ Key Features

### 🔐 Azure Authentication
- Supports **multiple tenants** and multiple parallel Azure contexts  
- Integrated PowerShell login flow  
- Automatic detection of existing sessions  
- Optional tenant selector for switching contexts  

### 🛰 Bastion & VM Discovery
- Retrieves all **subscriptions**, **Bastion hosts**, and **Virtual Machines**  
- Fast, parallel PowerShell queries  
- Clean UI with grouping, icons, and bold formatting for quick scanning  

### 🖥 One-Click Bastion RDP Sessions
- Automatically fetches the Bastion-generated `.rdp` file  
- Cleans invalid/signature directives from RDP content  
- Starts RDP via `mstsc.exe`  
- Detects existing sessions and updates VM status  

### 🛡 Credential Manager (Local + KeyVault)
- Manage VM credentials using:
  - Manual username/password  
  - Azure KeyVault secret retrieval  
- Uses Windows Credential Manager (`cmdkey.exe`)  
- Detects Credential Guard compatibility  
- Displays current credential type per VM  

### 📡 RDP Session Monitor
- Tracks live MSTSC windows  
- Emits “session started” / “session ended” events  
- Cleans up stale RDP cache files  
- Drives floating widget updates  

### 🧩 Floating RDP Widgets
Small, draggable, snap-to-edge floating windows showing:
- VM Name  
- State and status  
- Bastion host  
- IP Address  
- Tags  
- Notes  

Automatically appear when RDP sessions start.

### 🛠 Built-in Tools
- Az module validator + fixer  
- Dependency check system  
- Custom preference system stored in registry (with schema-based cleanup)  
- Single-instance application with named pipe activation  

---

## 🏗 Technical Requirements

### **Minimum Requirements**
- **Windows 10 or Windows 11**
- **.NET Framework 4.7.2**
- **PowerShell 5.1**
- `Az.Accounts`, `Az.Compute`, `Az.Network` modules (the application can help install/fix these)
- Azure Bastion deployed in your subscription(s)
- Valid Azure permissions:
  - Read access to Subscriptions / Bastions / VMs  
  - Sign-in permissions in tenant  

### **Optional**
- Azure KeyVault access for secret-based credential injection  

---

## 🧰 How It Works (High-Level)
1. JumpBridge uses embedded **PowerShell 5.1** to retrieve:
   - Azure subscriptions  
   - Tenant identities  
   - Bastion hosts  
   - Virtual machine metadata  
2. It requests a Bastion-generated RDP file via HTTPS using an Azure Access Token.  
3. It sanitizes the RDP file and launches it with `mstsc.exe`.  
4. A custom **RDP Session Monitor** checks for active sessions and UI widgets are updated live.  

_No Azure credentials are ever stored by the application._  
Windows Credential Manager and KeyVault serve as the secure storage providers.

---

## 🔒 Security Notes
- JumpBridge never stores usernames or passwords in files.  
- Passwords from Azure KeyVault remain in-memory only.  
- Local credentials use **cmdkey.exe** for secure storage.  
- The app requires the user’s existing Azure authentication context.  
- Communication with Bastion occurs using Azure-issued tokens only.  
- RDP files are generated per-session and automatically cleaned up.  

---

## 📘 Usage Guide

### 1. Start the App  
If you are not logged in to Azure PowerShell, the app will guide you through sign-in.

### 2. Discover Bastion Hosts  
Use **Discover Bastions** to load subscriptions and Bastion resources.

### 3. Retrieve VMs  
Select any Bastion host and click **Retrieve VMs**.

### 4. Connect  
Click **Connect** on a running VM to initiate a Bastion-backed RDP session.

### 5. Manage Credentials  
Use the **Credential Manager** to save/update VM credentials locally or through KeyVault.

### 6. Widgets  
Floating widgets appear automatically when RDP sessions start.

---

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

---

## 🗨 Support & Feedback
Feel free to open issues for:
- Feature requests  
- Bug reports  
- Enhancements  
- Documentation improvements  

---

## ⭐ If You Find This Useful  
Please consider starring the repository! It helps support further development.
