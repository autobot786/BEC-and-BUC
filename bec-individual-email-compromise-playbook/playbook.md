# BEC Playbook: Individual Email Compromise (IEC)

**Use case:** A single user mailbox is compromised (password theft, MFA bypass, OAuth consent, session hijack), and the attacker uses that mailbox to **impersonate the user**, **request payments**, **harvest data**, or **pivot** to additional accounts.

This playbook is written to be **vendor-neutral**, with specific guidance and query examples for:
- **Microsoft 365 (Exchange Online / Defender / Entra ID)**
- **Google Workspace (Gmail / Admin / Security Investigation Tool)**

---

## 1) Objectives

1. **Stop ongoing attacker activity** (containment).
2. **Preserve evidence** for investigation and potential legal/insurance needs.
3. **Assess impact** (data access, internal/external fraud attempts, lateral movement).
4. **Restore secure access** for the user.
5. **Prevent recurrence** through hardening and user coaching.

---

## 2) Scope and Triggers

### In scope
- Single mailbox compromise (employee/contractor).
- Email-based fraud attempts from compromised account.
- Malicious inbox rules, forwarding, delegation, OAuth apps, and session tokens.

### Common triggers / alerts
- User reports “sent emails I didn’t send” or “MFA prompts I didn’t approve”.
- SOC alert: risky sign-in, impossible travel, unfamiliar device, legacy auth.
- Finance reports unusual payment request or supplier bank change email.
- External recipient flags suspicious request from your user.

---

## 3) Severity and Decision Criteria

### Severity levels (suggested)
- **SEV1 (Critical):** confirmed fraudulent payment attempt; evidence of data exfiltration of regulated info; attacker active right now; privileged mailbox or executives.
- **SEV2 (High):** confirmed compromise; attacker sent phishing to internal/external; unknown data access.
- **SEV3 (Medium):** suspicious sign-in; compromise not confirmed; no malicious outbound observed.
- **SEV4 (Low):** false positive / benign anomaly.

### “Go fast” conditions (skip straight to containment)
- Active attacker session observed
- Malicious forwarding rule present
- Finance/HR inboxes or privileged access
- External damage ongoing (many outbound messages)

---

## 4) Roles & RACI (minimum)

- **Incident Commander (IC):** coordinates actions, approvals, timeline.
- **Identity/Admin (IAM):** account lock, sessions, MFA, OAuth, conditional access.
- **Email Admin:** mailbox rules, forwarding, message trace, purge/recall.
- **SOC/IR Analyst:** investigation, scoping, evidence, IOC tracking.
- **Legal/Privacy:** notifications, regulatory assessment.
- **Finance/AP:** payment hold, supplier verification.
- **Comms (optional):** external/customer messaging.

---

## 5) Evidence Preservation (do this early)

**Record:**
- victim UPN/email, display name, department, manager
- first known suspicious time (UTC), user local time
- suspected vector (phish link, credential stuffing, OAuth app)
- attacker indicators: IPs, user agents, devices, OAuth app IDs, forwarding addresses
- affected external parties and fraudulent requests

**Preserve:**
- sign-in logs (Entra ID / Google), audit logs
- mailbox audit logs (message access, rules, delegates)
- message trace for outbound spam/phish
- copies of fraudulent emails (full headers)
- screenshots/exports from investigation consoles (date/time-stamped)

**Chain-of-custody:** If you have legal/insurance needs, store exports in a restricted evidence folder and record who accessed what and when.

---

## 6) Investigation Workflow (Identify + Scope)

### 6.1 Confirm compromise
Indicators include:
- risky sign-in, unfamiliar IP/geo/device
- successful sign-in followed by rule creation / forwarding
- OAuth “consent” granting Mail.Read, offline_access, IMAP/SMTP scopes
- new mailbox delegation (Full Access / Send As / Send on Behalf)
- password/MFA changes not initiated by user
- outbound emails with unusual language, urgency, or payment instructions

### 6.2 Determine attacker entry vector
Common vectors:
- **Credential phishing** (classic login page).
- **MFA fatigue / push bombing** (user approves prompt).
- **Token theft** (cookie/session hijack).
- **OAuth consent** (malicious app).
- **Password reuse + credential stuffing**.
- **Legacy auth** (IMAP/POP/SMTP) if enabled.

