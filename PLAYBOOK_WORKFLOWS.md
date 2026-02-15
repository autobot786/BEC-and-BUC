# BEC and BUC Playbook Steps - Comprehensive Workflow

Based on the playbooks in your repository, here are the detailed **step-by-step workflows** for different BEC scenarios, including remediation steps:

---

## Scenario 1: Email Compromise (EC)

### **Workflow Phases**

#### **Phase 1: Identification (0-30 minutes)**

**Step 1: Confirm Compromise**
- Review sign-in logs for risky activity (unfamiliar IP/geo/device)
- Check for successful sign-in followed by rule creation/forwarding
- Look for OAuth consent granting Mail.Read, offline_access scopes
- Identify new mailbox delegation (Full Access/Send As)
- Check for password/MFA changes not initiated by user
- Review outbound emails with unusual language or payment instructions

**Step 2: Determine Entry Vector**
- Credential phishing (fake login page)
- MFA fatigue/push bombing (user approves unwanted prompt)
- Token theft (cookie/session hijack)
- OAuth consent (malicious app)
- Password reuse + credential stuffing
- Legacy auth (IMAP/POP/SMTP) if enabled

**Step 3: Scope Impact**
- Establish time window: last known good → containment timestamp
- Identify outbound phishing/fraud emails sent
- Check mailbox settings: rules/forwarding/delegates
- Review data access: file links sent, mailbox search, message downloads
- Look for pivot attempts: password resets targeting others, internal phish

---

#### **Phase 2: Containment (30-60 minutes)**

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

#### **Phase 3: Eradication (Same day)**

## 8) Eradication (remove persistence)

- Remove all unknown rules, forwarding, delegates, mail connectors.
- Remove malicious OAuth apps and enterprise app consents.
- Ensure **MFA is strong** (number matching, phishing-resistant where possible).
- Enforce **Conditional Access** / context-based controls (geo, device compliance).
- Disable **legacy authentication** protocols (IMAP/POP/SMTP AUTH) unless needed.
- Review endpoint posture for the user device (malware scan, browser extensions).
- Re-issue recovery codes / update security info if needed.

---

#### **Phase 4: Recovery (1-7 days)**

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

#### **Phase 5: Remediation & Hardening**

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

## Scenario 2: Supply Chain / TPRM BEC

### **Workflow Phases**

## 8) Incident Workflow Overview

1. **Detect & Triage** (minutes)
2. **Validate & Stop Loss** (minutes–hours)
3. **Investigate & Scope** (hours)
4. **Contain & Eradicate** (hours–days)
5. **Recover & Remediate** (days–weeks)
6. **Post‑Incident** (within 1–2 weeks)

---

#### **Phase 1: Detect & Triage (0-30 minutes)**

### 9.1 Immediate actions (do first)
- **Stop the transaction pipeline**
  - Place the payment/invoice on **HOLD** in AP/ERP.
  - If payment already sent: start **recall/trace** with your bank immediately.
- **Preserve evidence**
  - Save the suspect email **as .eml/.msg** with full headers.
  - Take screenshots/exports of ERP changes (supplier master data, PO edits).
- **Start an incident record**
  - Assign incident ID, time of discovery, reporter, suspected supplier, amount/value.

### 9.2 Quick triage questions
- What is the **requested action**? (bank change, urgent wire, PO change, delivery reroute)
- Is this a **new supplier** or established supplier?
- What is the **value** at risk? Any deadlines today?
- Are there **other recipients** (AP shared inbox, multiple buyers)?
- Is there evidence of **account compromise** (reply chain hijack, internal sender, forwarded thread)?

### 9.3 BEC red flags (supply chain)
- "We changed banks" / "new beneficiary" with urgency or confidentiality
- New remittance details that **don't match** vendor master data
- Look‑alike domains (e.g., `vend0r.com`, extra hyphen, different TLD)
- Unusual payment method (crypto, gift cards, international wire to new jurisdiction)
- Pressure to bypass normal approvals

---

#### **Phase 2: Validate & Stop Loss (0-4 hours)**

### 10.1 Out‑of‑band validation (required)
Use **known-good contact channels** from vendor records:

- Call the vendor using the number stored in your vendor master file
- Use vendor portal/ticketing to confirm the request
- If the vendor says "Yes, we requested this":
  - Still require secondary validation for **bank changes** (e.g., signed letter + portal update + call‑back)

> **Never** use phone numbers or links provided in the suspicious email.

### 10.2 Financial containment
- Mark vendor as "**Payment Hold — Security Verification**"
- Cancel/void pending payments where feasible
- If funds sent:
  - Initiate bank recall/trace and fraud process immediately
  - Capture beneficiary details, bank SWIFT/IBAN, reference numbers, timestamps
- Flag impacted invoices/POs and stop fulfillment/shipping changes if applicable

### 10.3 Technical containment (email & systems)
- Block the sender domain / look‑alike domain at the email gateway
- Quarantine/purge the message from mailboxes (if your tooling supports it)
- If internal mailbox compromise suspected:
  - Disable account sessions, reset credentials, enforce MFA, revoke OAuth tokens
  - Remove malicious inbox rules/forwarders
  - Expand scope: search for similar messages, replies, or rules across AP mailboxes

---

#### **Phase 3: Investigate & Scope (0-24 hours)**

### 11.1 Evidence collection
**Email**
- Full headers (Received chain), DKIM/SPF/DMARC results, Reply‑To, Return‑Path
- Any attachments and links (store safely; do not execute)
- Message trace results: delivery path, other recipients, similar campaigns

