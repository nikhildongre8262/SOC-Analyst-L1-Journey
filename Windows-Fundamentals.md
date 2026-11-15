# 🪟 Windows Fundamentals for SOC Analysts

> Essential Windows knowledge for security investigation - enough to investigate confidently, not enough to become a Windows kernel engineer.

## 📌 What a SOC Analyst L1 Should Know

| Area | Key Concepts |
|---|---|
| Architecture | User Mode, Kernel Mode, Drivers |
| Processes | Process Trees, Parent/Child, Executable Paths |
| Identity | Users, Sessions, Privileges |
| File System | Windows & User Directories |
| Telemetry | Event Logs, Sysmon |
| Networking | IP, Ports, DNS |
| Persistence | Scheduled Tasks, Services, Registry |
| Investigation | Timelines, Correlation, Evidence |

> **Analyst Focus:** Learn Windows from an evidence and investigation perspective, not as a system administrator.

---

## 🏗️ Windows Architecture: User Mode vs Kernel Mode

**Why does a SOC analyst care about this boundary?**

Understanding where applications run helps identify suspicious behavior, privilege abuse, and potential system-level compromise.

### 👤 User Mode

- Runs applications such as browsers, Office, and common programs.
- Processes operate with restricted access to system resources.
- Applications use system calls to request privileged operations.
- Process isolation helps prevent applications from directly accessing protected memory.

### ⚙️ Kernel Mode

- Windows Kernel and drivers execute with high privileges.
- Manages memory, scheduling, I/O, and hardware access.
- Provides core operating system functionality.
- Kernel-level compromise can impact the entire system.

### 🔍 SOC Investigation Perspective

- Identify suspicious processes and their execution paths.
- Investigate privilege escalation attempts.
- Detect abnormal driver installation or loading.
- Recognize potential kernel-level compromise.

---

## 🔎 Essential Windows Investigation Areas

### 1. Processes & Execution
- Process ID (PID) and Parent Process ID (PPID)
- Process trees and executable paths
- Command-line arguments
- Suspicious process execution

### 2. Identity & Privileges
- User accounts and logon sessions
- Administrator privileges
- Failed and successful logons
- Privilege escalation indicators

### 3. Windows File System
- `C:\Windows` — Operating system files
- `C:\Program Files` — Installed applications
- `C:\Users` — User profiles
- `AppData` — Application data and potential malware locations
- `Temp` — Temporary files and suspicious payloads

### 4. Windows Telemetry
- Windows Event Viewer
- Security, System, and Application logs
- Sysmon process and network events
- Logon, process creation, and service activity

### 5. Network Activity
- IP addresses and ports
- DNS resolution
- Active network connections
- Process-to-network correlation

### 6. Persistence Mechanisms
- Scheduled Tasks
- Windows Services
- Registry Run Keys
- Startup folders

### 7. Investigation & Correlation
- Build event timelines
- Correlate processes, users, and network activity
- Identify suspicious execution chains
- Preserve evidence and document findings

---

## 🎯 Learning Objective

Understand Windows behavior, recognize suspicious activity, investigate endpoint evidence, and support incident response as a SOC Analyst L1.

---

**Status:** 📚 Learning & Practicing | 🔐 SOC Analyst L1 Preparation
