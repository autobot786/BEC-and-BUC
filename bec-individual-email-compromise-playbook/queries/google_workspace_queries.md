# Google Workspace / Gmail — Example Queries & Checks

## Key places to look
- Admin Console → Reports → Audit and Investigation
- Security Investigation Tool (if available) for Gmail logs
- Token / OAuth access changes (third-party app access)

## Gmail / Admin audit checks
- Suspicious logins (new country, impossible travel, new device)
- “Less secure apps” / IMAP access if enabled (legacy patterns)
- Mail forwarding settings and filters
- Delegation changes (mailbox delegation)
- Third-party app access (OAuth scopes)

## What to export/preserve
- Login audit logs for user
- Gmail audit logs (message sent, message accessed, settings changes)
- List of filters and forwarding addresses
- Third-party app access list and changes

## Fast containment actions (admin)
- Suspend user
- Reset password
- Force sign-out (revoke tokens)
- Remove forwarding and malicious filters
- Remove third-party app access
