# 🦠 Analyze Suspicious File (invoice.exe) - CTF Challenge

## 📋 Overview

**Analyze Suspicious File (invoice.exe)** is an interactive, browser-based Capture The Flag (CTF) challenge designed for cybersecurity training. This challenge focuses on malware triage and analysis of a suspicious executable file. Participants use static and dynamic analysis techniques including VirusTotal checks, strings analysis, entropy detection, and sandbox execution to extract Indicators of Compromise (IOCs).

## 🎯 Learning Objectives

By completing this CTF, participants will learn:

- **Static Analysis**: Extract information from files without execution (hash, strings, PE headers)
- **VirusTotal Integration**: Understand multi-engine malware detection and false positives
- **Packer Detection**: Identify packed malware through entropy analysis
- **Dynamic Analysis**: Observe malware behavior in sandbox environments
- **IOC Extraction**: Compile comprehensive Indicators of Compromise
- **Malware Triage**: Prioritize and categorize malicious files efficiently

## 🛠️ Challenge Tasks (5 Total)

| Task | Description | Skill Focus |
|------|-------------|-------------|
| **Task 1** | Check file hash on VirusTotal (3 engines detect it) | Threat Intelligence |
| **Task 2** | Use strings to extract embedded IP address | Static Analysis |
| **Task 3** | Determine if file is packed (high entropy - yes) | Packer Detection |
| **Task 4** | Run in sandbox to observe registry persistence | Dynamic Analysis |
| **Task 5** | Extract all IOCs (IP, hash, filename) | IOC Compilation |

## 🚀 Quick Start

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- No server required - runs entirely in the browser
- No installation needed

### Access the Challenge
1. Open the HTML file directly in your browser
2. Enter your name
3. Use the password: `45_2026`
4. Complete all 5 tasks to capture the flag

### Hosting on GitHub Pages
1. Fork or clone this repository
2. Go to repository Settings > Pages
3. Select the branch (usually `main`) and save
4. Access via `https://your-username.github.io/repository-name`

## 🎮 How to Play

### Login
```
Password: 45_2026
Name: Enter any name (progress is saved locally)
```

### Game Features

- **Interactive Terminal**: Clickable command buttons simulating malware analysis tools
- **VirusTotal Simulation**: Multi-engine scan results with detection names
- **Strings Output**: Embedded IP address and suspicious strings extraction
- **PE Header Analysis**: Entry point, sections, imported DLLs
- **Entropy Analysis**: Section-by-section entropy scoring for packer detection
- **Sandbox Execution**: Process tree, network connections, registry changes
- **Answer Validation**: Immediate feedback on submitted answers
- **Progress Tracking**: Local storage saves your progress across sessions

### Completing Tasks
1. Read each task description carefully
2. Click terminal commands to run analysis tools
3. Analyze the outputs from each tool
4. Type your answer in the input field
5. Click "Submit" to validate
6. Complete all 5 tasks to reveal the flag

## 🏆 Flag

```
FLAG{INVOICE_EXE_IOCS}
```

The flag is revealed only after completing all 5 tasks successfully.

## 📊 Challenge Details

### Available Analysis Commands

```
analyst@triage:~$ file invoice.exe
analyst@triage:~$ md5sum invoice.exe
analyst@triage:~$ strings invoice.exe
analyst@triage:~$ pecheck invoice.exe
analyst@triage:~$ entropy invoice.exe
analyst@triage:~$ vtcheck invoice.exe
analyst@triage:~$ sandbox invoice.exe
analyst@triage:~$ clear
```

### File Information
```
File: invoice.exe
Type: PE32 executable (GUI) Intel 80386, for MS Windows
MD5: d41d8cd98f00b204e9800998ecf8427e
Compiled: 2024-03-15 14:30:22
Sections: .text, .rdata, .data, .rsrc
Entry Point: 0x00401000
```

### VirusTotal Results (3/7 Detection)

| Engine | Detection |
|--------|-----------|
| Avast | Win32:Trojan-gen |
| Kaspersky | Trojan.Win32.Agent |
| Malwarebytes | Trojan.Downloader |
| ESET | Clean |
| BitDefender | Clean |
| Windows Defender | Clean |
| McAfee | Clean |

### Strings Analysis Findings

```
Embedded IP: 192.168.100.50
Suspicious Strings:
  - /upload.php
  - Mozilla/5.0
  - POST
  - Software\Microsoft\Windows\CurrentVersion\Run
  - InvoiceHelper
  - cmd.exe /c ping
  - SeDebugPrivilege
  - OpenProcessToken
```

### Entropy Analysis
```
.text section: 7.2 (HIGH - likely packed/encrypted)
.rdata section: 6.8 (HIGH)
.data section: 5.1 (MEDIUM)
Overall entropy: 7.1
⚠️ WARNING: High entropy indicates PACKED executable!
```

### Sandbox Results
```
Process Tree:
  invoice.exe (PID: 4520) → cmd.exe (PID: 4521) → powershell.exe (PID: 4522)

Network Connections:
  TCP 192.168.100.50:443 (HTTPS)
  TCP 8.8.8.8:53 (DNS)

Registry Changes:
  SET: HKCU\Software\Microsoft\Windows\CurrentVersion\Run\InvoiceHelper

File Changes:
  CREATE: C:\Users\Victim\AppData\Local\Temp\invoice.exe
  CREATE: C:\Windows\Temp\dump.bin
```

