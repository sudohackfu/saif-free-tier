# SAIF Architecture & System Design (Mermaid Diagrams)

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Architecture Diagrams**](ARCHITECTURE.md) • [**User Guide**](USER_GUIDE.md) • [**Benchmark Whitepaper**](BENCHMARK.md) • [**Rule Catalog**](RULE_CATALOG.md) • [**Overrides & Snoozing**](OVERRIDES_AND_SNOOZE.md) • [**FAQ**](FAQ.md) • [**Benchmark Suite**](../benchmark/README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

This document provides visual architectural specifications and data flow models for the **Semantic AI Firewall (SAIF)** using GitHub-native Mermaid diagrams styled to match the dark-mode cyber-defense interface.

<p align="center">
  <a href="screenshots/architecture_infographic.png" title="Click to view full resolution">
    <img src="screenshots/architecture_infographic.png" alt="SAIF Zero-Trust Architecture Infographic" width="860" style="max-width: 100%; border-radius: 12px; border: 1px solid #334155; box-shadow: 0 8px 30px rgba(0,0,0,0.6);" />
  </a>
  <br>
  <sub><em>(Click infographic to view full 4K high-resolution architecture blueprint)</em></sub>
</p>

---

## 1. System Topology & Workstation Boundary

SAIF operates entirely on the local developer workstation. Zero telemetry, raw prompt text, or audit logs ever leave the local machine.

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
flowchart TB
    subgraph Workstation ["💻 Developer Workstation Boundary"]
        subgraph Browser ["🌐 Modern Web Browser (Chrome / Edge / Brave)"]
            UI["Web AI Chat Interface<br/><b>Claude / ChatGPT / Gemini</b>"]:::blueNode
            CS["In-Page Content Script<br/><b>Ambient Watcher & Interceptor</b>"]:::cyanNode
            MODAL["In-Page Security Shield<br/><b>Real-Time Block & Redaction UI</b>"]:::redNode
            BG["Extension Service Worker<br/><b>Background Routing & Localhost Bridge</b>"]:::slateNode
            
            UI -->|"User Types Prompt"| CS
            CS -->|"Preflight Risk Badge"| UI
            CS -->|"Outbound Submit Intercepted"| MODAL
            CS <-->|"Internal IPC"| BG
        end

        subgraph LocalDaemon ["⚡ Local SAIF Daemon (saif.exe on 127.0.0.1:44321)"]
            PROXY["Local HTTP Server<br/><b>Loopback Endpoint</b>"]:::blueNode
            NORM["Syntactic Normalizer<br/><b>Base64, Hex, Homoglyphs & AST</b>"]:::amberNode
            DLP["Tier 1: Pre-compiled DLP Engine<br/><b>27+ High-Speed Regex Rules (<1ms)</b>"]:::cyanNode
            ENTROPY["Tier 2: Algorithmic Verification<br/><b>Shannon Entropy & Luhn Checksums (<3ms)</b>"]:::purpleNode
            ONNX["Tier 3: Local ONNX Transformer<br/><b>Neural Semantic Classifier (<10ms)</b>"]:::purpleNode
            DECIDE["Cedar Zero-Trust Engine<br/><b>Default-Deny Policy Arbitration</b>"]:::greenNode
            AUDIT[("Encrypted Audit Store<br/><b>Local Workstation SQLite</b>")]:::slateNode

            PROXY --> NORM
            NORM --> DLP
            DLP --> ENTROPY
            ENTROPY --> ONNX
            ONNX --> DECIDE
            DECIDE --> AUDIT
        end

        BG <-->|"Loopback REST / WebSocket"| PROXY
    end

    subgraph CloudAI ["☁️ External Generative AI Cloud"]
        OPENAI["OpenAI ChatGPT"]:::cloudNode
        ANTHROPIC["Anthropic Claude"]:::cloudNode
        GOOGLE["Google Gemini"]:::cloudNode
        DEEPSEEK["DeepSeek Chat"]:::cloudNode
    end

    UI -.->|"Allowed Clean Prompts Egress Directly"| CloudAI
    MODAL -.->|"Blocked Prompts Terminated Locally"| Workstation

    classDef default fill:#121217,stroke:#27272a,stroke-width:1.5px,color:#f4f4f5,rx:8px,ry:8px;
    classDef blueNode fill:#0f172a,stroke:#3b82f6,stroke-width:2px,color:#60a5fa,rx:8px,ry:8px;
    classDef cyanNode fill:#083344,stroke:#06b6d4,stroke-width:2px,color:#22d3ee,rx:8px,ry:8px;
    classDef greenNode fill:#022c22,stroke:#10b981,stroke-width:2px,color:#34d399,rx:8px,ry:8px;
    classDef redNode fill:#450a0a,stroke:#ef4444,stroke-width:2px,color:#f87171,rx:8px,ry:8px;
    classDef amberNode fill:#451a03,stroke:#f59e0b,stroke-width:2px,color:#fbbf24,rx:8px,ry:8px;
    classDef purpleNode fill:#2e1065,stroke:#8b5cf6,stroke-width:2px,color:#c084fc,rx:8px,ry:8px;
    classDef slateNode fill:#18181b,stroke:#3f3f46,stroke-width:1.5px,color:#d4d4d8,rx:8px,ry:8px;
    classDef cloudNode fill:#141416,stroke:#52525b,stroke-width:1px,color:#a1a1aa,rx:6px,ry:6px;

    style Workstation fill:#09090b,stroke:#2563eb,stroke-width:2px,color:#60a5fa;
    style Browser fill:#0f1117,stroke:#27272a,stroke-width:1.5px,color:#e4e4e7;
    style LocalDaemon fill:#0f1117,stroke:#8b5cf6,stroke-width:1.5px,color:#c084fc;
    style CloudAI fill:#09090b,stroke:#3f3f46,stroke-width:1.5px,color:#a1a1aa;
```

---

## 2. End-to-End Interception Sequence

The sequence below illustrates the life of an outbound prompt submission, from initial typing through local evaluation, blocking, and optional break-glass developer override.

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'darkMode': true,
    'background': '#09090b',
    'actorBkg': '#18181b',
    'actorBorder': '#3b82f6',
    'actorTextColor': '#60a5fa',
    'actorLineColor': '#38bdf8',
    'signalColor': '#38bdf8',
    'signalTextColor': '#e4e4e7',
    'noteBkgColor': '#18181b',
    'noteBorderColor': '#3b82f6',
    'noteTextColor': '#f4f4f5',
    'labelBoxBkgColor': '#18181b',
    'labelBoxBorderColor': '#27272a',
    'labelTextColor': '#f4f4f5',
    'loopTextColor': '#38bdf8',
    'altSectionBkgColor': '#121217',
    'fontFamily': 'Inter, system-ui, -apple-system, sans-serif',
    'fontSize': '13px'
  }
}}%%
sequenceDiagram
    autonumber
    actor Dev as 👨‍💻 Developer
    participant Page as 🌐 Web AI App DOM
    participant CS as 🛡️ Content Script (Sensor)
    participant Daemon as ⚡ saif.exe (Local Daemon)
    participant AI as ☁️ AI Provider (Claude/ChatGPT)

    Note over Dev,Page: Phase 1: Ambient Input Monitoring
    Dev->>Page: Types prompt text in composer
    Page->>CS: Real-time input event triggered
    CS->>CS: Preflight regex heuristic check
    opt Sensitive Pattern Detected
        CS->>Page: Render ambient risk card / badge
    end

    Note over Dev,Page: Phase 2: Outbound Submission Interception
    Dev->>Page: Hits Enter / clicks Submit button
    Page->>CS: Intercept keydown & submit events
    CS->>CS: Cancel event & freeze submission
    CS->>Daemon: POST /v1/evaluate (payload, domain)

    Note over Daemon: Phase 3: Zero-Trust Local Evaluation (<15ms)
    Daemon->>Daemon: De-obfuscation (Base64, Hex, AST unrolling)
    Daemon->>Daemon: Tier 1: Fast Regex Matching (<1ms)
    Daemon->>Daemon: Tier 2: Shannon Entropy & Math Checksums (<3ms)
    Daemon->>Daemon: Tier 3: Local ONNX Semantic Inference (<10ms)
    Daemon->>Daemon: Cedar Zero-Trust Policy Arbitration

    alt Verdict == ALLOW (Benign Prompt)
        Daemon-->>CS: { verdict: "ALLOW", latencyMs: 6.2 }
        CS->>Page: Release and dispatch submission
        Page->>AI: Outbound TLS transmission to AI provider
        AI-->>Dev: Streaming response displayed
    else Verdict == BLOCK (Secret / PII Leak Detected)
        Daemon-->>CS: { verdict: "BLOCK", rule: "SAIF-001", entity: "AWS_KEY" }
        CS->>Page: Render In-Page Security Shield Modal
        Page-->>Dev: Displays violation details & remediation options

        alt Action 1: Auto-Redact
            Dev->>Page: Clicks "Redact & Send"
            Page->>CS: Replace sensitive token with [REDACTED]
            CS->>Page: Update composer & submit sanitized text
            Page->>AI: Outbound transmission without sensitive tokens
        else Action 2: Break-Glass Override
            Dev->>Page: Clicks "Break-Glass Override" & enters reason
            CS->>Daemon: POST /v1/override (token, justification)
            Daemon-->>CS: { status: "AUTHORIZED", nonce: "uuid-v4" }
            CS->>Page: Permit outbound transmission
            Page->>AI: Transmission allowed with local audit record
        end
    end
```

---

## 3. Multi-Tier Hybrid Inspection Pipeline

SAIF combines deterministic regular expressions, mathematical checksums, and neural ONNX classifiers in a staged cascade to deliver 100% catch rate while maintaining under 15ms P50 latency.

<p align="center">
  <a href="screenshots/dlp_pipeline_infographic.png" title="Click to view full resolution">
    <img src="screenshots/dlp_pipeline_infographic.png" alt="SAIF Multi-Tier Inspection Pipeline Infographic" width="860" style="max-width: 100%; border-radius: 12px; border: 1px solid #334155; box-shadow: 0 8px 30px rgba(0,0,0,0.6);" />
  </a>
  <br>
  <sub><em>(Click infographic to view full 4K high-resolution inspection pipeline diagram)</em></sub>
</p>

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
flowchart TD
    START(["📥 Inbound Raw Prompt Payload"]):::blueNode --> NORM["Syntactic Normalization Engine"]:::amberNode

    subgraph Normalization ["Pre-Processing & De-Obfuscation"]
        NORM --> B64["Base64 / Hex Decoding"]:::amberNode
        NORM --> UNI["Unicode NFKC Canonicalization"]:::amberNode
        NORM --> LEET["Leetspeak Unfolding"]:::amberNode
        NORM --> URLD["Recursive URL Parameter Decode"]:::amberNode
        NORM --> AST["AST Literal & Code Unrolling"]:::amberNode
    end

    B64 & UNI & LEET & URLD & AST --> TIER1{"Tier 1: High-Speed DLP"}:::cyanNode

    subgraph FastPath ["Deterministic Regex & Trie Layer (<1ms)"]
        TIER1 -->|"Known Secret Pattern"| REGEX["27+ Pre-Compiled Patterns"]:::cyanNode
        TIER1 -->|"Keyword / Entity Token"| AHO["Aho-Corasick Token Index"]:::cyanNode
    end

    REGEX & AHO -->|"Definitive Pattern Match"| CEDAR
    REGEX & AHO -->|"No Definite Pattern Match"| TIER2{"Tier 2: Algorithmic Verification"}:::purpleNode

    subgraph MathLayer ["Mathematical & Entropy Checksums (<3ms)"]
        TIER2 --> SHANNON["Shannon Entropy Filter (H >= 3.2)"]:::purpleNode
        TIER2 --> LUHN["Luhn Mod-10 Credit Card Validator"]:::purpleNode
        TIER2 --> SOV["19-Country Sovereign ID Algorithms"]:::purpleNode
    end

    SHANNON & LUHN & SOV -->|"Algorithm Confirmed"| CEDAR
    SHANNON & LUHN & SOV -->|"Ambiguous / Context Dependent"| TIER3{"Tier 3: ONNX Neural Cascade"}:::purpleNode

    subgraph NeuralLayer ["Local ONNX Semantic Transformer (<10ms)"]
        TIER3 --> EMBED["Local Tokenizer & Embedding Engine"]:::purpleNode
        EMBED --> ONNX_ENG["SAIF Engineer ONNX Model"]:::purpleNode
        ONNX_ENG --> INTENT["Semantic Context & Intent Classifier"]:::purpleNode
    end

    INTENT --> CEDAR["Cedar Zero-Trust Arbitration Engine"]:::greenNode

    subgraph Decision ["Zero-Trust Policy Arbitration"]
        CEDAR --> VERDICT{"Policy Evaluation"}:::blueNode
        VERDICT -->|"Security Violation"| ACT_BLOCK["Verdict: BLOCK"]:::redNode
        VERDICT -->|"Verified Clean"| ACT_ALLOW["Verdict: ALLOW"]:::greenNode
        VERDICT -->|"Token Sanitizable"| ACT_REDACT["Verdict: REDACT"]:::amberNode
    end

    ACT_BLOCK --> OUT_BLOCK(["🛡️ In-Page Security Shield Modal"]):::redNode
    ACT_ALLOW --> OUT_ALLOW(["🚀 Outbound Transit to AI"]):::greenNode
    ACT_REDACT --> OUT_REDACT(["✨ Sanitized Payload Delivered"]):::amberNode

    classDef default fill:#121217,stroke:#27272a,stroke-width:1.5px,color:#f4f4f5,rx:8px,ry:8px;
    classDef blueNode fill:#0f172a,stroke:#3b82f6,stroke-width:2px,color:#60a5fa,rx:8px,ry:8px;
    classDef cyanNode fill:#083344,stroke:#06b6d4,stroke-width:2px,color:#22d3ee,rx:8px,ry:8px;
    classDef greenNode fill:#022c22,stroke:#10b981,stroke-width:2px,color:#34d399,rx:8px,ry:8px;
    classDef redNode fill:#450a0a,stroke:#ef4444,stroke-width:2px,color:#f87171,rx:8px,ry:8px;
    classDef amberNode fill:#451a03,stroke:#f59e0b,stroke-width:2px,color:#fbbf24,rx:8px,ry:8px;
    classDef purpleNode fill:#2e1065,stroke:#8b5cf6,stroke-width:2px,color:#c084fc,rx:8px,ry:8px;

    style Normalization fill:#0f1117,stroke:#27272a,stroke-width:1px,color:#fbbf24;
    style FastPath fill:#0f1117,stroke:#27272a,stroke-width:1px,color:#22d3ee;
    style MathLayer fill:#0f1117,stroke:#27272a,stroke-width:1px,color:#c084fc;
    style NeuralLayer fill:#0f1117,stroke:#27272a,stroke-width:1px,color:#c084fc;
    style Decision fill:#0f1117,stroke:#27272a,stroke-width:1px,color:#34d399;
```

---

## 4. Break-Glass Override & Snooze State Machine

The state diagram below models the operational states of the SAIF workstation sensor, illustrating transitions between active protection, blocking, override authorization, and developer snoozing.

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'darkMode': true,
    'background': '#09090b',
    'stateBkg': '#121217',
    'stateBorder': '#3b82f6',
    'stateTextColor': '#60a5fa',
    'innerStateBkg': '#09090b',
    'innerStateBorder': '#27272a',
    'innerStateTextColor': '#f4f4f5',
    'lineColor': '#38bdf8',
    'transitionColor': '#38bdf8',
    'transitionLabelColor': '#e4e4e7',
    'fontFamily': 'Inter, system-ui, -apple-system, sans-serif',
    'fontSize': '13px'
  }
}}%%
stateDiagram-v2
    [*] --> ActiveMonitoring : Daemon Online & Sensor Active

    state ActiveMonitoring {
        [*] --> IdleListening
        IdleListening --> PreflightEvaluating : Composer Input Detected
        PreflightEvaluating --> IdleListening : Text Cleared / Safe
        PreflightEvaluating --> RiskBadgeRendered : High Entropy Detected
        RiskBadgeRendered --> IdleListening : Text Removed
    }

    ActiveMonitoring --> Intercepted : User Triggers Submission
    
    state Intercepted {
        [*] --> LocalRPCDispatch
        LocalRPCDispatch --> CleanEgress : Verdict == ALLOW (<15ms)
        LocalRPCDispatch --> ShieldModalOpen : Verdict == BLOCK
    }

    CleanEgress --> ActiveMonitoring : Prompt Delivered to AI Provider

    state ShieldModalOpen {
        [*] --> UserDecision
        UserDecision --> Redacting : User Selects 'Redact & Send'
        UserDecision --> OverrideAuth : User Selects 'Break-Glass'
        UserDecision --> Dismissed : User Cancels Modal
    }

    Redacting --> CleanEgress : Sensitive String Replaced with Token
    Dismissed --> ActiveMonitoring : Text Preserved in Composer

    state OverrideAuth {
        [*] --> JustificationPrompt
        JustificationPrompt --> SingleUseNonceIssued : Reason Submitted
        SingleUseNonceIssued --> AuthorizedBypass : Nonce Validated by Daemon
    }

    AuthorizedBypass --> CleanEgress : Prompt Delivered & Logged Locally

    ActiveMonitoring --> Snoozed : User Selects Snooze (5m/15m/1h)
    
    state Snoozed {
        [*] --> TimerRunning
        TimerRunning --> SnoozeExpired : Timer Counts Down to 0:00
        TimerRunning --> ManualResume : User Clicks 'Resume Protection'
    }

    SnoozeExpired --> ActiveMonitoring : Protection Auto-Resumed
    ManualResume --> ActiveMonitoring : Protection Resumed Immediately

    ActiveMonitoring --> FailOpenFallback : Daemon Process Offline
    FailOpenFallback --> ActiveMonitoring : Heartbeat Restored (127.0.0.1:44321)
```

---

## 5. Adversarial Evasion Normalization Pipeline

Attackers and complex developer payloads frequently use nested encoding or AST wrappers to bypass naive string matching. SAIF unrolls these layers before semantic evaluation.

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
    INPUT["🚨 Raw Adversarial Prompt Payload"]:::redNode --> DETECT{"Payload Inspection"}:::blueNode

    DETECT -->|"Base64 String"| B64["Base64 Recursive Unpacker"]:::amberNode
    DETECT -->|"URL / Hex Encoded"| URLD["URL & Hex Decoder"]:::amberNode
    DETECT -->|"Unicode Homoglyphs"| UNI["NFKC Canonicalizer"]:::amberNode
    DETECT -->|"Leetspeak Chars"| LEET["Linguistic Unfolder"]:::amberNode
    DETECT -->|"Code AST Wrappers"| AST["Literal & Template Extractor"]:::amberNode

    B64 --> NESTED{"Nested Layers Remaining?"}:::cyanNode
    URLD --> NESTED
    UNI --> NESTED
    LEET --> NESTED
    AST --> NESTED

    NESTED -->|"Yes (Depth <= 3)"| DETECT
    NESTED -->|"No (Fully Canonical)"| CANON["✨ Normalized Canonical Stream"]:::greenNode

    CANON --> ENGINE["⚡ SAIF Local Triage Engine"]:::purpleNode

    classDef default fill:#121217,stroke:#27272a,stroke-width:1.5px,color:#f4f4f5,rx:8px,ry:8px;
    classDef blueNode fill:#0f172a,stroke:#3b82f6,stroke-width:2px,color:#60a5fa,rx:8px,ry:8px;
    classDef cyanNode fill:#083344,stroke:#06b6d4,stroke-width:2px,color:#22d3ee,rx:8px,ry:8px;
    classDef greenNode fill:#022c22,stroke:#10b981,stroke-width:2px,color:#34d399,rx:8px,ry:8px;
    classDef redNode fill:#450a0a,stroke:#ef4444,stroke-width:2px,color:#f87171,rx:8px,ry:8px;
    classDef amberNode fill:#451a03,stroke:#f59e0b,stroke-width:2px,color:#fbbf24,rx:8px,ry:8px;
    classDef purpleNode fill:#2e1065,stroke:#8b5cf6,stroke-width:2px,color:#c084fc,rx:8px,ry:8px;
```

---

## 6. Summary of Architectural Guarantees

| Invariant | Guarantee | Technical Implementation |
| :--- | :--- | :--- |
| **Zero Cloud Egress** | Prompts, regex scans, and neural embeddings never leave workstation memory | 100% local execution in `saif.exe` binding to `127.0.0.1:44321` |
| **Deterministic Fail-Open/Fail-Closed** | Configurable fail posture ensures developers are never unexpectedly blocked | Local heartbeat sensor with configurable posture toggle in dashboard |
| **Sub-15ms Latency** | Interception overhead is imperceptible during daily coding and AI chat | Multi-tier cascade: regex (<1ms) -> Shannon (<3ms) -> ONNX (<10ms) |
| **Audit Nonce Integrity** | Break-glass overrides cannot be forged or replayed | Single-use cryptographic UUID nonces generated by local daemon |
| **Adversarial Resilience** | Resistant to Base64, homoglyph, and template packing attacks | Recursive normalization pipeline unrolling up to 3 nested layers |
