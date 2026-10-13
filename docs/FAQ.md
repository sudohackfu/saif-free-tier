# Frequently Asked Questions (FAQ)

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Changelog (v1.0.5)**](../CHANGELOG.md) • [**Architecture Diagrams**](ARCHITECTURE.md) • [**User Guide**](USER_GUIDE.md) • [**Benchmark Whitepaper**](BENCHMARK.md) • [**Rule Catalog**](RULE_CATALOG.md) • [**Overrides & Snoozing**](OVERRIDES_AND_SNOOZE.md) • [**FAQ**](FAQ.md) • [**Benchmark Suite**](../benchmark/README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

### Q: Why does SAIF not require installing a self-signed root CA or breaking TLS?
**A:** In v1.0.4+, SAIF completely eradicated network-level TLS proxying and root CA injection. Instead, SAIF uses direct **Policy Enforcement Points (PEPs)**:
1. **Browser Web Apps**: Intercepted in-page before transmission via Chromium Manifest V3 Main-World hooks (`window.fetch` / `XMLHttpRequest`).
2. **Desktop IDEs & CLIs**: Intercepted directly through native lifecycle hooks (Google Antigravity IDE `PreInvocation` and `PreToolUse` protocol via `saif hook`).
This eliminates the risk of broken Google Cloud certificate pinning (`SSL_PIN_FAILED`) and security warnings across developer tools.

### Q: How does the Antigravity IDE and `agy` CLI hook work?
**A:** When configured in `%USERPROFILE%\.gemini\config\hooks.json`, Antigravity pipes outbound prompts and tool commands directly to `saif.exe hook` via `stdin`. If clean, `saif.exe` exits in under 5ms and execution proceeds. If sensitive data is detected, `saif.exe` spawns the standalone desktop triage window to let you sanitize, override, or block the turn before any bytes leave your workstation.

### Q: What is the Standalone Desktop Triage Window and how does `OverrideLease` work?
**A:** For non-browser assistants (such as Antigravity IDE), SAIF spawns an isolated Microsoft Edge webview window (`msedge.exe --app=http://127.0.0.1:18080/triage?session=...`) showing detected violations, accordion drawers, and live diff previews. When you authorize an override or sanitization, SAIF issues an ephemeral in-memory `OverrideLease` token. When the assistant re-submits the approved turn, the daemon recognizes the lease and passes the prompt without prompting you twice.

### Q: How does SAIF handle multi-line paragraph formatting in Gemini and Claude?
**A:** Web AI composers like Google Gemini use custom `<rich-textarea>` custom elements where Chromium's `innerText` double-inflates block margins. SAIF v1.0.5 reconstructs exact paragraph DOM trees and blank lines (`<p><br></p>`) during in-place prompt sanitization, guaranteeing zero line collapse or character displacement.

### Q: What happens if `saif.exe` stops running or my laptop restarts?
**A:** By default, SAIF employs a **fail-open** posture to ensure software developers are never unexpectedly blocked during routine work. If the local daemon is offline or restarting, outbound AI prompts transit normally.
* For security-sensitive environments, you can toggle **Fail-Closed Offline Mode** in the Extension Dashboard (`System Diagnostics` tab). When enabled, prompts are blocked if the daemon cannot verify the payload.

### Q: Does SAIF send any prompt data to the cloud?
**A:** **Zero.** SAIF is 100% on-device. All regex matching, entropy calculation, mathematical checksums, and ONNX neural inference run entirely inside the local `saif.exe` process. No telemetry, prompt snippets, or audit logs ever leave your machine.

### Q: How much RAM does SAIF consume?
**A:** Depending on the active model profile:
* **`SAIF Light`**: ~35 MB idle RAM, ~118 MB peak active memory.
* **`SAIF Neural`**: ~146 MB idle RAM, ~246 MB peak active memory (512-token context).
* **`SAIF Deep Neural`**: ~396 MB idle RAM, ~485 MB peak active memory (8,192-token context).

### Q: Can I run SAIF on a different port?
**A:** Yes. Launch `saif.exe` with the `--port` flag:
```cmd
saif.exe --daemon --port 19090
```
Then update the target port in the Extension Options Dashboard.

### Q: How do I completely uninstall SAIF?
**A:** You can uninstall cleanly at any time through Windows Settings or from the command line:
1. **Windows Settings**: Go to **Settings > Apps > Installed apps**, locate **SAIF Free Community Edition**, and click **Uninstall**.
2. **Command Line**:
   ```cmd
   "%LOCALAPPDATA%\Programs\SAIF\uninstall.exe" --uninstall
   ```
This terminates running background processes, removes user PATH registrations, deletes the autostart registry entry, and removes all program files cleanly.