**ERP/Procurement**
- Vendor master data audit logs (bank details, address, contact changes)
- PO change logs; delivery address change history
- Approval workflow logs (who approved, from where, and when)

**Authentication**
- Sign-in logs for AP shared mailboxes and relevant employees
- OAuth app consents and token grants
- Suspicious IPs, impossible travel, new devices

### 11.2 Determine the attack type
**A) Spoofing / look‑alike domain**
- DMARC fail, mismatched From/Return-Path, new domain registered recently (if you track)
- No sign of vendor mailbox compromise

**B) Vendor mailbox compromised**
- Legit vendor domain, valid DKIM, known thread hijacked
- Vendor confirms "We didn't send this" or reports compromise
- Similar emails sent to multiple customers

**C) Your org mailbox compromised**
- Emails sent from internal accounts or rules forwarding supplier mail to attacker
- ERP changes made by compromised internal accounts

**D) Third‑party platform compromise**
- Invoice/PO altered within invoicing portal/EDI provider
- Authentication anomalies on the platform
- Vendor and buyer email both look normal but the system record is changed

### 11.3 Scope questions (TPRM specific)
- Which **suppliers** were targeted? (one, several, strategic vendors)
- Which **business processes** were touched? (bank changes, PO approvals, shipping)
- Any **contractual notification** obligations with the supplier or platform provider?
- Do we need to initiate a **supplier security incident inquiry** (attestation, IR summary, timeline)?

---

#### **Phase 4: Contain & Eradicate (same day - 7 days)**

### 12.1 Containment actions (choose based on scenario)
**If spoofing / look‑alike**
- Block domains and similar patterns; update anti‑impersonation policies
- Add banner warnings for "external sender" and vendor name mismatches
- Implement stricter controls for vendor bank changes

**If vendor mailbox compromise**
- Notify vendor via known-good channel; request they:
  - Reset passwords, enforce MFA, revoke suspicious OAuth apps
  - Remove forwarding rules
  - Provide indicators (malicious IPs/domains) and a customer advisory, if appropriate
- Internally:
  - Block attacker infrastructure (domains, addresses)
  - Search and purge similar emails
  - Audit recent vendor-related transactions

**If internal mailbox compromise**
- Full IR for compromised accounts:
  - Contain: disable, reset, MFA, revoke tokens/sessions
  - Investigate: mailbox audit, rule creation, sent items
  - Identify other accounts via lateral movement indicators
- Lock down vendor master data changes and review all recent changes

**If third-party platform compromise**
- Disable integration tokens / API keys if suspected
- Coordinate with provider's security team; request incident details and logs
- Consider pausing platform use for payment-critical changes until verified

### 12.2 Eradication
- Remove malicious rules/forwarders, block OAuth apps, rotate API keys
- Update ERP role permissions; require step-up auth for supplier master edits
- Patch/configure email security controls (DMARC enforcement, anti-spoof rules, impersonation protection)

---

#### **Phase 5: Recover & Remediate (1-30 days)**

## 13) Phase 5 — Recover & Remediate (1–30 days)

### 13.1 Financial recovery
- Track all attempted and completed transfers; reconcile with bank statements
- Work with bank(s) and law enforcement reporting channels promptly
- Document recovery actions and outcomes (recall success, partial recovery, loss)

### 13.2 Business recovery
- Re-issue legitimate payments via validated channels
- Restore procurement/AP workflow with reinforced verification steps
- Communicate internally: "What changed in process; how to handle vendor requests"

### 13.3 TPRM remediation & re-assessment
- Re-score supplier risk based on incident facts
- Require remediation evidence from supplier/provider:
  - MFA enforcement, log retention, incident response process, user training, domain protection
- Consider contractual actions:
  - Security addendum, audit rights, incident notification SLAs, indemnities (as applicable)
- Update vendor onboarding and periodic review checklists

---

## Quick Reference Checklists

### **Email Compromise - First 60 Minutes**

## Appendix: What "good" looks like in the first 60 minutes

1. Account locked + sessions revoked.
2. Evidence exports initiated.
3. Rules/forwarding/delegates removed.
4. Outbound message trace started and purge queued.
5. Finance notified if there's any hint of payment fraud.
6. Internal alert to recipients if phish was sent internally.

### **Supply Chain/TPRM - Time-based Checklists**

## 17) Quick Checklists

### 17.1 15-minute checklist
- [ ] Put invoice/payment on HOLD
- [ ] Preserve email with full headers
- [ ] Identify vendor + amount + due date
- [ ] Notify IR Lead + AP Lead
- [ ] Begin out-of-band vendor validation

### 17.2 4-hour checklist
- [ ] Bank recall started (if applicable)
- [ ] Block malicious domain/address + purge messages
- [ ] Pull ERP vendor master audit logs
- [ ] Determine likely attack type (spoof vs vendor compromise vs internal compromise vs platform)
- [ ] Notify TPRM owner and Procurement lead

### 17.3 24-hour checklist
- [ ] Complete scoping: affected suppliers, invoices, POs, mailboxes
- [ ] Confirm supplier/provider remediation steps
- [ ] Implement control improvements for bank changes
- [ ] Draft internal advisory and process reminders
- [ ] Capture lessons learned items and owners

---

These workflows provide **actionable, time-bound steps** for both Email Compromise and Supply Chain/TPRM scenarios, complete with containment, eradication, recovery, and remediation measures. Each phase includes specific technical actions, communication requirements, and verification steps to ensure comprehensive incident response.