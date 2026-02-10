# BEC Playbook — Supply Chain Compromise (Third‑Party Risk / TPRM)

**Document ID:** IR-BEC-TPRM-001  
**Version:** 1.0  
**Owner:** Security Incident Response (with Procurement/AP)  
**Classification:** Internal  
**Last updated:** 2026-02-10 (Europe/London)

---

## 1) Purpose

This playbook provides **end-to-end response procedures** for **Business Email Compromise (BEC)** incidents that leverage a **supplier / third party compromise** (TPRM scenario), such as:

- Vendor email account compromise (EAC) leading to **invoice/payment diversion**
- Spoofed/look‑alike vendor domains used to request **bank detail changes** or **urgent payments**
- Compromised third‑party invoicing platforms / procurement portals used to **alter invoices, POs, or delivery details**

The goal is to **prevent loss**, **recover funds where possible**, **contain account compromise**, and **reduce recurrence** through **third‑party risk controls**.

---

## 2) Scope

**In scope**
- AP/Finance payment change requests (bank account, beneficiary, remittance)
- Procurement workflows (PO creation/approval, delivery address changes, expedited shipping)
- Supplier portals and EDI/invoicing integrations
- Executive/employee impersonation that results in supply-chain financial fraud

**Out of scope (use other playbooks)**
- Software supply chain compromise (malicious code update) — use “Software Supply Chain Incident” playbook
- Pure malware infection without business process manipulation — use “Phishing/Malware” playbook

---

## 3) Definitions

- **BEC / EAC:** Business Email Compromise / Email Account Compromise — fraud using compromised or impersonated email accounts.
- **TPRM:** Third‑Party Risk Management — governance and controls around supplier/third‑party security and business risk.
- **Mandate/Invoice fraud:** Fraud that changes beneficiary/bank details to redirect legitimate payments.

---

## 4) Activation Criteria

Activate this playbook when **any** of the following occur:

1. A supplier requests **bank account / payment detail changes** via email or unexpected channel.
2. An invoice/PO appears altered (new beneficiary, different bank, unusual urgency) and **cannot be validated**.
3. A payment is made and the supplier reports **non-payment** (possible diversion).
4. Evidence indicates a supplier’s email/domain was **spoofed or compromised**.
5. Procurement/AP systems show **unauthorized changes** to supplier master data, PO, or delivery instructions.

---

## 5) Severity & Prioritization

Use your org’s incident severity model. Suggested guidance:

- **SEV1 (Critical):** Funds transferred (or imminent transfer), senior exec impersonation, multiple suppliers impacted, or confirmed compromise of AP/ERP.
- **SEV2 (High):** Attempted diversion stopped before payment; confirmed compromised supplier mailbox; high-value supplier targeted.
- **SEV3 (Medium):** Suspicious request with no confirmed compromise; single low-value attempt; quickly contained.

---

## 6) Roles & RACI

**Core roles**
- **IR Lead (Security):** Owns incident coordination, evidence capture, technical containment.
- **Finance/AP Lead:** Freezes payments, validates invoices, leads bank recall actions.
- **Procurement/Vendor Manager:** Supplier contact, portal access, master data governance, contractual notifications.
- **IT / Email Admin:** Mailbox investigation, message tracing, filtering, account resets.
- **Legal/Compliance:** Notifications, contractual obligations, privacy/regulatory analysis.
- **Comms (Internal/External):** Approved messaging to stakeholders if needed.
- **TPRM Owner:** Supplier risk assessment, remediation requirements, re‑rating, ongoing monitoring.

**Minimum RACI (example)**
- **Freeze payment / put vendor on hold:** AP (R), Procurement (A), IR (C), Legal (I)
- **Bank recall / fraud claim:** AP (R), Legal (C), IR (I), Procurement (I)
- **Supplier engagement & attestation:** Procurement (R), TPRM (A), Legal (C), IR (C)
- **Email/ERP forensics:** IR (A/R), IT (R), AP (C)

---

## 7) Preconditions (Preparedness Checklist)

These controls dramatically reduce loss frequency:

**Email & identity**
- DMARC enforcement, SPF/DKIM, anti‑spoofing and look‑alike domain detection
- MFA for email and procurement/ERP; conditional access and device compliance
- Disable legacy auth; monitor for forwarding rules, OAuth app grants, suspicious logins

**Payment controls**
- “**No email-only bank changes**” policy — require portal workflow + out‑of‑band verification
- Dual authorization for bank detail updates and first payment to new beneficiary
- Vendor master data changes restricted to a small group with elevated logging
- Call‑back procedures using **known-good numbers** from the vendor record, not from email

**TPRM & contracts**
- Supplier incident notification clauses and response SLAs
- Documented secure channels for payment changes (portal, ticketing, encrypted email)
- Periodic supplier contact validation (phone/email) and domain allow‑listing

---

## 8) Incident Workflow Overview

1. **Detect & Triage** (minutes)
2. **Validate & Stop Loss** (minutes–hours)
3. **Investigate & Scope** (hours)
4. **Contain & Eradicate** (hours–days)
5. **Recover & Remediate** (days–weeks)
6. **Post‑Incident** (within 1–2 weeks)

---

## 9) Phase 1 — Detect & Triage (0–30 minutes)

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
- “We changed banks” / “new beneficiary” with urgency or confidentiality
- New remittance details that **don’t match** vendor master data
- Look‑alike domains (e.g., `vend0r.com`, extra hyphen, different TLD)
- Unusual payment method (crypto, gift cards, international wire to new jurisdiction)
- Pressure to bypass normal approvals

