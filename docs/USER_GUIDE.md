# SAIF User Guide: Setup, Browser Onboarding & Operational Guide

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Changelog (v1.0.1)**](../CHANGELOG.md) • [**Architecture Diagrams**](ARCHITECTURE.md) • [**User Guide**](USER_GUIDE.md) • [**Benchmark Whitepaper**](BENCHMARK.md) • [**Rule Catalog**](RULE_CATALOG.md) • [**Overrides & Snoozing**](OVERRIDES_AND_SNOOZE.md) • [**FAQ**](FAQ.md) • [**Benchmark Suite**](../benchmark/README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

Welcome to the **SAIF Free Community Edition (Release v1.0.1)**! This guide walks you through setting up SAIF on your workstation in under 2 minutes.

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


1. Download **[`SAIF-Setup.exe`](../releases/SAIF-Setup.exe)** (Release v1.0.1, 19.69 MB).
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

## 6. IDE & CLI Tool Protection (Antigravity, Claude Code, Copilot, Cline)

SAIF protects developer AI tools directly inside your editor and terminal. When running the installer with **Full Developer Protection**, SAIF configures `NODE_EXTRA_CA_CERTS` and local proxy routing automatically.

### Supported Tools:
* **Antigravity IDE**: Inherits `NODE_EXTRA_CA_CERTS` and proxy settings automatically. Outbound code context, files attached to chat, and prompt completions are inspected locally.
* **Claude Code (`@anthropic-ai/claude-code`)**: Inherits user-level environment variables; prompts and file reads are governed transparently.
* **GitHub Copilot & Copilot Chat**: Protects outbound code context from leaking proprietary secrets or unredacted credentials.
* **Cline, Roo Code & Continue.dev**: Compatible via standard proxy settings or custom `base_url` pointing to `http://127.0.0.1:18080/v1`.

---

## 7. Workstation CLI Commands

Manage SAIF directly from your terminal (PowerShell, cmd, or bash):

```powershell
# Check agent status, listening port, and active model profile
saif status

# Pause firewall protection (transparent bypass mode)
saif off                 # Pause indefinitely
saif pause 15m           # Pause for 15 minutes (or 5m, 1h)

# Resume active inspection and DLP enforcement
saif on

# Run a passive 5-point diagnostic health check
saif doctor

# Automatically repair certificates, trust store, and environment variables
saif repair              # Run active self-healing
saif repair --dry-run    # Preview actions without modifying system
```

---

## 8. macOS and Linux Support

Windows is the initial launch platform for the Public Beta. Native packages for **macOS (Apple Silicon & Intel DMG)** and **Linux (deb / rpm / systemd)** are currently under active development on our near-term roadmap.
