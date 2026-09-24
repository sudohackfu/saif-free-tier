# Privacy Policy for SAIF - Semantic AI Firewall

**Effective Date:** September 24, 2026  
**Last Updated:** September 24, 2026  
**Publisher:** SAIF Open Source Project  
**Repository:** [https://github.com/sudohackfu/saif-free-tier](https://github.com/sudohackfu/saif-free-tier)

---

### 1. Introduction & Core Principle
The **SAIF (Semantic AI Firewall)** extension was engineered from the ground up on the principle of **Zero-Trust On-Device Processing**. Our core mission is to protect sensitive enterprise credentials, API keys, Personally Identifiable Information (PII), and proprietary source code from accidental transmission into external Generative AI web applications.

We believe that a security tool must never compromise user privacy. **SAIF does not collect, store, sell, or transmit any user prompt text, browsing history, or personal data to remote cloud servers.**

---

### 2. Data Collection & Cloud Transmission
* **Zero Cloud Egress**: The extension does **NOT** transmit any prompt content, web page content, personal credentials, or browsing history to any external third-party server, analytics platform, or advertising network.
* **No Tracking or Telemetry**: SAIF contains no third-party tracking scripts, telemetry analytics (such as Google Analytics or Mixpanel), advertising SDKs, or remote error reporting beacons.
* **No Monetization of Data**: The SAIF Project will never sell, rent, monetize, or transfer user data to data brokers or third parties under any circumstances.

---

### 3. Local-First Processing Architecture
* **Local In-Browser Pattern Matching**: Prompt inspection, regular expression scanning, Shannon entropy checks, and token sanitization execute 100% locally within your browser's isolated JavaScript execution environment.
* **Local Loopback Communication (`127.0.0.1`)**: If the optional native workstation background daemon (`saif.exe`) is installed on your computer, the extension communicates strictly over local loopback (`http://127.0.0.1:18080`) to query local on-device neural models. This traffic never leaves your physical workstation.

---

### 4. Chrome Extension Storage Usage
The extension requests the `storage` permission solely to persist configuration on your local device:
* **Rule & Category Toggles**: Remembering which DLP rules you have enabled or disabled.
* **Domain Allowlists**: Storing web domains where you have chosen to temporarily snooze protection.
* **Local Audit History**: Storing a rolling ring-buffer of recent blocked events locally in `chrome.storage.local`. This data is accessible only by you within the extension dashboard and can be exported or purged at any time.

---

### 5. Permissions Rationale
* **`storage`**: Used exclusively to store user configuration, rule toggles, and local audit logs.
* **`activeTab` & `tabs`**: Used to identify the active AI service URL to enforce domain policies and display the security status popup.
* **`scripting`**: Used to inject in-page prompt sanitization and security block modals into web chat inputs.
* **`webNavigation`**: Used to detect sensitive query parameters in omnibox search submissions and prevent accidental leakage before page load.
* **`clipboardRead` / `clipboardWrite`**: Used exclusively when the user explicitly clicks the "Copy Redacted" or "Sanitize" button in the security overlay to paste clean text.
* **Host Permissions (`<all_urls>`)**: Required to monitor text inputs across any generative AI application or internal enterprise web portal where users draft prompts.

---

### 6. Compliance with Google User Data Policy
SAIF strictly complies with the [Chrome Web Store User Data Policy](https://developer.chrome.com/docs/webstore/user-data/):
1. **Single Purpose**: Dedicated exclusively to real-time client-side prompt security and data loss prevention.
2. **Limited Use**: We only request permissions necessary to execute real-time prompt interception.
3. **Data Security**: All sensitive prompt evaluations remain strictly on the user's local machine.

---

### 7. Contact & Security Inquiries
If you have questions, security disclosures, or concerns regarding this Privacy Policy, please open an issue on our GitHub repository:  
👉 [https://github.com/sudohackfu/saif-free-tier/issues](https://github.com/sudohackfu/saif-free-tier/issues)
