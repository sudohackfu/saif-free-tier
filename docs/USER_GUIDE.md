# SAIF User Guide: Setup, Browser Onboarding & Operational Guide

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Changelog (v1.0.4)**](../CHANGELOG.md) • [**Architecture Diagrams**](ARCHITECTURE.md) • [**User Guide**](USER_GUIDE.md) • [**Benchmark Whitepaper**](BENCHMARK.md) • [**Rule Catalog**](RULE_CATALOG.md) • [**Overrides & Snoozing**](OVERRIDES_AND_SNOOZE.md) • [**FAQ**](FAQ.md) • [**Benchmark Suite**](../benchmark/README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

Welcome to the **SAIF Free Community Edition (Release v1.0.4)**! This guide walks you through setting up SAIF on your workstation in under 2 minutes.

---

## 1. Quick Installation (Windows)

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'darkMode': true,
    'background': '#09090b',
    'mainBkg': '#121217',
    'nodeBorder': '#27272a',
    'textColor': '#f4f4f5',
    'fontFamily': 'Inter, system-ui, -apple-system, sans-serif',
    'fontSize': '13px',
    'lineColor': '#38bdf8'
  }
}}%%
flowchart LR
    A["📥 Download SAIF-Setup.exe"]:::blueNode --> B["⚙️ Run Installer (User Space)"]:::slateNode
    B --> C["⚡ Daemon Starts (127.0.0.1:18080)"]:::purpleNode
    B --> D["📂 Extension Folder Opened"]:::slateNode
    D --> E["🧩 Load Unpacked in Browser"]:::cyanNode
    C & E --> F["🛡️ Zero-Trust Protection Active!"]:::greenNode

    classDef blueNode fill:#0f172a,stroke:#3b82f6,stroke-width:2px,color:#60a5fa,rx:8px,ry:8px;
    classDef slateNode fill:#18181b,stroke:#3f3f46,stroke-width:1.5px,color:#d4d4d8,rx:8px,ry:8px;
    classDef cyanNode fill:#083344,stroke:#06b6d4,stroke-width:2px,color:#22d3ee,rx:8px,ry:8px;
    classDef purpleNode fill:#2e1065,stroke:#8b5cf6,stroke-width:2px,color:#c084fc,rx:8px,ry:8px;
    classDef greenNode fill:#022c22,stroke:#10b981,stroke-width:2px,color:#34d399,rx:8px,ry:8px;
```

1. Download **[`SAIF-Setup.exe`](../releases/SAIF-Setup.exe)** (Release v1.0.4, 19.85 MB).
2. Run `SAIF-Setup.exe`.
   - **No Administrator Rights Required**: SAIF runs 100% in your user space (`%LOCALAPPDATA%\Programs\SAIF`).
   - The installer automatically starts the background daemon and prepares your browser extension.
3. Upon completion, the installer automatically opens:
   - Your browser to the Extensions management page (`chrome://extensions`).
   - The permanent extension folder in Windows Explorer:
     `%LOCALAPPDATA%\Programs\SAIF\extension`

---

## 2. Enabling the Browser Extension

Because SAIF is an open-source security tool distributed directly via GitHub, it is loaded as an unpacked developer extension:

### In Google Chrome:
1. Open `chrome://extensions` in your browser.
2. Enable the **Developer mode** toggle in the top-right corner.
3. Click **Load unpacked** in the top-left corner.
4. Select the opened folder:
   `%LOCALAPPDATA%\Programs\SAIF\extension`
5. The **SAIF Edge Interceptor** shield icon will appear in your browser toolbar.

> [!NOTE]
> **Chrome Off-Store Extension Notice**:  
> When Google Chrome restarts, it displays a standard informational prompt: *"Disable developer mode extensions"*.  
> Simply click **Cancel** or press **Esc**. Protection continues seamlessly. (Chromium browsers like **Brave** and **Microsoft Edge** do not show this prompt).

### In Brave Browser:
1. Open `brave://extensions`.
2. Enable **Developer mode** (top-right).
3. Click **Load unpacked** and select `%LOCALAPPDATA%\Programs\SAIF\extension`.

