# SAIF Overrides, Snoozing & Local Audit History

SAIF is designed for **developer velocity**. Security controls should never block critical development work when you encounter a false alarm or need to urgently paste test data.

---

## 1. Break-Glass Override Workflow

When SAIF intercepts an outbound prompt, an in-page shield modal appears over the AI chat:

```
┌────────────────────────────────────────────────────────────┐
│ 🛡️  SAIF Security Shield: Outbound Prompt Intercepted      │
├────────────────────────────────────────────────────────────┤
│ Detected: AWS Access Key ID (pat-aws-key)                  │
│ Confidence: High (Mathematical Entropy Verified)           │
│ Snippet: "...AKIAIOSFODNN7EXAMPLE..."                      │
│                                                            │
│ Options:                                                   │
│   [ Sanitize & Redact ]   Replace secret with [REDACTED]   │
│   [ Break-Glass Override ] Submit anyway with audit log    │
│   [ Dismiss & Edit ]      Return to prompt composer        │
└────────────────────────────────────────────────────────────┘
```

### How Override Works
1. Clicking **Break-Glass Override** prompts for an optional one-line developer reason (e.g., *"Testing with dummy sandbox credentials"*).
2. The prompt immediately unblocks and transits directly to the AI service.
3. The override event is recorded locally in your workstation audit log with a timestamp and the user reason.

---

## 2. Time-Bounded Snoozing

If you are conducting an extended debugging session with synthetic data, you can temporarily snooze active enforcement:

1. Click the **SAIF shield icon** in your browser toolbar to open the popup.
2. Select a pause duration:
   - **Pause for 5 Minutes**
   - **Pause for 15 Minutes**
   - **Pause for 1 Hour**
3. While snoozed:
   - The toolbar icon displays an amber indicator.
   - All prompts transit without interception.
   - When the timer expires, enforcement resumes automatically without user action required.
4. You can click **Resume Now** at any moment to re-engage active protection immediately.

---

## 3. Local Audit History & CSV Export

SAIF records blocked and overridden events strictly to your local workstation:
- **Database Location**: `%LOCALAPPDATA%\saif\spool.db` (local SQLite database).
- **Privacy Guarantee**: Zero telemetry is sent to external servers. Your prompts never leave your device.
- **Exporting History**:
  1. Open the SAIF Extension Dashboard (`dashboard.html`).
  2. Navigate to the **Recent Violations** tab.
  3. Filter by date range, rule category, or decision type.
  4. Click **Export CSV** or **Export JSON** to save local compliance records for your team or security reviews.
