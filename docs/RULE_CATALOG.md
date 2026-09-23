# SAIF Rule Catalog & Inline Bypass Directives

> **Documentation Index**:  
> [**Overview**](../README.md) • [**Architecture Diagrams**](ARCHITECTURE.md) • [**User Guide**](USER_GUIDE.md) • [**Benchmark Whitepaper**](BENCHMARK.md) • [**Rule Catalog**](RULE_CATALOG.md) • [**Overrides & Snoozing**](OVERRIDES_AND_SNOOZE.md) • [**FAQ**](FAQ.md) • [**Benchmark Suite**](../benchmark/README.md) • [**Empirical Report**](../BENCHMARK_REPORT.md) • [**Community EULA**](../EULA.md) • [**MIT License**](../LICENSE.md)

---

The **SAIF Free Community Edition** includes 27+ pre-configured, high-precision DLP rules and semantic classifiers running locally on your workstation.

---

## 1. Supported Detection Entities

### 1.1 Cloud Provider Credentials
* **AWS Access Keys**: Standard Access Key IDs (`AKIA...`) and Temporary STS Credentials (`ASIA...`).
* **AWS Secret Access Keys**: High-entropy 40-character base64 secret tokens with Shannon entropy filter ($H \ge 3.2$).
* **Google Cloud Service Accounts**: Private key JSON blocks (`"type": "service_account"`) and GCP API keys (`AIza...`).
* **Microsoft Azure Connection Strings**: Storage account keys and service bus shared access signatures.
* **Oracle Cloud Infrastructure (OCI)**: Private API signing keys and fingerprint tokens.

### 1.2 Developer Secrets & Tokens
* **GitHub Personal Access Tokens**: Classic tokens (`ghp_...`), Fine-Grained tokens (`github_pat_...`), OAuth tokens (`gho_...`), and refresh tokens (`ghr_...`).
* **GitLab Personal Access Tokens**: `glpat-...` tokens.
* **Package Registry Tokens**: NPM publication tokens (`npm_...`) and PyPI tokens (`pypi-...`).
* **Private Cryptographic Keys**: RSA, DSA, EC, and OpenSSH private key headers (`-----BEGIN OPENSSH PRIVATE KEY-----`, `-----BEGIN RSA PRIVATE KEY-----`).
* **Slack Tokens**: Bot and user tokens (`xoxb-...`, `xoxp-...`).

### 1.3 Database Connection Strings & Auth URIs
* **PostgreSQL / MySQL**: URIs containing embedded passwords (`postgresql://user:pass@host:5432/db`).
* **Redis / MongoDB**: Authenticated cache and document database connection strings (`redis://:authpassword@host:6379`).
* **Stripe Secret Keys**: Live API keys (`sk_live_...`) and restricted tokens (`rk_live_...`).
* **JSON Web Tokens (JWT)**: Three-part signed base64 authorization tokens (`eyJ...`).

### 1.4 Financial & Identity PII
* **US Social Security Numbers**: Format `XXX-XX-XXXX` with area and group number validation.
* **Major Credit Cards**: Visa, Mastercard, American Express, Discover with mandatory **Luhn Mod-10** algorithmic checksum verification.
* **International Bank Account Numbers (IBAN)**: Mandatory **ISO 7064 Mod 97-10** checksum validation.
* **European & Sovereign National IDs**:
  - UK National Insurance Numbers (NIN)
  - Italian Codice Fiscale (16-character alphanumeric parity check)
  - Spanish DNI / NIE (Modulo-23 verification digit)
  - French NIR (Social Security complement check)
  - Brazilian CPF (Dual verification digits)

### 1.5 Healthcare Identifiers (HIPAA)
* **Medical Record Numbers (MRN)**: Patient identifiers disclosed in clinical context.
* **CMS National Provider Identifiers (NPI)**: 10-digit medical provider IDs with Luhn verification.

### 1.6 Conceptual Engineering Trade Secrets
* **Project Phoenix Aerospace Engine**: Semantic NLU boundaries protecting confidential wing spar structural engineering, thrust vector flight dynamics, and cooling manifold specifications.

---

## 2. Inline Bypass Directives

Software engineers frequently need to paste sample code, dummy API keys, or documentation snippets into AI chats. SAIF supports **inline bypass directives** directly inside your prompt, code comments, or markdown blocks:

### 2.1 Targeted Rule Ignore
To bypass a specific detection rule on a single line, add `saif:ignore[<rule-id>]` in an inline comment:

```python
# saif:ignore[pat-aws-key]
aws_access_key = "AKIAEXAMPLEMOCKKEY123"
```

```javascript
// saif:ignore[github_token]
const token = "ghp_MockSampleTokenForDocs1234567890";
```

### 2.2 Sample / Test Data Allowance
To mark an entire block of mock or synthetic fixtures as benign:

```sql
-- saif:allow-sample
INSERT INTO mock_users (ssn, card) VALUES ('999-01-0001', '4000123456789010');
```

### 2.3 File-Level or Block-Level Bypass
To temporarily bypass all detection on an entire prompt submission (use responsibly):

```markdown
<!-- saif:ignore-all -->
Here is our complete sanitized architecture overview...
```

---

## 3. Toggling Rules in the Dashboard

Every rule can also be toggled **ON** or **OFF** globally with a single click:
1. Open the SAIF Extension Options Dashboard (`chrome-extension://<id>/dashboard.html`).
2. Navigate to the relevant tab (*Cloud Credentials*, *Developer Secrets*, *Database & Auth*, or *PII & Compliance*).
3. Flip any rule toggle. Changes synchronize instantly to the on-device daemon without requiring a service restart.
