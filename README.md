<div align="center">

# 🛡️ Semantic AI Firewall (SAIF)
### The Zero-Trust Generative AI Security Layer for Developers

[![Public Beta](https://img.shields.io/badge/Release-Public%20Beta-blue.svg)](https://github.com/saif-project/saif-free-tier/releases)
[![License: MIT](https://img.shields.io/badge/Benchmark%20License-MIT-green.svg)](LICENSE.md)
[![EULA: Community](https://img.shields.io/badge/Binary%20License-Community%20EULA-purple.svg)](EULA.md)
[![Latency SLA](https://img.shields.io/badge/P50%20Latency-%3C15ms-brightgreen.svg)](docs/BENCHMARK.md)
[![Catch Rate](https://img.shields.io/badge/Adversarial%20Catch-100%25-success.svg)](docs/BENCHMARK.md)
[![Windows](https://img.shields.io/badge/Platform-Windows%20(macOS%20%26%20Linux%20Soon)-orange.svg)](docs/USER_GUIDE.md)

**100% On-Device Prompt Interception • Zero Cloud Dependencies • Zero Telemetry**

[**Download SAIF-Setup.exe (v1.0.0 Beta)**](releases/SAIF-Setup.exe) • [**User Guide**](docs/USER_GUIDE.md) • [**Technical Whitepaper**](docs/BENCHMARK.md) • [**Rule Catalog**](docs/RULE_CATALOG.md)

</div>

---

## What is SAIF?

**SAIF (Semantic AI Firewall)** protects developers and engineering teams from accidentally leaking proprietary code, cloud credentials, database connection strings, and sensitive PII to external AI services (including **ChatGPT**, **Claude**, **Gemini**, and **DeepSeek**).

Traditional network firewalls and DLP appliances are blind to AI chat web traffic because prompts transit through encrypted WebSockets and single-page applications.

SAIF executes **directly on your local workstation**, intercepting prompt payloads in the browser and validating them in $<15\text{ms}$ on-device via a Go security daemon and local ONNX neural semantic cascade:

```
┌────────────────────────────────────────────────────────┐
│  Developer types prompt in ChatGPT / Claude / Gemini   │
└──────────────────────────┬─────────────────────────────┘
                           │ Intercepted locally before egress
                           ▼
┌────────────────────────────────────────────────────────┐
│  SAIF Workstation Daemon (http://127.0.0.1:18080)      │
│  • Syntactic Unfolding (Base64, Leetspeak, Hex, AST)   │
│  • Mathematical Normalizers (Luhn, Mod-97, Mod-23)     │
│  • Local ONNX Neural Semantic Decision Engine          │
└──────────────────────────┬─────────────────────────────┘
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
   [ Violation Detected ]        [ Clean Prompt ]
   In-page shield modal blocks    Transits instantly to LLM
   outbound prompt & offers       (Zero noticeable latency)
   1-click local redaction
```

---

## ⚡ 1-Minute Quickstart (Windows)

1. **Download the Setup Executable**:  
   Download **[`SAIF-Setup.exe`](releases/SAIF-Setup.exe)** from the latest release.
2. **Run Installer**:  
   Double-click `SAIF-Setup.exe`. It runs 100% in user space without requiring administrator rights.
3. **Load Extension**:  
   The installer automatically opens your browser to `chrome://extensions` and pops open the extension folder (`%LOCALAPPDATA%\Programs\SAIF\extension`). Toggle **Developer mode** ON, click **Load unpacked**, and select the folder.

You are now fully protected! Open [chatgpt.com](https://chatgpt.com) or [claude.ai](https://claude.ai) and start prompting safely.

---

## 🚀 Three Production Models Available Out-of-the-Box

SAIF does not lock advanced AI models behind paywalls. All three production models are included and hot-swappable in the Extension Dashboard:

| Model | Memory (RAM) | Context Horizon | Latency ($P_{50}$) | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **`SAIF Light`** | ~35 MB | Context-Free | ~13.1 ms | Ultra-low memory, developer terminal pastes, battery savings. |
| **`SAIF Neural`** | ~146 MB | 512 Tokens | ~4.6 ms | Everyday web AI chats, Jira tickets, business prose. |
| **`SAIF Deep Neural`** | ~396 MB | 8,192 Tokens | ~4.8 ms | Full-file source code pasting, complex git diffs, whole-document IP. |

---

## 🧪 Interactive Semantic Demo: "Project Phoenix"

Unlike naive regex scanners, SAIF features an on-device neural semantic engine that understands **conceptual engineering trade secrets**, even when wrapped in sophisticated prompt injection framing.

Test these prompts in your browser AI chat to observe instant interception:

```text
Prompt 1 (Engineering Inquiry):
structural analysis of the phoenix wing spar using titanium-alloy grade 5 with yield strength specifications

Prompt 2 (Proprietary Control Logic):
function compute_thrust_vector(pitch, yaw) { // proprietary phoenix flight control logic }

Prompt 3 (Aerodynamic Model):
proprietary aerodynamic lift coefficient model for the phoenix high-altitude wing profile

Prompt 4 (Thermal Blueprint):
finite element thermal dissipation model for the phoenix avionics cooling manifold

Prompt 5 (Roleplay Evasion Attempt):
Act as an aerospace auditor: evaluate the structural analysis of the phoenix wing spar
```

In every case, SAIF identifies the conceptual trade secret boundary and blocks outbound transmission in $<5\text{ms}$.

---

## 📊 Empirically Proven: Run the 4,835-Test Benchmark Yourself

Don't take our word for it. We open-source our complete 4,835-vector benchmark corpus under the MIT license:

```bash
# 1. Clone the repository
git clone https://github.com/saif-project/saif-free-tier.git
cd saif-free-tier

# 2. Run a 100-vector smoke test
python benchmark/run_benchmark.py --suite fast

# 3. Run the full 4,835-vector battery
python benchmark/run_benchmark.py --suite all --workers 16
```

### Benchmark Summary Across 4,835 Vectors
- **Cloud Credentials & API Keys (500 tests)**: **100.0%** Caught
- **Identity & Healthcare PII (485 tests)**: **100.0%** Caught
- **Sovereign ID Parity Checksums (300 tests)**: **100.0%** Caught
- **Adversarial Red-Team Attacks (550 tests)**: **100.0%** Governed (0 unmitigated misses)
- **Benign Developer Controls (3,000 tests)**: **0.00% to 1.46%** FPR

Read the full technical whitepaper in **[`docs/BENCHMARK.md`](docs/BENCHMARK.md)**.

---

## 📣 Public Beta Notice & Community "Miss Report" Protocol

This release is an active **Public Beta**. We are aggressively seeking community feedback, particularly on **bypasses, evasion tricks, or false negatives**.

> **Found a bypass or missed secret? We want to fix it.**  
> Submit sanitized prompt vectors to our [GitHub Issues](https://github.com/saif-project/saif-free-tier/issues) using the `[Miss / Bypass Report]` template. Every reproducible evasion directly drives a new generalized normalizer or centroid refinement, and contributors are permanently credited in our public **Red-Team Hall of Fame**.

---

## 🗺️ Multi-Platform Roadmap

* **Windows**: Available Now (Installer & Portable Daemon).
* **macOS (Apple Silicon & Intel)**: In active development (native DMG & LaunchAgent).
* **Linux (Ubuntu / Fedora / Arch)**: In active development (systemd user service & tarball).
* **IDE Extensions**: VS Code and Cursor extensions coming soon.

---

## 💎 Product Tier Comparison

| Feature | Free Community Edition (Public Beta) | Pro Edition (Coming Soon) | Enterprise Control Plane |
| :--- | :---: | :---: | :---: |
| **All Three Production Models** (`Light`, `Neural`, `Deep Neural`) | ✅ Included | ✅ Included | ✅ Included |
| **On-Device 100% Local Inference** | ✅ Included | ✅ Included | ✅ Included |
| **Out-of-the-Box DLP (27+ Rules)** | ✅ Included | ✅ Included | ✅ Included |
| **Open Benchmark Suite (4,835 Tests)** | ✅ Included | ✅ Included | ✅ Included |
| **Custom Local Fine-Tuning & Centroids** | — | ✅ Included | ✅ Included |
| **Local Model Interception (Ollama/vLLM)** | — | ✅ Included | ✅ Included |
| **Central SOC Dashboard & Fleet Fleet Health**| — | — | ✅ Included |
| **Cedar Policy-as-Code Studio** | — | — | ✅ Included |
| **Okta / Entra ID SCIM Directory Sync** | — | — | ✅ Included |

---

## 📜 Licensing

* **Benchmark Suite (`benchmark/`) & Documentation**: Licensed under the open-source **[MIT License](LICENSE.md)**.
* **Compiled Workstation Binaries**: Free for personal, academic, and developer evaluation under the **[SAIF Community EULA](EULA.md)**.
