# Changelog

All notable changes to the **Semantic AI Firewall (SAIF) Free Tier** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.5] - 2026-10-13

### Added
- **Unified Webview Triage Modal (`triage.html` / `triage.js`)**:
  - Replaces legacy imperative DOM modal injection with an isolated, high-DPI iframe webview matching the desktop triage experience.
  - Multi-threat composite extraction rendering distinct badges for deterministic regex DLP matches (`REGEX_DLP`) and on-device ONNX semantic policy violations (`SEMANTIC_ONNX`).
  - Interactive accordion match inspection with live multiline diff preview (`#diff-preview`) and masked token tags.
  - Batch action presets for instant Redact All (`R`) and Break-Glass (`B`), plus granular per-violation overrides.
- **Top-Level Omnibox Navigation Triage Parity (`blocked.html`)**:
  - Fully modernizes the standalone blocked warning page to adopt the exact same Composite Threat Triage modal UI as `triage.html`.
  - Enforces synchronous and asynchronous URL diversion on semantic address bar queries to prevent confidential prompt leaks into search engine query logs.
- **Standalone Desktop Triage & Override Lease**:
  - Out-of-browser triage session support via `POST /api/v1/triage/create` and standalone window launch.
  - Bounded in-memory `OverrideLease` tracking allowing seamless prompt resubmission without repeat blocking during triage turn.
  - Interval-containment algorithm (`applyMultiTokenRedaction`) merging overlapping spans and subsuming inner regex tokens inside semantic sentence boundaries.

### Fixed
- **ContentEditable & Rich-Textarea Line-Count Fidelity**:
  - Reconstructed exact paragraph layout and blank lines (`<p><br></p>`) in rich-text input frameworks (such as Google Gemini's `<rich-textarea>`), eliminating line collapse and character drift during prompt sanitization.
- **Verbatim Paste Preservation**:
  - Deprecated premature in-place paste mutation to preserve the full raw prompt for submission-time threat extraction and triage context.

---

## [1.0.4] - 2026-10-09

### Added
- **Native Antigravity IDE & Antigravity CLI (`agy`) Hook**:
  - Direct in-process lifecycle hook (`saif hook`) integrated via `~/.gemini/config/hooks.json` with zero proxy overhead or TLS certificate friction.
  - **Native 3-Button Win32 CBT Modal Dialog**: Intercepts outbound prompt violations with an interactive desktop modal offering **[Break Glass]** (audited emergency override), **[Redact]** (in-place transcript sanitization with safe execution continuation), and **[Block]** (immediate transcript purge and Zero-Egress Circuit Breaker termination).
  - **In-Place Stateless Transcript Sanitization**: Eliminates conversation history replay leaks by sanitizing sensitive tokens in `transcript_full.jsonl` immediately upon redaction or block, ensuring future conversation turns cannot inadvertently bundle confidential data.
  - **Contiguous Turn Deduplication**: Automatically detects multi-step autonomous agent loops (`invocationNum > 0`) to prevent repetitive prompt dialogs across tool execution cycles.
  - Sub-5ms `PreToolUse` mediation: gates sensitive tools (`view_file`, `write_to_file`, `run_command`) over loopback HTTP (`POST /api/v1/hook`).
- **Interactive Live Debug Wire Stream**:
  - New `--debug` CLI flag (`saif --debug`) streaming un-truncated raw JSON wire traffic for all incoming requests and outgoing decisions in real time.
  - Dedicated "View Live Traffic Stream (Terminal)..." tray menu item launching an external terminal for real-time visibility.
- **Windows System Tray Overhaul**:
  - Full management of resident daemon from the notification tray: master protection toggle, Antigravity IDE hook toggle, 4-tier snooze timers (5m, 15m, 1h, indefinite), Model Triad profile hot-swapping, Windows startup toggle, and update notifications.
- **Automated Workstation Auto-Healing**:
  - Upgrading to v1.0.4 automatically purges any stale proxy environment variables (`HTTP_PROXY`, `HTTPS_PROXY`, `NODE_EXTRA_CA_CERTS`, `SSL_CERT_FILE`) and deletes legacy self-signed root CAs from the Windows trust store.
- **Automatic User PATH Integration**:
  - Installer automatically registers `%LOCALAPPDATA%\Programs\SAIF` into the user's `PATH` environment variable.
- **Finished Screen Browser Onboarding**:
  - Installer offers an opt-out checkbox on the completion screen to open the Antigravity IDE Setup Guide in the default browser.

### Removed
- **Purge of Proxy Break-and-Inspect**:
  - Completely eradicated network-level TLS proxying, PAC proxy configuration (`AutoConfigURL`), and self-signed Root CA generation from installer and daemon runtime.
  - Eliminated the confusing "Protection Scope" installer wizard screen in favor of a clean, single-action frictionless EULA install.

### Fixed
- **Fixed Google Cloud Certificate Pinning Conflicts**:
  - Resolved fatal `SSL_PIN_FAILED` and connection reset errors in Antigravity IDE, Google Cloud Code, and gRPC runtimes caused by legacy proxy interception in v1.0.2/v1.0.3.

---

## [1.0.1] - 2026-09-28

### Added
- **Workstation CLI Control & Management Commands**:
  - `saif on` / `saif resume`: Instantly re-enable prompt inspection and DLP rules.
  - `saif off`: Switch local firewall to transparent bypass mode (zero latency, no socket teardown).
  - `saif pause [dur]`: Pause protection for preset durations (`5m`, `15m`, `1h`, or custom durations).
  - `saif status`: Display real-time daemon state, listening port, active model profile, and rules count.
  - `saif doctor`: Read-only 5-point workstation diagnostic check across certificates, trust store, environment variables, and proxy socket.
  - `saif repair [--dry-run]`: Self-healing auto-remediation that regenerates CAs, re-imports into Windows CurrentUser store, and synchronizes `NODE_EXTRA_CA_CERTS`.
- **Installer Scope Selection & Transparency**:
  - Single-action installer now offers **Full Developer Protection** (IDEs, CLIs, Browsers) or **Browser Only** (zero proxy/cert install).
  - Pre-installation technical disclosure dialog detailing on-device certificate generation and zero telemetry.
  - Interactive maintenance detection: re-running setup detects existing install and offers **Diagnostics & Repair**.
  - Zero-footprint clean uninstaller: removes root CA from Windows store, cleans environment variables, and prompts for audit log retention.
- **IDE & CLI AI Tool Interception Support**:
  - Automatic `NODE_EXTRA_CA_CERTS` provisioning for Antigravity IDE, Claude Code, GitHub Copilot, Cline, and Continue.dev.
  - Transparent HTTP `CONNECT` forward proxy tunneling for non-AI domains (`github.com`, `npmjs.org`).
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
