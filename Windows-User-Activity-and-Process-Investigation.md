

# 🪟 Windows User Activity & Process Investigation

> SOC Analyst L1 Learning Notes  
> Understanding user activity, process behavior, Windows artifacts, and investigation context.

---

## 📌 Overview

During a Windows investigation, a process name alone is not enough to determine whether an activity is legitimate or suspicious.

A SOC Analyst should examine multiple investigation points, including:

- Process execution path
- Parent and child processes
- User context
- Command-line arguments
- File locations
- Execution time
- Digital signature and reputation
- Related system artifacts

The goal is to understand **what happened, how it happened, and what activity followed.**

---

# 1. 🗂️ User Data, AppData, Temp & Downloads

Windows user directories can provide valuable investigation context. Temporary files, downloaded files, application data, and user activity may help identify suspicious execution or possible malware activity.

### Important Windows Locations

| Location | Investigation Relevance |
|---|---|
| `%APPDATA%` | Roaming application data and user-specific application settings |
| `%LOCALAPPDATA%` | Local application data, caches, and user-specific files |
| `%LOCALAPPDATA%\Temp` | Temporary files and possible staging or payload locations |
| `C:\Users\<username>\Downloads\` | Downloaded files that may require investigation |
| `C:\Users\Public\` | Shared user-accessible location that may be relevant during investigations |

> ⚠️ These locations are not inherently malicious. Legitimate applications also use them. Investigate the file, its origin, behavior, and surrounding activity.

### 🔎 What to Ask During Investigation?

- Is there a newly created executable or script?
- Is the filename unusual or impersonating a legitimate file?
- When was the file created or modified?
- Which user account was involved?
- Was the file downloaded or executed?
- Does its location and behavior match normal activity?

---

# 2. 🌐 Browser Artifacts

Browser artifacts can provide useful evidence of user activity and help establish investigation context.

### Potential Investigation Sources

- Browser download history
- Browsing history
- Downloaded files
- Browser-related files and caches
- Web activity context

### Investigation Questions

- Was a suspicious file downloaded?
- What was the download source?
- Does the download time correlate with suspicious process execution?
- Was the activity associated with a particular user?

> Browser artifacts may help establish the context of an event, but they should be correlated with other evidence.

---

# 3. ⚙️ Task Manager — First Look at System Activity

Task Manager is a useful starting point during initial Windows triage.

### Shortcut

`Ctrl + Shift + Esc`

### Useful Tabs

| Tab | What to Examine |
|---|---|
| Processes | Running applications and processes |
| Performance | CPU, memory, disk, and network utilization |
| Users | Active users and their resource usage |
| Details | Process-level information and identifiers |
| Services | Windows services and their current states |
| Startup apps | Programs configured to launch during startup |

### 🚨 Initial Triage Indicators

- Unexpected processes
- Unusual resource consumption
- Unknown or suspicious users
- Unexpected startup applications
- Suspicious background activity

> **Limitation:** Task Manager provides a useful initial view, but it does not replace deeper investigation using event logs, endpoint detection tools, process analysis, or other forensic evidence.

---

# 4. 🧾 Windows Artifacts — Evidence Beyond the Process List

A process may terminate or disappear, but related artifacts can still help establish that execution or activity occurred.

## Important Artifacts

| Artifact | Investigation Value |
|---|---|
| Prefetch | Execution-related evidence and application launch information |
| Amcache | Program execution and application-related metadata |
| Event Logs | Timestamped system and security events |
| Recent Activity | User activity context and recently accessed items, where available |

### 🧩 Understanding the Evidence

- **Prefetch:** Can help investigate application execution on systems where Prefetch is enabled and applicable.
- **Amcache:** Can provide information about applications and files observed by Windows.
- **Event Logs:** Help correlate events, timestamps, logon activity, system changes, and other activity.
- **Recent Activity:** May provide clues about user interaction or accessed files, depending on the artifact and its availability.

> **Key takeaway:** Evidence beyond the active process list can help reconstruct activity, even when a process is no longer running.

---

# 5. 🔍 Process Analysis

## Process Name Is Not Enough

A familiar process name does not automatically make a process legitimate.

For example, `svchost.exe` is a legitimate Windows process name, but a file using the same name in an unexpected directory requires investigation.

### Example: Process Path Comparison

**Expected location:**

```text
C:\Windows\System32\svchost.exe
```

**Unexpected location:**

```text
C:\Users\Public\svchost.exe
```

The second location is suspicious in this context and deserves further validation. However, the path alone is not proof of malware.

### 🛡️ Process Investigation Checklist

| Investigation Point | Question |
|---|---|
| File path | Is the executable running from an expected location? |
| Parent process | Which process launched it? |
| User | Which account executed it? |
| Command line | What arguments or commands were used? |
| Digital signature | Is the file signed? Is the signature valid and expected? |
| Reputation | Is the file known, suspicious, or potentially malicious? |
| Behavior | What did the process do after execution? |

### Investigation Rule

> Never classify a process based on its filename alone. Compare its location, parent process, user context, command line, signature, behavior, and related telemetry.

---

# 6. 🌳 Process Tree Analysis

A process tree represents parent-child relationships between processes.

Instead of examining a process in isolation, investigate how it was launched and what processes it created.

### Example of a Suspicious Execution Chain

```text
explorer.exe
└── winword.exe
    └── powershell.exe
        └── cmd.exe
            └── [Further child process]
