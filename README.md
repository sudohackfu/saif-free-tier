# Semantic AI Firewall (SAIF)
### The Zero-Trust Generative AI Security Layer for Developers

[![Release: v1.0.5](https://img.shields.io/badge/Release-v1.0.5-blue.svg)](releases/SAIF-Setup.exe)
[![License: MIT](https://img.shields.io/badge/Benchmark%20License-MIT-green.svg)](LICENSE.md)
[![EULA: Community](https://img.shields.io/badge/Binary%20License-Community%20EULA-purple.svg)](EULA.md)
[![Latency SLA](https://img.shields.io/badge/P50%20Latency-%3C15ms-brightgreen.svg)](docs/BENCHMARK.md)
[![Catch Rate](https://img.shields.io/badge/Adversarial%20Catch-100%25-success.svg)](docs/BENCHMARK.md)
[![Windows](https://img.shields.io/badge/Platform-Windows%20(macOS%20%26%20Linux%20Soon)-orange.svg)](docs/USER_GUIDE.md)

**100% On-Device Prompt Interception • Zero Cloud Dependencies • Zero Telemetry**

> **Documentation Index**:  
> [**Overview**](README.md) • [**Changelog (v1.0.5)**](CHANGELOG.md) • [**Architecture Diagrams**](docs/ARCHITECTURE.md) • [**User Guide**](docs/USER_GUIDE.md) • [**Benchmark Whitepaper**](docs/BENCHMARK.md) • [**Rule Catalog**](docs/RULE_CATALOG.md) • [**Overrides & Snoozing**](docs/OVERRIDES_AND_SNOOZE.md) • [**FAQ**](docs/FAQ.md) • [**Benchmark Suite**](benchmark/README.md) • [**Empirical Report**](BENCHMARK_REPORT.md) • [**Community EULA**](EULA.md) • [**MIT License**](LICENSE.md)

---

## Download & Quickstart (Release v1.0.5)

| Asset | Platform / Target | Size | Description |
| :--- | :--- | :--- | :--- |
| **[`SAIF-Setup.exe`](releases/SAIF-Setup.exe)** | Windows 10 / 11 (64-bit) | 19.85 MB | Native Inno Setup single-action installer (Zero UAC, zero proxy, native IDE hook, automatic PATH integration). |
| **[`saif-extension.zip`](releases/saif-extension.zip)** | Chromium (Chrome, Brave, Edge) | 124.6 KB | Pre-packaged Manifest V3 browser extension for unpacked developer installation. |

```powershell
# Quick install: execute user-space installer (no administrator privileges required)
Start-Process .\releases\SAIF-Setup.exe

# Terminal control commands (available immediately in any terminal via user PATH)
saif status      # Check agent state, listening port, and active model profile
saif pause 15m   # Temporarily pause firewall (transparent bypass mode)
saif on          # Resume active inspection and DLP enforcement
saif hook        # Native in-process hook bridge for Antigravity IDE and agy CLI
saif doctor      # Run 5-point workstation diagnostic health check
saif repair      # Auto-remediate workstation issues & clean legacy proxy variables
saif --debug     # Live interactive terminal wire stream (raw un-truncated JSON)
```

For complete setup and IDE onboarding, see the [**User Guide**](docs/USER_GUIDE.md).  
Detailed release notes and version history are tracked in the [**Changelog**](CHANGELOG.md).

---

## Visual Showcase: Protection in Action

SAIF operates transparently on your local workstation without synthetic testbed mockups. Here is what real-time enforcement looks like in everyday development:

### 1. In-Page AI Prompt Interception & Local Redaction
When sensitive credentials, keys, or proprietary data are entered into AI web apps (Claude, ChatGPT, Gemini, DeepSeek), SAIF halts outbound transmission before the request reaches the network. Developers can choose to auto-sanitize tokens locally or trigger an authorized break-glass override:

<p align="center">
  <a href="docs/screenshots/inpage_ai_chat_interception.png" title="Click to view full resolution">
    <img src="docs/screenshots/inpage_ai_chat_interception.png" alt="In-Page AI Chat Interception" width="680" style="max-width: 100%; border-radius: 8px; border: 1px solid #334155; box-shadow: 0 4px 16px rgba(0,0,0,0.35);" />
  </a>
  <br>
  <sub><em>(Click image to view full resolution)</em></sub>
</p>

### 2. Ambient In-Composer Risk Warning
Before submission, SAIF monitors composer input in real-time, displaying a non-intrusive preflight risk badge if sensitive variables or confidential tokens are detected:

<p align="center">
  <a href="docs/screenshots/inpage_preflight_risk_card.png" title="Click to view full resolution">
    <img src="docs/screenshots/inpage_preflight_risk_card.png" alt="Ambient Preflight Risk Card" width="680" style="max-width: 100%; border-radius: 8px; border: 1px solid #334155; box-shadow: 0 4px 16px rgba(0,0,0,0.35);" />
  </a>
  <br>
  <sub><em>(Click image to view full resolution)</em></sub>
</p>

### 3. Omnibox (Address Bar) Search Interception
Developers frequently copy/paste API tokens, internal URLs, or credentials directly into their browser address bar, inadvertently submitting them to public search engine query logs. SAIF intercepts Omnibox navigation at the socket layer and blocks the query before it leaves the machine:

<p align="center">
  <a href="docs/screenshots/omnibox_blocked_showcase.png" title="Click to view full resolution">
    <img src="docs/screenshots/omnibox_blocked_showcase.png" alt="Omnibox Address Bar Search Interception" width="640" style="max-width: 100%; border-radius: 8px; border: 1px solid #334155; box-shadow: 0 4px 16px rgba(0,0,0,0.35);" />
  </a>
  <br>
  <sub><em>(Click image to view full resolution)</em></sub>
</p>

### 4. Workstation Options Console & Model Triad
Full visibility into active protection engines, algorithmic guards, and hot-swappable local models (`SAIF Light`, `SAIF Neural`, and `SAIF Deep Neural`):

<p align="center">
  <a href="docs/screenshots/dashboard_showcase.png" title="Click to view full resolution">
    <img src="docs/screenshots/dashboard_showcase.png" alt="SAIF Dashboard Showcase" width="680" style="max-width: 100%; border-radius: 8px; border: 1px solid #334155; box-shadow: 0 4px 16px rgba(0,0,0,0.35);" />
  </a>
  <br>
  <sub><em>(Click image to view full resolution)</em></sub>
</p>

### 5. Toolbar Popup Console
Instant workstation status, engine latency diagnostics, and 1-click protection snooze controls:

<p align="center">
  <a href="docs/screenshots/popup_showcase.png" title="Click to view full resolution">
    <img src="docs/screenshots/popup_showcase.png" alt="SAIF Popup Console" width="340" style="max-width: 100%; border-radius: 8px; border: 1px solid #334155; box-shadow: 0 4px 16px rgba(0,0,0,0.35);" />
  </a>
  <br>
  <sub><em>(Click image to view full resolution)</em></sub>
</p>

---

## What is SAIF?

**SAIF (Semantic AI Firewall)** protects developers and engineering organizations from accidentally leaking proprietary source code, cloud credentials, database connection strings, and sensitive PII to external AI services.

Traditional network firewalls, web proxies, and cloud DLP gateways fail on modern AI web traffic because prompt payloads transit through end-to-end encrypted WebSockets and single-page applications.

SAIF executes **directly on your workstation**, validating prompt payloads in sub-millisecond time on-device via a native Go security daemon and local ONNX neural cascades:

<p align="center">
  <a href="docs/screenshots/architecture_infographic.png" title="Click to view full architecture diagram">
    <img src="docs/screenshots/architecture_infographic.png" alt="SAIF Architecture & Master Signal Flow" width="100%" style="max-width: 860px; border-radius: 10px; border: 1px solid #334155; box-shadow: 0 8px 24px rgba(0,0,0,0.4);" />
  </a>
  <br>
  <sub><em>(Click to view high-resolution architecture diagram)</em></sub>
</p>

---

## Universal Surface Protection: Every Field & The Omnibox

SAIF is not a narrow chat wrapper. It enforces security boundaries across **the entire browser surface**:

1. **Every Web Input Field**: Monitors textareas, form inputs, comment boxes, and rich editors across **any website**—including GitHub pull request reviews, Jira issue descriptions, Slack web, internal dashboards, and enterprise portals.
2. **The Browser Omnibox (Address Bar)**: Intercepts browser navigation before HTTP requests leave the socket. If an engineer inadvertently pastes a tokenized URL (`https://api.internal/v1?token=eyJ...`) or credential into Google/Bing search bars, SAIF immediately redirects to a local safe page.
3. **Clipboard Paste Traps**: High-entropy credentials pasted into editable elements are intercepted in-place with instant sanitization.

---

## Deep Combinatorial DLP Engine (Beyond Basic Regex)

Rather than relying on brittle, easily bypassed pattern lists, SAIF employs a multi-stage **mathematical and information-theoretic inspection engine**:

* **Universal Mathematical Checksums**: Eliminates false positives on random numerical strings by enforcing exact checksum algorithms:
  * **ISO/IEC 7812 Luhn Mod-10** for payment cards.
  * **ISO 7064 Mod 97-10** for international bank account numbers (IBAN).
  * **Modulo-23** and regional parity checks for European fiscal codes.
* **Shannon Entropy Thresholding ($H \ge 3.2$)**: Evaluates character randomness to catch unformatted cryptographic secrets, private keys, and high-entropy API tokens even when they lack recognizable prefixes.
* **Recursive De-Obfuscation Pipeline**: Normalizes candidate payloads before rules execute:
  * Multi-pass Base64 and Hex unpacking.
  * Leetspeak transliteration ($3 \to e$, $0 \to o$, $@ \to a$).
  * Unicode NFKC canonicalization and homoglyph mapping.
  * URL percent-encoding resolution and template literal unwrapping.

<p align="center">
  <a href="docs/screenshots/dlp_pipeline_infographic.png" title="Click to view full DLP pipeline">
    <img src="docs/screenshots/dlp_pipeline_infographic.png" alt="SAIF Combinatorial DLP Pipeline" width="100%" style="max-width: 840px; border-radius: 10px; border: 1px solid #334155; box-shadow: 0 8px 24px rgba(0,0,0,0.4);" />
  </a>
  <br>
  <sub><em>(Click to view high-resolution pipeline diagram)</em></sub>
</p>

---

## Three Production Models Available Out-of-the-Box

All three production models are included and hot-swappable directly in the Extension Console:

| Model | Memory (RAM) | Context Horizon | Latency ($P_{50}$) | Primary Workload |
| :--- | :--- | :--- | :--- | :--- |
| **`SAIF Light`** | ~35 MB | Context-Free | ~2.1 ms | Ultra-low overhead, credentials, terminal pastes, battery savings. |
| **`SAIF Neural`** | ~146 MB | 512 Tokens | ~13.8 ms | Real-time contextual analysis, web AI chats, Jira tickets, business prose. |
| **`SAIF Deep Neural`** | ~396 MB | 8,192 Tokens | ~21.5 ms | Full-file source code pasting, complex git diffs, whole-document IP. |

---

## Semantic Scope: Free Community vs. Pro & Enterprise

To provide a sustainable, enterprise-grade architecture, SAIF maintains a clear distinction between universal baseline security and organization-specific proprietary governance:

```mermaid
flowchart TB
    subgraph Free["Free Community Edition (100% On-Device)"]
        F1["Pre-Trained Universal Models<br/>(SAIF Light &bull; Neural &bull; Deep Neural)"]
        F2["General Threat Coverage<br/>(Common Cloud Keys, PII, Broad Secrets)"]
        F3["Local Workstation Logging<br/>(Local SQLite Spool)"]
    end

    subgraph ProEnt["Enterprise & Pro Upgrade Layer"]
        E1["1-Click Custom Semantic Centroid Training<br/><i>(Trains directly on Private Repos & Internal Docs)</i>"]
        E2["Proprietary Codenames & Trade Secrets<br/><i>(Project Titan, Internal Microservices, M&A)</i>"]
        E3["Centralized Cedar Policy-as-Code Studio"]
        E4["Fleet-Wide SIEM Streaming<br/><i>(Splunk &bull; Datadog &bull; Syslog)</i>"]
    end

    F1 -.->|Upgrade Path| E1
    E1 --> E2
    E2 --> E3 --> E4

    classDef freeBox fill:#10b98110,stroke:#10b981,stroke-width:1.5px;
    classDef entBox fill:#8b5cf610,stroke:#8b5cf6,stroke-width:1.5px;
    class Free freeBox;
    class ProEnt entBox;
```

### Free Community Edition (What You Get Today)
* **Universal General Semantic Protection**: Pre-trained on broad software engineering concepts, open-source architectures, credential leakage, PII classifications, and universal trade secret terminology.
* **100% On-Device Sovereignty**: Completely self-contained local ONNX runtime with zero cloud phoning.
* **Full Model Triad Unlocked**: Free access to `SAIF Light`, `SAIF Neural`, and `SAIF Deep Neural`.

### Why Teams Upgrade to Pro & Enterprise
* **1-Click Custom Semantic Centroid Training**: Pre-trained models cannot know your organization's internal, unannounced project names (e.g. *"Project Titan"*, confidential microservice architectures, proprietary financial algorithms). Pro and Enterprise allow 1-click training on private Git repositories and internal documentation, generating bespoke semantic centroids without cloud data leakage.
* **Fleet-Wide Cedar Policy Dispatch**: Manage centralized, cryptographically signed policy-as-code manifests distributed to thousands of endpoints.
* **Streaming Enterprise SIEM Connectors**: Real-time audit telemetry streaming to Splunk, Datadog, and Syslog for SOC threat correlation.

---

## Development Roadmap & The Future Horizon

We are aggressively expanding SAIF from browser interception to a full-stack developer security layer:

```mermaid
flowchart LR
    subgraph Phase1["Live Today: Workstation Surface"]
        W1["Browser Extension (Chrome / Edge / Brave)"]
        W2["Omnibox Search Interception"]
        W3["Go Workstation Daemon (:18080)"]
        W4["Antigravity IDE & agy CLI Native Hooks"]
    end

    subgraph Phase2["In Development: Developer Ecosystem"]
        D1["OpenAI Codex & VS Code LSP Bridge"]
        D2["Cursor & Windsurf Extension Hooks"]
        D3["Claude Code CLI Pre-Hook Wrapper"]
        D4["JetBrains (IntelliJ / PyCharm) Sensor"]
    end

    subgraph Phase3["Next-Gen: Multi-Modal & Agents"]
        M1["Clipboard Screenshot OCR DLP"]
        M2["Deep Document Sanitizer (PDF / DOCX)"]
        M3["Model Context Protocol (MCP) Tool Firewall"]
        M4["Local Model Gateway (Ollama :11434)"]
    end

    Phase1 ==> Phase2 ==> Phase3

    classDef live fill:#10b98115,stroke:#10b981,stroke-width:1px;
    classDef dev fill:#3b82f615,stroke:#3b82f6,stroke-width:1px;
    classDef future fill:#8b5cf615,stroke:#8b5cf6,stroke-width:1px;
    class Phase1 live;
    class Phase2 dev;
    class Phase3 future;
```

### 1. Supported Environments & Ecosystem Roadmap
* **Antigravity IDE & Antigravity CLI (`agy`) [Live Today]**: Native zero-egress in-process hook (`saif hook`) gating prompts and tools before network transmission.
* **OpenAI Codex & VS Code LSP Bridge [In Active Development]**: Dedicated Language Server Protocol proxy providing gutter alerts and pre-submission mediation.
* **Cursor & Windsurf [In Active Development]**: In-process extension hook bridge for modern agentic IDEs.
* **Claude Code (`@anthropic-ai/claude-code`) [In Active Development]**: Pre-execution shell wrapper enforcing DLP on prompts and tool executions.
* **JetBrains IDE Plugin [On Roadmap]**: Native plugin for IntelliJ IDEA, PyCharm, and WebStorm connecting to loopback daemon (`127.0.0.1:18080`).

### 2. Next-Generation Inspection Surfaces
* **Multi-Modal Clipboard OCR & Screenshot DLP**: Extracts and scans text from pasted screenshots, whiteboard photos, and architecture diagrams before vision models ingest them.
* **Deep Document Sanitization**: Pre-flight text extraction and inspection for PDF, DOCX, and XLSX attachments dropped into web AI chats.
* **Model Context Protocol (MCP) Tool Firewall**: Enforces Cedar authorization guardrails on autonomous agent tool invocations, protecting against indirect prompt injection attacks.
* **Local Model Gateway (Ollama / vLLM)**: Brings zero-trust governance and DLP auditing to local LLM endpoints running on `localhost:11434`.

---

## 1-Minute Quickstart (Windows)

1. **Download the Setup Executable**:  
   Download **[`SAIF-Setup.exe`](releases/SAIF-Setup.exe)** (19.6 MB) from the latest release.
2. **Run Installer**:  
   Double-click `SAIF-Setup.exe`. It runs 100% in user space without requiring administrator privileges.
3. **Load Extension**:  
   The installer automatically unpacks the extension to `%LOCALAPPDATA%\Programs\SAIF\extension`. Open `chrome://extensions`, enable **Developer mode**, click **Load unpacked**, and select the folder.

You are now fully protected across all web applications and search engines!

---

## Empirically Proven: Run the 4,835-Test Benchmark Yourself

We open-source our complete 4,835-vector benchmark suite under the MIT license so anyone can verify our claims on their own hardware:

```bash
# 1. Clone the repository
git clone https://github.com/saif-project/saif-free-tier.git
cd saif-free-tier

# 2. Run a fast 100-vector smoke test
python benchmark/run_benchmark.py --suite fast

# 3. Run the full 4,835-vector suite across all cores
python benchmark/run_benchmark.py --suite all --workers 16
```

### Benchmark Summary Across 4,835 Vectors

* **Cloud Credentials & API Keys (500 tests)**: **100.0%** Caught
* **Identity & Healthcare PII (485 tests)**: **100.0%** Caught
* **Sovereign ID Parity Checksums (300 tests)**: **100.0%** Caught
* **Adversarial Red-Team Attacks (550 tests)**: **100.0%** Governed (0 unmitigated misses)
* **Benign Developer Controls (3,000 tests)**: **0.00% to 1.46%** FPR

Read the complete technical whitepaper in **[`docs/BENCHMARK.md`](docs/BENCHMARK.md)**.

---

## Public Beta Notice & Community Bypass Reporting

This release is an active **Public Beta**. We are aggressively seeking feedback from developers and security researchers, especially on novel prompt injections or edge-case evasions.

> **Found a bypass or missed credential? Help us harden the engine.**  
> Submit sanitized prompt vectors to our [GitHub Issues](https://github.com/saif-project/saif-free-tier/issues) using the `[Miss / Bypass Report]` template. Every reproducible evasion drives new mathematical normalizers or centroid updates, and contributors are permanently credited in our public **Red-Team Hall of Fame**.

---

## Licensing

* **Benchmark Suite (`benchmark/`) & Documentation**: Licensed under the open-source **[MIT License](LICENSE.md)**.
* **Compiled Workstation Binaries**: Free for personal, academic, and developer evaluation under the **[SAIF Community EULA](EULA.md)**.
