# Microsoft 365 / Entra ID / Exchange Online — Example Queries & Checks

> Treat these as starting points. Adjust to your log retention and licensing (Defender, Purview, etc.).

## Entra ID (Azure AD) Sign-in checks (conceptual)
- Successful sign-ins from unfamiliar country/ASN
- Sign-ins with new device / browser
- Sign-ins followed by “user consent to application” events

## Microsoft 365 Defender / Advanced Hunting (KQL) examples

### Suspicious sign-ins for a user
```kusto
let U = "victim@contoso.com";
SigninLogs
| where UserPrincipalName == U
| where ResultType == 0
| project TimeGenerated, IPAddress, Location, DeviceDetail, AppDisplayName, ClientAppUsed, ConditionalAccessStatus
| order by TimeGenerated desc
```

### Inbox rule creation / modification (Unified Audit Log / OfficeActivity)
```kusto
OfficeActivity
| where OfficeWorkload == "Exchange"
| where Operation in ("New-InboxRule","Set-InboxRule","UpdateInboxRules","New-TransportRule","Set-TransportRule")
| where UserId has "victim@contoso.com"
| project TimeGenerated, Operation, UserId, ClientIP, Parameters
| order by TimeGenerated desc
```

### Mail forwarding changes
```kusto
OfficeActivity
| where OfficeWorkload == "Exchange"
| where Operation in ("Set-Mailbox","Set-MailboxFolderPermission","Add-MailboxPermission","UpdateInboxRules")
| where UserId has "victim@contoso.com"
| project TimeGenerated, Operation, UserId, ClientIP, Parameters
| order by TimeGenerated desc
```

### Outbound spam/phish from the mailbox (EmailEvents)
```kusto
let U = "victim@contoso.com";
EmailEvents
| where SenderFromAddress == U
| project Timestamp, RecipientEmailAddress, Subject, InternetMessageId, DeliveryAction, ThreatTypes
| order by Timestamp desc
```

## Exchange Online admin checks
- Mailbox **ForwardingSmtpAddress** / **DeliverToMailboxAndForward**
- Inbox rules (including rules that **delete**, **move to RSS**, or **mark as read**)
- Delegates: FullAccess / SendAs / SendOnBehalf
- OAuth apps: check enterprise app consents and user consents
- Message trace for outbound to unusual recipients

## Fast containment actions (admin)
- Block sign-in
- Revoke sessions
- Force password reset
- Reset MFA methods
- Remove rules/forwarding/delegation