```

This is an illustrative chain, not automatically malicious.

The relationship between processes may provide more investigation context than the child process name alone.

### Why Process Trees Matter

- Identify the original launching process.
- Understand the sequence of execution.
- Detect unusual parent-child relationships.
- Investigate suspicious script or command execution.
- Determine what happened before and after an alert.

### 🧠 Key Concept

> Parent → Child → Story

A process tree can help explain the execution story behind an event.

An unusual parent-child relationship may provide stronger investigative context than the child process name itself.

---

# 7. 📁 Windows File System — Know the Normal Locations

Understanding standard Windows directories helps identify files running from unusual locations.

| Directory | Purpose |
|---|---|
| `C:\Windows\System32` | Core Windows system binaries and components |
| `C:\Windows\SysWOW64` | 32-bit Windows components on 64-bit systems |
| `C:\Program Files` | Installed 64-bit applications on typical 64-bit Windows installations |
| `C:\Program Files (x86)` | Installed 32-bit applications on typical 64-bit Windows installations |
| `C:\ProgramData` | Machine-wide application data |
| `C:\Users` | User profiles and user-specific data |

### Investigation Rule

> First establish what is normal for the host. Then investigate unexpected binaries, directories, and user-writable execution locations.

An unusual location is an investigation lead, not a verdict.

---

# 8. 🧰 Windows Investigation Utilities

Native Windows utilities are useful during initial triage and system investigation.

| Utility | Command | Purpose |
|---|---|---|
| System Configuration | `msconfig` | Startup and system configuration |
| Event Viewer | `eventvwr.msc` | View and investigate Windows event logs |
| Registry Editor | `regedit` | Inspect Windows Registry configuration and entries |
| Services | `services.msc` | Examine background services and their configurations |

### Important

Native Windows utilities are part of the investigation toolkit. They help gather information, but findings should be validated and correlated with other evidence.

---

# 9. 🚨 SOC Analyst L1 — Initial Triage Checklist

When investigating a suspicious process or Windows activity, begin with these questions:

- [ ] Is the process unexpected?
- [ ] Is the executable running from an unusual location?
- [ ] Is the user or account context unexpected?
- [ ] Is the parent-child relationship suspicious?
- [ ] Are the command-line arguments unusual?
- [ ] Is there an unexpected startup item?
- [ ] Is there a resource usage spike?
- [ ] Are there related files in Temp, Downloads, AppData, or Public?
- [ ] Are browser artifacts or download activity relevant?
- [ ] Are supporting Event Logs or other artifacts available?
- [ ] Can the activity be correlated with other telemetry?

### L1 Investigation Mindset

**Observe → Validate → Correlate → Document → Escalate if required**

Do not jump to conclusions based on a single indicator.

---

# 💡 Key Takeaways

- A process name alone is not enough.
- File location matters, but it is not proof by itself.
- Parent-child relationships provide valuable execution context.
- User data locations can provide investigation leads.
- Windows artifacts may preserve evidence beyond the active process list.
- Task Manager is a starting point, not the complete investigation.
- Establish normal system behavior before treating activity as abnormal.
- Correlate evidence before making conclusions.

---

# 🙏 Learning Source & Acknowledgement

Special thanks to **Indian Cyber Club** for the learning resources, guidance, and cybersecurity knowledge that contributed to these notes.

This documentation is part of my continuous learning journey toward becoming a **SOC Analyst L1**, strengthening my Windows investigation, process analysis, and cybersecurity fundamentals.

---

*Learn consistently. Investigate carefully. Improve continuously.* 🔐