## 🔍 Investigation Walkthrough

### Task 1: VirusTotal Check
The MD5 hash `d41d8cd98f00b204e9800998ecf8427e` is detected by **3 out of 7** engines. Key observations:
- Multiple engines detecting it suggests genuine malware
- Detection names indicate trojan/ downloader capabilities
- Some engines not detecting suggests packing/obfuscation
- Detection ratio of 3/7 is significant enough to treat as malicious

### Task 2: Strings Analysis
The embedded IP address **192.168.100.50** was found in strings output. This is significant because:
- Hardcoded IPs indicate command and control infrastructure
- Attackers often embed C2 addresses in malware
- The IP is not a standard Windows or system address
- Combined with `/upload.php`, suggests data exfiltration capability

### Task 3: Packer Detection
The file **is packed** with high entropy (7.1). This indicates:
- The malware uses packing to evade signature-based detection
- High entropy in .text section (7.2) is unusual for normal executables
- Packed malware requires unpacking for full analysis
- Packers compress/encrypt malicious code to bypass antivirus

### Task 4: Sandbox Analysis
The malware creates persistence via registry Run key:
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run\InvoiceHelper
```
This means:
- Malware survives system reboots
- Uses legitimate Run key for persistence
- Masquerades as "InvoiceHelper" to appear benign
- Runs from Temp directory to avoid suspicion

### Task 5: Extract IOCs
Complete IOCs for blocking and detection:
```
IP: 192.168.100.50
Hash: d41d8cd98f00b204e9800998ecf8427e
Filename: invoice.exe
```

## 🎨 Visual Features

- **Interactive Terminal**: Green-on-black terminal with clickable command buttons
- **VirusTotal Badges**: Color-coded malicious (red) and clean (green) engine results
- **Sandbox Panel**: Orange-bordered execution results with highlighted persistence
- **Terminal Output**: Syntax-highlighted findings with yellow highlights for key data
- **Progress Indicators**: Visual completion status for each task
- **Glowing Flag Animation**: Celebratory golden flag reveal with glow effect
- **Toast Notifications**: Non-intrusive success/error messages
- **Dark Theme**: Orange-accented UI for malware analysis theme

## 💾 Data Storage

- **Progress**: Saved in browser's `localStorage`
- **Persistence**: Progress survives page refreshes
- **Privacy**: All data stays on the user's device
- **Reset**: Clear browser data to reset progress

## 🛡️ Malware Analysis Concepts Covered

### Static Analysis Techniques
- File type identification
- Cryptographic hashing (MD5, SHA256)
- Strings extraction and analysis
- PE header examination
- Import table analysis
- Section analysis

### Dynamic Analysis Techniques
- Sandbox execution
- Process tree monitoring
- Network traffic capture
- Registry change tracking
- File system monitoring
- Persistence mechanism detection

### IOC Types Extracted
- Network indicators (IP addresses, domains, URLs)
- Host indicators (file hashes, filenames, paths)
- Behavioral indicators (registry keys, mutexes, process names)
- Tool-specific indicators (packer signatures, compilation timestamps)

## 📁 File Structure

```
analyze-suspicious-file/
│
├── index.html          # Main CTF challenge file
├── README.md           # This documentation
└── (no other files required)
```

## 🔧 Technical Implementation

- **Pure Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **No Dependencies**: Zero external libraries
- **Responsive Design**: Works on desktop and mobile
- **Terminal Simulation**: Interactive command execution with scrollable output
- **Storage**: Browser localStorage API
- **Gamification**: Progress tracking, badge system, visual rewards
- **Command Parser**: Simulated analysis tool outputs

## 📊 Malware Triage Workflow

```
1. File Discovery
   └── Identify suspicious file (invoice.exe)

2. Static Analysis
   ├── Calculate file hash
   ├── Check VirusTotal
   ├── Extract strings
   ├── Check PE headers
   └── Analyze entropy

3. Dynamic Analysis
   ├── Execute in sandbox
   ├── Monitor processes
   ├── Track network connections
   ├── Record registry changes
   └── Document file system changes

4. IOC Extraction
   ├── Compile network IOCs
   ├── Document host IOCs
   ├── Record behavioral IOCs
   └── Generate threat report

5. Response
   ├── Block IOCs
   ├── Update detection rules
   ├── Remove persistence
   └── Remediate affected systems
```

## 🎓 Educational Use Cases

- **Cybersecurity Training Programs**
- **SOC Analyst Onboarding**
- **Malware Analysis Workshops**
- **Blue Team Exercises**
- **Incident Response Training**
- **Academic Courses** (Malware Analysis, Reverse Engineering)
- **Self-paced Learning**
- **DFIR Training**

## 🔄 Version History

- **v1.0** - Initial release
  - 5 tasks with validation
  - Interactive terminal with 8 analysis commands
  - Simulated VirusTotal, strings, entropy, and sandbox outputs
  - Local storage progress tracking
  - Student login system

## 👥 Target Audience

- Security Operations Center (SOC) Analysts
- Incident Response Team Members
- Malware Analysts
- Threat Intelligence Analysts
- Digital Forensics Examiners
- Cybersecurity Students
- IT Security Professionals
- Blue Team Practitioners

---

**Happy Malware Hunting! 🦠**
