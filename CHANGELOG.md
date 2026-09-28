# Changelog

All notable changes to the **Semantic AI Firewall (SAIF) Free Tier** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.1] - 2026-09-27

### Added
- **Frictionless Inno Setup Windows Installer**:
  - Native Per-Monitor DPI v2 Segoe UI installer packaged as `SAIF-Setup.exe` (19.69 MB).
  - Installs cleanly into user space (`%LOCALAPPDATA%\Programs\SAIF`) with **zero administrator privileges (UAC-free)**.
  - Streamlined single-action install flow with immediate EULA acceptance gating and automated launch of the background daemon and browser extension directory.
- **Model Triad Architecture**:
  - Hot-swappable local execution profiles accessible via the Options Dashboard and toolbar console:
    - **`SAIF Light` (`c4`)**: Zero-overhead deterministic DLP regex and CPAG algorithmic validation (<0.5ms) designed for developer terminal pastes, credentials, and low-spec environments.
    - **`SAIF Neural` (`light` / 512T)**: Local on-device ONNX embedding prototypes for everyday conversational text and web AI chat sessions.
    - **`SAIF Deep Neural` (`deep_neural` / 8,192T)**: Extended-context neural attention for full source code files, complex git diffs, and whole-document engineering intellectual property.
- **Egress Policy Engine (3 Operational Modes)**:
  - **`AI Only` (Default)**: Protects all user inputs across chat composers while inspecting the network wire exclusively on designated AI provider endpoints (ChatGPT, Claude, Gemini, DeepSeek, Ollama, Perplexity, etc.). Non-AI endpoints are bypassed with zero audit buffer saturation.
  - **`Audit Only` (Monitor / Simulation Mode)**: Evaluates all web traffic and user inputs, logging simulated blocks to workstation audit history with verdict `AUDIT` and action `log` without blocking the wire request or disrupting users with overlays.
  - **`Strict` (Enforce All)**: Unconditionally scans and blocks sensitive data egress across all websites, search engines, and background requests.
- **CPAG 2.0 Algorithmic Verification**:
  - Mod-10 Luhn checksums for credit cards, Mod-11 verification for Brazilian CPF, statutory § 139b AO verification for German Tax IDs, and Mod-23 verification for Spanish DNI.
- **Presidio Category Taxonomy**:
  - Standardized PII and credential categorization (`secrets`, `pii_financial`, `pii_national_id`, `pii_healthcare`, `neural_arbitration`, `sem_trade_secrets`).
- **Configurable Fail-Closed Offline Posture**:
  - User-selectable toggle (`failClosedOffline`) to fail-closed and block prompt transmission if the local daemon is offline.
- **Pre-packaged Release Assets**:
  - Added standalone `releases/SAIF-Setup.exe` and `releases/saif-extension.zip`.

### Changed
- Standardized local loopback agent daemon to port `18080` across all documentation, extension controllers, and installer scripts.
- Refined toolbar badge visual indicators: `⚡` (Cyan) for Light, `🧠` (Green) for Neural, `🧠` (Purple) for Deep Neural, and `OFF` (Amber) when snoozed or disconnected.
- Enhanced search engine and browser telemetry noise rejection in `interceptor.js`.

---

## [1.0.0] - 2026-09-01

### Added
- **Initial Public Beta Release**:
  - Manifest V3 browser extension sensor for Google Chrome, Brave, and Microsoft Edge.
  - Sub-millisecond in-browser fast-path evaluation and in-page prompt redaction.
  - Omnibox (address bar) search query interception protecting pasted credentials from public search engine query logs.
  - Interactive options console with local threat sandbox, custom rule creator, and prompt violation audit log.
  - Zero cloud dependencies with 100% on-device prompt evaluation.