### In Microsoft Edge:
1. Open `edge://extensions`.
2. Enable **Developer mode** in the left sidebar.
3. Click **Load unpacked** and select `%LOCALAPPDATA%\Programs\SAIF\extension`.

---

## 3. Verifying Protection

Once the extension is loaded, test your protection immediately:

1. Open your favorite web AI chat:
   - [ChatGPT (chatgpt.com)](https://chatgpt.com)
   - [Claude (claude.ai)](https://claude.ai)
   - [Google Gemini (gemini.google.com)](https://gemini.google.com)
   - [DeepSeek (chat.deepseek.com)](https://chat.deepseek.com)
2. Try typing a mock AWS access key:
   ```text
   AKIAIOSFODNN7EXAMPLE
   ```
3. Click Send. SAIF intercepts the prompt before it leaves your browser, displays the in-page shield modal, and offers local redaction or a deliberate break-glass override.

---

## 4. Selecting Your Production Model

Click the SAIF shield icon in your browser toolbar and click **Open Full Dashboard** to access the **Agent Execution Profiles** switcher:

* **`SAIF Light`** (~35MB RAM, ~13ms P50): Zero-overhead deterministic regex and entropy scanner. Ideal for battery life and low-spec laptops.
* **`SAIF Neural`** (~146MB RAM, ~4.6ms P50, 512 tokens): Balanced contextual neural cascade for everyday developer AI conversations.
* **`SAIF Deep Neural`** (~396MB RAM, ~4.8ms P50, 8,192 tokens): Sovereign flagship neural engine for full-file source code pasting and large git diffs.

Toggling profiles updates the local daemon instantly without restarting your browser or daemon.

---

## 5. Egress Inspection Modes

SAIF includes a three-mode Egress Policy Engine accessible via the extension popup or the **System Health & Architecture** tab in the dashboard:

* **`AI Only` (Default & Recommended)**:  
  Provides full real-time input protection across **all** websites (Google, GitHub, Jira, Pastebin, intranet apps) while restricting background network wire inspection exclusively to AI provider endpoints (`chatgpt.com`, `claude.ai`, `gemini.google.com`, `deepseek.com`, etc.). Background analytics or telemetry packets from third-party non-AI services do not trigger false-alarm blocks.
* **`Audit Only` (Monitor & Log)**:  
  Runs all rule evaluation engines across all traffic without interrupting your workflow. Violations are recorded as simulated blocks (`SIMULATED BLOCK`) in your local audit history for compliance and security posture review without showing blocking modals.
* **`Strict` (Enforce All)**:  
  Enforces zero-trust blocking across all web inputs, search engines, and background network traffic universally.

---

## 6. Antigravity IDE & Antigravity CLI (`agy`) Hook Setup

SAIF v1.0.4 integrates directly with **Google Antigravity IDE** and the **Antigravity CLI (`agy`)** through native lifecycle interception hooks.

Unlike brittle HTTP proxies or TLS certificate injection that break Google Cloud certificate pinning, SAIF utilizes the official **Antigravity Hook Protocol (`PreInvocation` and `PreToolUse`)**. Outbound user prompts and agent tool invocations are scanned synchronously in-memory before wire transmission. If sensitive data (API keys, PII, internal IP addresses, corporate secrets) is detected, SAIF triggers native desktop triage or blocks execution before any bytes leave the workstation.

### Configuration (`hooks.json`)

To enable protection across all Antigravity sessions on your machine, configure your **Global Hooks** file (or colocated in individual workspaces):

* **Global Path**: `%USERPROFILE%\.gemini\config\hooks.json`
* **Workspace Path**: `<workspace-root>\.agents\hooks.json`

> [!WARNING]
> **Encoding Critical Requirement: Save as UTF-8 (NO BOM)**:  
> `hooks.json` **MUST** be saved strictly as **UTF-8 without BOM** (pure UTF-8, **not** `UTF-8 with BOM` / `UTF-8-BOM`).  
> If saved with a UTF-8 BOM (`0xEF 0xBB 0xBF`), Antigravity's Go JSON parser will fail on startup with `invalid character '\ufeff' looking for beginning of value` and silently ignore or reject all configured lifecycle hooks!  
> * **In VS Code / Antigravity IDE**: Look at the bottom right status bar. If it shows `UTF-8 with BOM`, click it and select **Save with Encoding** ➔ **UTF-8**.  
> * **In PowerShell**: Do **not** use `Set-Content -Encoding UTF8` (Windows PowerShell 5.1 inserts a BOM). Use:  
>   `[System.IO.File]::WriteAllText($path, $json, [System.Text.UTF8Encoding]::new($false))`

> [!IMPORTANT]
> **Executable Path Specification**:  
> Configure `"command"` using the full absolute path to `saif.exe`, replacing `<username>` with your Windows account name (e.g. `C:\\Users\\<username>\\AppData\\Local\\Programs\\SAIF\\saif.exe hook`). Specifying the full path guarantees that Antigravity launches the hook reliably even before the IDE reloads the user `PATH` environment variable. In JSON, backslashes must be escaped (`\\`).

Add the following configuration:

```json
{
  "saif-firewall": {
    "PreInvocation": [
      {
        "type": "command",
        "command": "C:\\Users\\<username>\\AppData\\Local\\Programs\\SAIF\\saif.exe hook",
        "description": "Scans outbound prompts & conversation context before dispatch to LLM",
        "timeout": 15
      }
    ],
    "PreToolUse": [
      {
        "matcher": "run_command|write_to_file|replace_file_content|multi_replace_file_content|view_file",
        "hooks": [
          {
            "type": "command",
            "command": "C:\\Users\\<username>\\AppData\\Local\\Programs\\SAIF\\saif.exe hook",
            "description": "Validates agent tool actions & code modifications before local execution",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

### Native 3-Button Break-Glass Triage Dialog

When a sensitive secret or policy violation is detected in an outbound user prompt during `PreInvocation`, SAIF displays an interactive native Win32 dialog offering three distinct actions:

| Action | Button | Behavior & Guarantees |
| :--- | :---: | :--- |
| **Break Glass** | **`[Break Glass]`** | Authorizes an audited emergency override. Logs the security override event to the local SAIF daemon audit trail and permits the prompt to transmit raw to the AI service. |
| **Redact** | **`[Redact]`** | Sanitizes sensitive tokens in-place directly within the active session transcript (`transcript_full.jsonl`) with masked labels (e.g., `[REDACTED_SSN]`), posts an ephemeral notification in the chat UI, and proceeds with the safe sanitized turn. |
| **Block** | **`[Block]`** | Immediately purges the sensitive violation from the local session transcript (`transcript_full.jsonl`) so subsequent turns cannot accidentally bundle or leak the confidential text, then halts the turn cleanly via a circuit breaker error step. |

> [!NOTE]
> **Stateless Chat Replay Prevention & Deduplication**:  
> Cloud LLM chats are stateless; each subsequent turn bundles and re-transmits the complete conversation transcript. By immediately sanitizing `transcript_full.jsonl` upon **Redact** or **Block**, SAIF guarantees that aborted or sanitized secrets are never leaked in subsequent queries. In addition, multi-turn agent execution loops (`invocationNum > 0`) are automatically deduplicated so you are only prompted once per user submission.

### How It Works:
1. **Zero Proxy Overhead**: Runs directly via stdin/stdout IPC without listening proxies or breaking TLS certificate pinning.
2. **Synchronous Dual-Layer Interception**: Prompts and tool parameters are evaluated against the local Cedar policy engine and regex pattern manifest in < 5ms.
3. **Fail-Closed Security with In-Place Remediation**: If a secret or policy violation is detected, SAIF halts transmission immediately and provides the developer with full control over override, in-place redaction, or clean termination.

---

## 7. Windows System Tray Controls

SAIF runs quietly in the Windows System Tray (notification area). Right-click the SAIF shield icon to manage protection and inspection profiles on the fly:

| # | Tray Menu Control | Type | Description |
|:---:|:---|:---:|:---|
| **1** | **`SAIF Free Tier - Active / Paused`** | Status Header | Displays the live running state, daemon uptime, and port (`127.0.0.1:18080`). |
| **2** | **`Protection Enabled`** | Toggle | Instant protection pause/resume toggle. When unchecked, firewall enters transparent bypass. |
| **3** | **`Engine Profiles ▸`** | Submenu | Select active intelligence profile: **SAIF Light** (deterministic regex), **SAIF Neural** (512-token BERT cascade), or **SAIF Deep Neural** (8,192-token code scanner). |
| **4** | **`Diagnostics ▸`** | Submenu | Run self-healing diagnostics: **Run Doctor Health Check** (terminal audit) or **Run System Repair** (environment cleanup). |
| **5** | **`Stream Prompts / API Wire`** | Action | Spawns a dedicated PowerShell streaming terminal displaying live wire payloads, DLP tokens, and AI egress events. |
| **6** | **`Open Web Dashboard`** | Action | Opens the local Next.js telemetry & audit log portal in your default browser (`http://127.0.0.1:18080/dashboard`). |
| **7** | **`Documentation & Guide`** | Action | Opens the local offline User Setup Guide and operational documentation. |
| **8** | **`About SAIF Free Tier`** | Dialog | Displays current version (`v1.0.4`), local build hash, license terms, and community support links. |
| **9** | **`Exit SAIF`** | Action | Gracefully terminates the background daemon process and releases local port bindings. |

---

## 8. Workstation CLI Reference

SAIF is fully manageable from any terminal (PowerShell, Command Prompt, Windows Terminal, or bash):

```powershell
# 1. Daemon Health & Status
saif status              # Check agent status, listening port, and active model profile
saif doctor              # Run passive 5-point diagnostic health check

# 2. Protection Controls
saif on                  # Resume active inspection and DLP enforcement
saif off                 # Pause firewall protection indefinitely (transparent bypass)
saif pause 15m           # Temporarily snooze protection for 15 minutes (or 5m, 1h)

# 3. Native IDE Hook Interceptor
saif hook                # Evaluates JSON hook payloads from Antigravity IDE / agy CLI via stdin

# 4. Self-Healing & Diagnostics
saif repair              # Run active self-healing (cleans stale proxies and trust store)
saif repair --dry-run    # Preview repair actions without modifying environment

# 5. Runtime Flags
saif --debug             # Start daemon with verbose live debug logging enabled
saif --profile neural    # Launch with specific profile (light | neural | deep)
saif --version           # Display binary version and release build metadata
saif --help              # Display comprehensive command help
```

---

## 9. Supported Environments & Ecosystem Roadmap

SAIF is committed to securing AI developer workflows across all modern IDEs, CLI tools, and web interfaces:

| Environment | Integration Mechanism | Status | Notes |
|:---|:---|:---:|:---|
| **Google Chrome / Chromium / Edge / Brave** | Manifest V3 Edge Interceptor | **Live (v1.0.4)** | Full front-door modal interception, redaction, and egress gating. |
| **Google Antigravity IDE** | Native `PreInvocation` Hook | **Live (v1.0.4)** | Direct lifecycle hook integration; zero proxy or TLS certificate friction. |
| **Antigravity CLI (`agy`)** | Native CLI Hook Protocol | **Live (v1.0.4)** | Terminal prompt & context streaming protection. |
| **OpenAI Codex & Codex CLI** | Native Stdin Interceptor & Wrapper | **Active Development** | Dedicated pre-submission security wrapper. |
| **Cursor IDE** | Custom Extension & Agent Hook | **Active Development** | Intercepts agent composer and multi-file code attachments. |
| **Anthropic Claude Code** | Terminal Interceptor | **Active Development** | Direct prompt and tool execution inspection. |
| **JetBrains (IntelliJ, PyCharm, WebStorm)** | Native Plugin SDK Interceptor | **Active Development** | JVM-level AI assistant egress security module. |

---

## 10. macOS and Linux Support

Windows is the primary platform for the current release. Native packages for **macOS (Apple Silicon & Intel DMG)** and **Linux (deb / rpm / systemd)** are currently under active development.

