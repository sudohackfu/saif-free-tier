# SAIF Open Benchmark Suite (4,835 Golden Vectors)

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Architecture Diagrams**](../docs/ARCHITECTURE.md) • [**User Guide**](../docs/USER_GUIDE.md) • [**Benchmark Whitepaper**](../docs/BENCHMARK.md) • [**Rule Catalog**](../docs/RULE_CATALOG.md) • [**Overrides & Snoozing**](../docs/OVERRIDES_AND_SNOOZE.md) • [**FAQ**](../docs/FAQ.md) • [**Benchmark Suite**](README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

The **SAIF Open Benchmark Suite** is an open-source, empirical evaluation harness and dataset collection designed to measure the security efficacy, contextual disambiguation fidelity, and latency overhead of on-device Generative AI firewalls.

It contains **4,835 standardized test vectors** across five distinct test corpuses:
1. **Cloud Credentials & Developer Secrets** (`ground_truth_secrets.json`, 500 vectors): Real-world CredData API keys, AWS access tokens, GitHub PATs, JWTs, and database URIs.
2. **Identity & Healthcare PII** (`ground_truth_pii.json`, 485 vectors): Presidio-aligned personal identity numbers, US SSNs, credit cards (Luhn Mod-10), and HIPAA medical record disclosures.
3. **International Sovereign Identity Parity** (`presidio_parity.json`, 300 vectors): Algorithmic government identifiers across 19 countries (e.g., Italian Codice Fiscale, Spanish DNI/NIE, French NIR, UK NIN, Brazilian CPF).
4. **Benign Developer Controls** (`benign_controls.json`, 3,000 vectors): Clean SQL queries, code snippets, git diffs, mock UUIDs, and technical support requests to empirically measure False Positive Rate (FPR).
5. **Adversarial Red-Team Attacks** (`adversarial_stress_corpus.json`, 550 vectors): Advanced evasion attempts including Base64 encoding, Leetspeak, Unicode homoglyphs, JavaScript AST template unrolling, and linguistic negation traps.

---

## Quickstart

Ensure `saif.exe` is running locally (`http://127.0.0.1:18080`), then execute:

```bash
# 1. Run a 100-vector rapid smoke test (~2 seconds)
python benchmark/run_benchmark.py --suite fast

# 2. Run the complete 4,835-vector battery
python benchmark/run_benchmark.py --suite all
```

---

## Modular Suite Selection

You can target specific evasion disciplines and verticals using the `--suite` flag:

```bash
# Evaluate only Cloud Secrets & API Keys (500 tests)
python benchmark/run_benchmark.py --suite secrets

# Evaluate only Identity & Healthcare PII (485 tests)
python benchmark/run_benchmark.py --suite pii

# Evaluate only Sovereign Identity Checksums (300 tests)
python benchmark/run_benchmark.py --suite parity

# Evaluate False Positive Specificity on Clean Code (3,000 tests)
python benchmark/run_benchmark.py --suite benign

# Evaluate Adversarial Evasion & Negation Attacks (550 tests)
python benchmark/run_benchmark.py --suite redteam
```

---

## Evaluating Across Models

SAIF ships with three unlocked production models out-of-the-box. You can evaluate any model by specifying the `--model` argument:

```bash
# Benchmark SAIF Light (Deterministic Fast-Path Engine)
python benchmark/run_benchmark.py --model light --suite all

# Benchmark SAIF Neural (Balanced 512-Token Cascade)
python benchmark/run_benchmark.py --model neural --suite all

# Benchmark SAIF Deep Neural (Sovereign Flagship 8,192-Token Cascade)
python benchmark/run_benchmark.py --model deep-neural --suite all
```

---

## Concurrency & Performance Tuning

The benchmark runner executes concurrently across multiple worker threads to maximize workstation throughput:

```bash
# Run with 32 worker threads
python benchmark/run_benchmark.py --workers 32 --suite all
```

---

## Output Reports

Every run automatically writes:
- **`BENCHMARK_REPORT.md`**: Publication-grade GitHub markdown report with SLA verification and suite breakdowns.
- **`benchmark_results.json`**: Machine-readable metrics payload including P50, P90, P95, and P99 latencies.

---

## License

The **SAIF Benchmark Suite** code and datasets are licensed under the **MIT License**.
