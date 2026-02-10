# Incident Report — Individual Email Compromise (IEC)

## Executive Summary
- **Incident ID:** {{incident_id}}
- **Victim mailbox:** {{victim_email}}
- **Severity:** {{severity}}
- **Status:** {{status}}
- **Detected:** {{detected_time}}
- **Contained:** {{contained_time}}
- **Summary:** {{summary}}

## Timeline (UTC)
| Time (UTC) | Event | Source/Notes |
|---|---|---|
{{timeline_rows}}

## Impact Assessment
- **Outbound emails sent:** {{outbound_count}}
- **External recipients impacted:** {{external_recipients}}
- **Internal recipients impacted:** {{internal_recipients}}
- **Data access / exfil indications:** {{data_access}}
- **Financial impact:** {{financial_impact}}
- **Regulatory/contractual impact:** {{regulatory_impact}}

## Indicators
- **Suspicious IPs:** {{ips}}
- **User agents/devices:** {{user_agents}}
- **Forwarding addresses:** {{forwarding}}
- **OAuth apps:** {{oauth_apps}}
- **Domains/URLs:** {{domains_urls}}

## Actions Taken
### Containment
{{containment_actions}}

### Eradication
{{eradication_actions}}

### Recovery
{{recovery_actions}}

## Root Cause (Best current assessment)
{{root_cause}}

## Lessons Learned / Preventative Actions
{{lessons_learned}}