---

## 10) Phase 2 — Validate & Stop Loss (0–4 hours)

### 10.1 Out‑of‑band validation (required)
Use **known-good contact channels** from vendor records:

- Call the vendor using the number stored in your vendor master file
- Use vendor portal/ticketing to confirm the request
- If the vendor says “Yes, we requested this”:
  - Still require secondary validation for **bank changes** (e.g., signed letter + portal update + call‑back)

> **Never** use phone numbers or links provided in the suspicious email.

### 10.2 Financial containment
- Mark vendor as “**Payment Hold — Security Verification**”
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

## 11) Phase 3 — Investigate & Scope (0–24 hours)

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
- Vendor confirms “We didn’t send this” or reports compromise
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

## 12) Phase 4 — Contain & Eradicate (same day–7 days)

### 12.1 Containment actions (choose based on scenario)
**If spoofing / look‑alike**
- Block domains and similar patterns; update anti‑impersonation policies
- Add banner warnings for “external sender” and vendor name mismatches
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
- Coordinate with provider’s security team; request incident details and logs
- Consider pausing platform use for payment-critical changes until verified

### 12.2 Eradication
- Remove malicious rules/forwarders, block OAuth apps, rotate API keys
- Update ERP role permissions; require step-up auth for supplier master edits
- Patch/configure email security controls (DMARC enforcement, anti-spoof rules, impersonation protection)

---

## 13) Phase 5 — Recover & Remediate (1–30 days)

### 13.1 Financial recovery
- Track all attempted and completed transfers; reconcile with bank statements
- Work with bank(s) and law enforcement reporting channels promptly
- Document recovery actions and outcomes (recall success, partial recovery, loss)

### 13.2 Business recovery
- Re-issue legitimate payments via validated channels
- Restore procurement/AP workflow with reinforced verification steps
- Communicate internally: “What changed in process; how to handle vendor requests”

### 13.3 TPRM remediation & re-assessment
- Re-score supplier risk based on incident facts
- Require remediation evidence from supplier/provider:
  - MFA enforcement, log retention, incident response process, user training, domain protection
- Consider contractual actions:
  - Security addendum, audit rights, incident notification SLAs, indemnities (as applicable)
- Update vendor onboarding and periodic review checklists

---

## 14) Communications & Reporting

### 14.1 Internal notifications (minimum)
- CISO / Head of Security (SEV1–SEV2)
- CFO / Finance Director (if funds at risk or any transfer occurred)
- Procurement leadership + TPRM owner (any confirmed vendor compromise)

### 14.2 External notifications (as applicable)
- **Bank**: immediate fraud notification and recall/trace request
- **Law enforcement**: file report promptly via the appropriate national channel (e.g., FBI IC3 in the US; Action Fraud in the UK)
- **Impacted supplier(s)**: known-good channel only; coordinate on joint customer messaging if needed
- **Regulators / customers**: only if required (e.g., personal data exposure or material incident thresholds)

> Legal/Compliance should approve any external statements.

---

## 15) Evidence Handling

- Preserve: suspect emails (.eml/.msg), headers, message traces, ERP audit logs, bank confirmations
- Maintain chain-of-custody for legal hold
- Store artifacts in your incident management system with restricted access

---

## 16) Decision Points (Rapid)

**Payment sent?**
- Yes → immediate bank recall + law enforcement reporting + SEV elevation
- No → proceed with validation + containment; still investigate compromise

**Compromise location?**
- Vendor / internal / platform → pick containment track (Section 12)

**Repeat risk?**
- If multiple suppliers targeted or systemic control gap → initiate enterprise-wide control uplift

---

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

## 18) Templates (See /playbooks/templates)

- Vendor validation call script
- Bank recall request checklist
- Internal notification message for AP/Procurement
- Supplier incident inquiry questionnaire (TPRM)

---

## 19) Metrics (Track and Trend)

- Time-to-hold (minutes from report to payment hold)
- Funds at risk vs. prevented vs. recovered
- % vendor bank changes using approved workflow (no email-only)
- Number of suppliers impacted per incident
- Mean time to vendor confirmation (out-of-band)
- Control gaps found per incident (and closure time)

---

## 20) References (external guidance)

- FBI — Business Email Compromise: https://www.fbi.gov/how-we-can-help-you/scams-and-safety/common-frauds-and-scams/business-email-compromise  
- FBI IC3 PSA (11 Sep 2024) — “Business Email Compromise: The $55 Billion Scam”: https://www.ic3.gov/PSA/2024/PSA240911  
- NIST SP 800-61 Rev. 3 (Apr 2025) — Incident Response Recommendations and Considerations for Cybersecurity Risk Management: https://csrc.nist.gov/pubs/sp/800/61/r3/final  
- NIST SP 800-161 Rev. 1 — Cybersecurity Supply Chain Risk Management Practices (includes updates as of 11-01-2024): https://csrc.nist.gov/pubs/sp/800/161/r1/upd1/final  
- UK Action Fraud — BEC scams: https://www.actionfraud.org.uk/bec-scams/  
- NCSC Northern Ireland — Business Email Compromise: https://www.nicybersecuritycentre.gov.uk/business-email-compromise  

> Keep this playbook consistent with your organization’s incident response policy, finance controls, and local reporting obligations.
