# Templates — BEC Supply Chain (TPRM)

**Use:** Copy/paste and adapt to your incident ticketing system.  
**Classification:** Internal

---

## 1) Vendor validation call script (bank change / urgent payment)

**Goal:** Confirm whether the vendor genuinely requested the change.

**Preparation**
- Use the phone number from the **vendor master record** or previously validated contract docs.
- Have these details ready:
  - Invoice number(s), PO number(s)
  - Requested bank/beneficiary details (do not read full account numbers aloud unless necessary)
  - Request timestamp and sender email address

**Script**
1. “Hi, this is <NAME> from <COMPANY>. We received a request related to payment details for <VENDOR>.  
   I’m calling using the number we have on file to verify authenticity.”
2. “Did your team send a request on <DATE/TIME> to <ACTION> (e.g., change bank details / send urgent wire)?”
3. If **No**:
   - “Thank you — we will treat this as potential fraud.  
      Can you confirm whether your email system or portal may be compromised?”
4. If **Yes**:
   - “Great — we will still follow our verification process.  
      Please submit the change via <PORTAL/TICKETING> and provide <SIGNED LETTER / FORM>.”
5. Confirm vendor security contact:
   - “Who is your security/IT contact for incident coordination? What is their direct line/email?”

**Close**
- “We will place payments on temporary hold until verification is complete.”

---

## 2) Bank recall / fraud notification checklist

- [ ] Payment type: wire / ACH / faster payments / SEPA / other
- [ ] Amount and currency:
- [ ] Sending account:
- [ ] Beneficiary name and account:
- [ ] Beneficiary bank (SWIFT/BIC/IBAN where available):
- [ ] Payment reference / remittance:
- [ ] Timestamp sent:
- [ ] Bank case / reference number:
- [ ] Action requested: recall / freeze / beneficiary contact / law enforcement liaison

**Notes**
- Ask the bank for: recall confirmation, expected next steps, and any required forms.

---

## 3) Internal alert message (AP / Procurement)

Subject: **Payment Verification Hold — Vendor Change Request**

Team,  
We received a request that may be a **vendor impersonation / BEC** attempt involving <VENDOR>.  
**Do not** process any bank detail changes or urgent payments for this vendor until Security and AP confirm authenticity.  

If you received a similar email or notice unusual PO/invoice changes, forward it to <SECURITY INBOX> and open an incident ticket under **IR-BEC-TPRM**.

Thanks,  
<NAME> (IR Lead)

---

## 4) Supplier incident inquiry (TPRM questionnaire)

Send to the vendor/provider via known-good channel.

1. Do you confirm a security incident affecting email accounts, invoice systems, or procurement portals?  
2. Earliest known date/time of compromise:  
3. What accounts/systems were affected? Any customer communications sent?  
4. What containment actions have you taken? (MFA, resets, token revocation, rule removal)  
5. Provide relevant indicators (malicious IPs/domains/email addresses)  
6. Provide the process you will use to validate any **future payment changes** securely.  
7. Provide a point-of-contact for incident coordination (name, role, phone).  
8. Provide a timeline for final incident report / RCA, if available.

