# Frequently Asked Questions (FAQ)

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
This immediately terminates running processes, reverts Windows Internet Settings (`AutoConfigURL`), removes the autostart registry entry, clears environment variables, and removes all program files.
