# Essential Windows Processes

**Windows Fundamentals | SOC Analyst L1 Learning**

Understanding important Windows processes, their responsibilities, and their relevance during security investigations.

> **Recognition Anchors — Not a Memorization Contest.**

The goal is to recognize what a process does and identify suspicious activity during SOC investigations.

## 1. Core Windows Processes

| Process | Recognition Anchor |
|---|---|
| `System` | Kernel and system threads |
| `smss.exe` | Session Manager |
| `csrss.exe` | Client/Server Runtime Subsystem |
| `wininit.exe` | Windows Initialization |
| `services.exe` | Service Control Manager |
| `lsass.exe` | Local Security Authority Subsystem |
| `svchost.exe` | Host for Windows Services |
| `winlogon.exe` | Windows Logon Management |
| `explorer.exe` | Windows User Shell |

## 2. Additional Important Processes

| Process | Recognition Anchor |
|---|---|
| `dwm.exe` | Desktop Window Manager |
| `taskhostw.exe` | Hosts Windows Tasks |
| `conhost.exe` | Console Window Host |
| `dllhost.exe` | COM Surrogate |
| `spoolsv.exe` | Print Spooler Service |
| `MsMpEng.exe` | Microsoft Defender Antivirus |
| `RuntimeBroker.exe` | Manages App Permissions |
| `w3wp.exe` | IIS Worker Process (when IIS is installed) |

## 3. Understanding the Process Lifecycle

A simplified Windows startup sequence:

1. `smss.exe` – Initializes Windows sessions.
2. `wininit.exe` – Initializes essential system components.
3. `services.exe` – Starts and manages Windows services.
4. `lsass.exe` – Handles local security and authentication.
5. `svchost.exe` – Hosts services that run from DLL files.
6. `winlogon.exe` – Manages interactive user logon.

*Note: This is a simplified recognition sequence, not a complete or universal process tree.*

## 4. SOC Analyst Investigation Focus

A process name alone is not proof that a process is legitimate.

During investigation, validate:

- **File Path:** Is it running from the expected directory?
- **Parent Process:** Which process launched it?
- **User Account:** Which user or account owns it?
- **Command Line:** Are the arguments suspicious?
- **Digital Signature:** Is the executable properly signed?
- **Process Behavior:** Is it making unusual network connections, modifying files, or spawning unexpected processes?

## Analyst Focus

A familiar process name is only a starting point. Malware can impersonate legitimate Windows processes.

Always investigate its path, parent process, user, command line, digital signature, and behavior before determining whether it is legitimate or suspicious.

**Learning Goal:** Recognize normal Windows activity, identify unusual process behavior, and develop a foundation for Windows process investigation in a SOC environment.
