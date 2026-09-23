# SAIF Architecture & System Design (Mermaid Diagrams)

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Architecture Diagrams**](ARCHITECTURE.md) • [**User Guide**](USER_GUIDE.md) • [**Benchmark Whitepaper**](BENCHMARK.md) • [**Rule Catalog**](RULE_CATALOG.md) • [**Overrides & Snoozing**](OVERRIDES_AND_SNOOZE.md) • [**FAQ**](FAQ.md) • [**Benchmark Suite**](../benchmark/README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

This document provides visual architectural specifications and data flow models for the **Semantic AI Firewall (SAIF)** using GitHub-native Mermaid diagrams. All diagrams are fully version-controlled, responsive, and rendered natively by GitHub in both light and dark mode.

---

## 1. System Topology & Workstation Boundary

SAIF operates entirely on the local developer workstation. Zero telemetry, raw prompt text, or audit logs ever leave the local machine.

```mermaid
flowchart TB
    subgraph Workstation ["Developer Workstation Boundary"]
        subgraph Browser ["Modern Web Browser - Chrome / Edge / Brave"]
            UI["Developer Web App UI - Claude / ChatGPT / Gemini"]
            CS["In-Page Content Script - Interceptor & Composer Watcher"]
            MODAL["In-Page Security Shield Modal"]
            BG["Extension Service Worker - Background Router"]
            
            UI -->|"User Types Prompt"| CS
            CS -->|"Real-time Preflight Risk"| UI
            CS -->|"Submit Intercepted"| MODAL
            CS <-->|"Internal Message Passing"| BG
        end

        subgraph LocalDaemon ["Local SAIF Process - saif.exe"]
            PROXY["Local HTTP Server - 127.0.0.1:44321"]
            NORM["Adversarial De-Obfuscator & Normalizer"]
            DLP["Tier 1: Pre-compiled DLP Engine"]
            ENTROPY["Tier 2: Shannon Entropy & Luhn Validators"]
            ONNX["Tier 3: ONNX Semantic Neural Classifier"]
            DECIDE["Cedar Zero-Trust Decision Engine"]
            AUDIT[("Local Encrypted Audit Store")]

            PROXY --> NORM
            NORM --> DLP
            DLP --> ENTROPY
            ENTROPY --> ONNX
            ONNX --> DECIDE
            DECIDE --> AUDIT
        end

        BG <-->|"Localhost REST API / WebSockets"| PROXY
    end

    subgraph CloudAI ["External Cloud Generative AI Providers"]
        OPENAI["OpenAI ChatGPT"]
        ANTHROPIC["Anthropic Claude"]
        GOOGLE["Google Gemini"]
        DEEPSEEK["DeepSeek Chat"]
    end

    UI -.->|"Allowed Prompts Egress Directly"| CloudAI
    MODAL -.->|"Blocked Prompts Terminated Locally"| Workstation

    classDef workstation fill:#0f172a,stroke:#3b82f6,stroke-width:2px,color:#fff;
    classDef cloud fill:#1e1e2e,stroke:#64748b,stroke-width:1px,color:#94a3b8;
    class Workstation workstation;
    class CloudAI cloud;
```

---

## 2. End-to-End Interception Sequence

The sequence below illustrates the lifecycle of an outbound prompt submission, from initial typing through local evaluation, blocking, and optional break-glass developer override.

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Developer
    participant Page as AI Web App DOM
    participant CS as Content Script (Sensors)
    participant Daemon as saif.exe (Local Engine)
    participant AI as Cloud AI Provider (Claude/ChatGPT)

    Note over Dev,Page: 1. Ambient Composer Monitoring
    Dev->>Page: Types prompt text in composer textarea
    Page->>CS: Input event triggered
    CS->>CS: Fast local regex preflight check
    opt Potential Secret Detected
        CS->>Page: Render subtle ambient risk badge
    end

    Note over Dev,Page: 2. Outbound Submission Interception
    Dev->>Page: Hits Enter / clicks Submit button
    Page->>CS: submit / keydown event intercepted
    CS->>CS: event.preventDefault() and event.stopImmediatePropagation()
    CS->>Daemon: POST /v1/evaluate (payload, domain, modelProfile)

    Note over Daemon: 3. Parallel Local Pipeline (<15ms)
    Daemon->>Daemon: Normalization (Base64/Hex/Unicode)
    Daemon->>Daemon: Tier 1: Fast Regex Matching (<1ms)
    Daemon->>Daemon: Tier 2: Shannon Entropy & Math Checksums (<3ms)
    Daemon->>Daemon: Tier 3: Local ONNX Semantic Inference (<10ms)
    Daemon->>Daemon: Cedar Zero-Trust Policy Arbitration

    alt Verdict = ALLOW (Clean Prompt)
        Daemon-->>CS: { verdict: "ALLOW", latencyMs: 6.2 }
        CS->>Page: Dispatch original submission event
        Page->>AI: Outbound TLS transmission to AI provider
        AI-->>Dev: Streaming AI response
    else Verdict = BLOCK (Sensitive Leak Detected)
        Daemon-->>CS: { verdict: "BLOCK", rule: "SAIF-001", entity: "AWS_SECRET_KEY" }
        CS->>Page: Render In-Page Security Shield Modal
        Page-->>Dev: Modal displays detected secret & remediation options
        
        alt Remediation Option A: Auto-Redaction
            Dev->>Page: Clicks "Redact & Send"
            Page->>CS: Auto-sanitize sensitive token with [REDACTED]
            CS->>Page: Update textarea & submit sanitized prompt
            Page->>AI: Outbound transmission without sensitive credentials
        else Remediation Option B: Break-Glass Override
            Dev->>Page: Clicks "Break-Glass Override" & enters reason
            CS->>Daemon: POST /v1/override (token, justification, nonce)
            Daemon-->>CS: { status: "AUTHORIZED", nonce: "uuid-v4" }
            CS->>Page: Release submission with authorized audit flag
            Page->>AI: Outbound transmission permitted
        end
    end
```

---

## 3. Multi-Tier Hybrid Inspection Pipeline

SAIF combines deterministic regular expressions, mathematical checksums, and neural ONNX classifiers in a staged cascade to deliver 100% catch rate while maintaining under 15ms P50 latency.

```mermaid
flowchart TD
    START(["Inbound Raw Prompt"]) --> NORM["Adversarial Normalization Engine"]

    subgraph Normalization ["Pre-Processing & De-Obfuscation"]
        NORM --> B64["Base64 / Hex Decoding"]
        NORM --> UNI["Unicode NFKC Canonicalization"]
        NORM --> LEET["Leetspeak Normalization"]
        NORM --> URLD["Recursive URL Parameter Unquoting"]
        NORM --> AST["Code & String Literal Extraction"]
    end

    B64 & UNI & LEET & URLD & AST --> TIER1{"Tier 1: High-Speed DLP"}

    subgraph FastPath ["Deterministic Inspection Layer < 1ms"]
        TIER1 -->|"Known Secret Prefix"| REGEX["27+ Pre-compiled Regex Patterns"]
        TIER1 -->|"Known Keyword / Pattern"| AHO["Aho-Corasick Token Index"]
    end

    REGEX & AHO -->|"Definitive Match"| CEDAR
    REGEX & AHO -->|"No Strict Match"| TIER2{"Tier 2: Algorithmic Verification"}

    subgraph MathLayer ["Mathematical & Entropy Checksums < 3ms"]
        TIER2 --> SHANNON["Shannon Entropy Filter H >= 3.2"]
        TIER2 --> LUHN["Luhn Mod-10 Credit Card Validator"]
        TIER2 --> SOV["Sovereign ID Algorithmic Checksums"]
    end

    SHANNON & LUHN & SOV -->|"Algorithm Confirmed"| CEDAR
    SHANNON & LUHN & SOV -->|"Uncertain / High Entropy"| TIER3{"Tier 3: ONNX Semantic AI"}

    subgraph NeuralLayer ["Local ONNX Inference < 10ms"]
        TIER3 --> EMBED["Local Tokenizer & Embedding"]
        EMBED --> ONNX_ENG["SAIF Engineer ONNX Transformer"]
        ONNX_ENG --> INTENT["Contextual Intent & Injection Classifier"]
    end

    INTENT --> CEDAR["Cedar Zero-Trust Arbitration Engine"]

    subgraph Decision ["Policy Enforcement & Action"]
        CEDAR --> VERDICT{"Policy Evaluation"}
        VERDICT -->|"Violation"| ACT_BLOCK["Verdict: BLOCK"]
        VERDICT -->|"Compliant"| ACT_ALLOW["Verdict: ALLOW"]
        VERDICT -->|"Sanitizable"| ACT_REDACT["Verdict: REDACT"]
    end

    ACT_BLOCK --> OUT(["Security Shield Rendered"])
    ACT_ALLOW --> OUT_ALLOW(["Prompt Dispatched to AI"])
    ACT_REDACT --> OUT_REDACT(["Redacted Payload Dispatched"])

    classDef pass fill:#065f46,stroke:#10b981,stroke-width:1px,color:#fff;
    classDef block fill:#7f1d1d,stroke:#ef4444,stroke-width:1px,color:#fff;
    class ACT_ALLOW pass;
    class ACT_BLOCK block;
```

---

## 4. Break-Glass Override & Snooze State Machine

The state diagram below models the operational states of the SAIF workstation sensor, illustrating transitions between active protection, blocking, override authorization, and developer snoozing.

```mermaid
stateDiagram-v2
    [*] --> ActiveMonitoring : Extension Loaded & Daemon Online

    state ActiveMonitoring {
        [*] --> IdleListening
        IdleListening --> PreflightEvaluating : Text Input Detected
        PreflightEvaluating --> IdleListening : Input Dismissed / Safe
        PreflightEvaluating --> RiskWarningActive : High Entropy Detected
        RiskWarningActive --> IdleListening : Text Cleared
    }

    ActiveMonitoring --> Intercepted : User Submits Outbound Prompt
    
    state Intercepted {
        [*] --> EvaluatingLocalRPC
        EvaluatingLocalRPC --> CleanEgress : Verdict == ALLOW
        EvaluatingLocalRPC --> BlockedModalOpen : Verdict == BLOCK
    }

    CleanEgress --> ActiveMonitoring : Prompt Dispatched to AI

    state BlockedModalOpen {
        [*] --> UserReviewingOptions
        UserReviewingOptions --> Redacting : User Clicks 'Redact & Send'
        UserReviewingOptions --> Overriding : User Clicks 'Break-Glass'
        UserReviewingOptions --> Cancelled : User Closes Modal / Discards
    }

    Redacting --> CleanEgress : Sensitive String Replaced with Token
    Cancelled --> ActiveMonitoring : Text Retained in Composer

    state Overriding {
        [*] --> JustificationInput
        JustificationInput --> NonceIssued : Reason Submitted
        NonceIssued --> AuthorizedBypass : Single-Use Nonce Validated
    }

    AuthorizedBypass --> CleanEgress : Prompt Dispatched with Audit Entry

    ActiveMonitoring --> Snoozed : Developer Enables Snooze (5m/15m/1h)
    
    state Snoozed {
        [*] --> CountdownTimerRunning
        CountdownTimerRunning --> SnoozeExpired : Timer Reaches 0:00
        CountdownTimerRunning --> ManualResume : Developer Clicks 'Resume Protection'
    }

    SnoozeExpired --> ActiveMonitoring : Protection Resumed Automatically
    ManualResume --> ActiveMonitoring : Protection Resumed Immediately

    ActiveMonitoring --> FailOpenFallback : Daemon Offline / Crash
    FailOpenFallback --> ActiveMonitoring : Daemon Reconnected (Heartbeat OK)
```

---

## 5. Adversarial Evasion Normalization Pipeline

Attackers and complex developer payloads frequently use nested encoding or AST wrappers to bypass naive string matching. SAIF unrolls these layers before semantic evaluation.

```mermaid
flowchart LR
    INPUT["Raw Evasive Prompt"] --> DETECT{"Payload Encoding Type"}

    DETECT -->|"Base64 String"| B64_DEC["Base64 Recursive Unpacker"]
    DETECT -->|"Hex / URL Encoded"| URL_DEC["URL & Hex Decode"]
    DETECT -->|"Unicode Homoglyphs"| UNI_DEC["NFKC Normalizer"]
    DETECT -->|"Leetspeak Chars"| LEET_DEC["Linguistic Substitution Unfolder"]
    DETECT -->|"Code AST Wrapper"| AST_DEC["JS / Python / JSON Literal Extractor"]

    B64_DEC --> CHECK_NESTED{"Nested Encoding Remaining?"}
    URL_DEC --> CHECK_NESTED
    UNI_DEC --> CHECK_NESTED
    LEET_DEC --> CHECK_NESTED
    AST_DEC --> CHECK_NESTED

    CHECK_NESTED -->|"Yes (Depth <= 3)"| DETECT
    CHECK_NESTED -->|"No"| CANON["Canonical Normalized Text Stream"]

    CANON --> ENGINE["SAIF Local Classification Engine"]

    classDef stage fill:#1e293b,stroke:#38bdf8,stroke-width:1px,color:#fff;
    class B64_DEC,URL_DEC,UNI_DEC,LEET_DEC,AST_DEC,CANON stage;
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
