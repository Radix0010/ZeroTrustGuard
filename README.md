# ZeroTrustGaurd
Zero trust platform for learning
# 🛡️ ZeroTrustGuard

**ZeroTrustGuard** is a host-based **zero trust application control platform for Windows** designed to prevent unauthorized code execution using a **default-deny security model**.

This project explores real-world defensive security concepts including **application allowlisting, process control, and policy-driven enforcement**, similar in philosophy to platforms like ThreatLocker.

---

## 🚀 Overview

Traditional antivirus and EDR tools often detect threats *after execution*.
ZeroTrustGuard takes a different approach:

> **If it’s not explicitly trusted, it does not run.**

The platform evaluates processes at runtime using a configurable policy engine and enforces rules such as:

* Blocking execution from high-risk directories
* Restricting parent-child process relationships
* Allowing only trusted publishers and system paths
* Logging all decisions for auditing and investigation

---

## 🔑 Key Features

* 🔒 **Default-Deny Execution Model**

  * Unknown or untrusted binaries are blocked by default

* 📁 **Path-Based Controls**

  * Deny execution from:

    * `AppData`
    * `Temp`
    * `Downloads`

* 🧾 **Publisher & Trust Validation**

  * Allow trusted publishers (e.g., Microsoft, Google)

* 🔗 **Parent-Child Process Restrictions**

  * Block common attack chains (e.g., Office → PowerShell)

* 📊 **Policy-Driven Engine**

  * JSON-based policy configuration
  * Easy to modify and extend

* 📝 **Logging & Audit Trail**

  * Every decision (allow/deny) is recorded with reason

* ⚙️ **Windows Service Architecture**

  * Built using .NET Worker Service for persistent background execution

---

## 🧠 Why This Project?

This project was built to deepen understanding of:

* Zero Trust Security Models
* Application Allowlisting
* Host-Based Intrusion Prevention (HIPS)
* Windows Process Monitoring
* Defensive Security Engineering

It serves as both a **learning platform** and a **portfolio project** demonstrating practical cybersecurity skills.

---

## 🏗️ Architecture

```text
[ Policy Engine ]
        ↓
[ Trust Evaluator ]
        ↓
[ Decision Engine ]
        ↓
 ┌───────────────┬────────────────┐
 │               │                │
[Execution]   [Ringfencing]   [Logging]
 │               │                │
 └───────────────┴────────────────┘
        ↓
[ Windows Service ]
```

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/ZeroTrustGuard.git
cd ZeroTrustGuard
```

---

### 2. Build the Project

```bash
dotnet build
```

---

### 3. Run Locally (Development Mode)

```bash
dotnet run --project src/ZeroTrustGuard.Service
```

---

### 4. Install as Windows Service

```powershell
cd scripts
.\install-service.ps1
```

---

## 📄 Configuration

Policies are defined in:

```
policies/policy.json
```

### Example Policy

```json
{
  "mode": "audit",
  "defaultDecision": "deny",
  "trustedPaths": [
    "C:\\Windows\\System32\\",
    "C:\\Program Files\\"
  ],
  "trustedPublishers": [
    "Microsoft Windows"
  ],
  "deniedPathContains": [
    "\\AppData\\",
    "\\Temp\\"
  ],
  "blockedParentChildRules": [
    {
      "parent": "WINWORD.EXE",
      "child": "powershell.exe"
    }
  ]
}
```

---

## 🧪 Testing

> ⚠️ Recommended: Use a virtual machine

### Basic Test Cases

| Scenario                 | Expected Result |
| ------------------------ | --------------- |
| Run EXE from `System32`  | ✅ Allowed       |
| Run EXE from `Downloads` | ❌ Denied        |
| Word spawning PowerShell | ❌ Denied        |

---

## 📊 Example Log Output

```text
DENY
Image: C:\Users\User\Downloads\evil.exe
Reason: Path contains denied fragment: \Downloads\
```

---

## 🔄 Modes of Operation

| Mode        | Description                    |
| ----------- | ------------------------------ |
| Learning    | Collect baseline behavior      |
| Audit       | Log only, no blocking          |
| Enforced    | Actively block based on policy |
| Break-Glass | Temporary bypass mode          |

---

## 🛣️ Roadmap

### v0.1 (Current)

* ✅ Policy engine
* ✅ Trust evaluation
* ✅ Logging system

### v0.2

* 🚧 Real-time process monitoring (ETW)
* 🚧 SQLite event storage
* 🚧 CLI enhancements

### v0.3

* 🔜 Network control (WFP)
* 🔜 Ringfencing improvements

### v1.0

* 🔜 GUI dashboard
* 🔜 Multi-host support
* 🔜 Tamper protection

---

## ⚠️ Disclaimer

This project is intended for:

* Educational use
* Security research
* Controlled lab environments

**Do NOT deploy in production systems.**

---

## 📌 Author

Built as part of a cybersecurity learning journey focused on:

* Blue Team skills
* Endpoint protection
* Defensive tooling

---

## ⭐ If you found this interesting

Give the repo a star ⭐ and follow the progress as it evolves into a full zero-trust platform.