### 6.3 Scope impact
- **Time window:** last known good → containment timestamp
- **Email activity:** outbound phishing/fraud emails; auto-replies
- **Mailbox settings:** rules/forwarding/delegates
- **Data access:** file links sent, mailbox search, message downloads
- **Pivot attempts:** password resets targeting others; internal phish

---

## 7) Containment (stop the bleeding)

Perform actions in this order when possible.

### 7.1 Immediate containment steps
1. **Disable sign-in / lock account** (temporary).
2. **Revoke sessions / refresh tokens** (global sign-out).
3. **Reset password** (force change) and require re-auth.
4. **Reset MFA** (re-register) if compromise likely.
5. **Revoke suspicious OAuth app grants**; block app if needed.
6. **Disable mailbox forwarding** and remove malicious rules.
7. **Remove malicious delegates** / shared mailbox permissions.
8. **Quarantine / purge malicious outbound messages** if possible.
9. **Block attacker IOCs** (IPs/domains) where applicable.

### 7.2 Finance containment (if payment fraud risk)
- Put a **temporary hold** on payment changes initiated via email.
- Out-of-band verify vendor bank changes (phone number from master data).
- If payment sent: contact bank immediately for recall; notify insurers.

---

## 8) Eradication (remove persistence)

- Remove all unknown rules, forwarding, delegates, mail connectors.
- Remove malicious OAuth apps and enterprise app consents.
- Ensure **MFA is strong** (number matching, phishing-resistant where possible).
- Enforce **Conditional Access** / context-based controls (geo, device compliance).
- Disable **legacy authentication** protocols (IMAP/POP/SMTP AUTH) unless needed.
- Review endpoint posture for the user device (malware scan, browser extensions).
- Re-issue recovery codes / update security info if needed.

---

## 9) Recovery

- Re-enable account only after:
  - password reset completed
  - MFA re-registered
  - sessions revoked
  - rules/delegates reviewed
  - monitoring in place (alerts for rules/forwarding/sign-ins)

- Notify impacted recipients:
  - internal recipients: warn about phish from compromised user
  - external recipients: warn + request deletion, provide verification channel

- Post-incident monitoring (minimum 7–14 days):
  - risky sign-ins
  - new inbox rules / forwarding / delegation
  - suspicious outbound mail volume
  - OAuth consent events

---

## 10) Communications Templates (quick)

### To internal staff
- Subject: “Security Notice: Suspicious email from <user> — do not click links”
- Key points: what happened, what to do, who to contact, how to report.

### To external recipients (if needed)
- Acknowledge compromise, advise to ignore requests, confirm your official payment-change process.

### To the affected user
- Explain steps taken, guidance on recognizing phish, report MFA prompts they didn’t initiate.

---

## 11) Lessons Learned & Hardening

**Control improvements**
- phishing-resistant MFA for high-risk roles
- disable legacy auth
- alert on new inbox rules and forwarding to external domains
- DMARC/SPF/DKIM tuning + inbound anti-phishing policies
- user training focused on payment-change verification
- enforce least privilege and mailbox auditing

**Metrics**
- time to detect (TTD), time to contain (TTC)
- number of recipients phished
- number of payment attempts blocked
- recurrence within 90 days

---

## 12) Quick Checklist

### Identification
- [ ] Confirm compromise via sign-in/audit evidence
- [ ] Identify initial vector (phish, OAuth, token, stuffing)
- [ ] Establish time window

### Containment
- [ ] Disable sign-in
- [ ] Revoke sessions/tokens
- [ ] Reset password + MFA
- [ ] Remove rules/forwarding/delegates
- [ ] Quarantine/purge outbound phish

### Eradication/Recovery
- [ ] Remove OAuth apps
- [ ] Harden auth policies
- [ ] Notify affected recipients
- [ ] Monitor 7–14 days
- [ ] Conduct lessons learned

---

## Appendix: What “good” looks like in the first 60 minutes

1. Account locked + sessions revoked.
2. Evidence exports initiated.
3. Rules/forwarding/delegates removed.
4. Outbound message trace started and purge queued.
5. Finance notified if there’s any hint of payment fraud.
6. Internal alert to recipients if phish was sent internally.
