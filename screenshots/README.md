# Screenshots

This folder contains visual evidence for the Azure Sentinel Linux Honeypot Lab.

Before publishing screenshots, censor or remove sensitive information such as:

- Public IP addresses
- Subscription IDs
- Tenant IDs
- Resource IDs
- Usernames
- SSH key names
- Any secrets, tokens, or credentials

## Recommended Screenshot List

| File Name | Description |
|---|---|
| `01-resource-group.png` | Azure resource group used for the lab |
| `02-debian-vm-created.png` | Debian 12 virtual machine overview |
| `03-network-security-group-ssh-rule.png` | NSG inbound rule allowing TCP/22 |
| `04-ssh-service-running.png` | SSH service running on Debian |
| `05-local-ssh-invalid-user-events.png` | Local SSH logs showing invalid user attempts |
| `06-log-analytics-workspace.png` | Log Analytics Workspace used by Sentinel |
| `07-sentinel-enabled.png` | Microsoft Sentinel enabled on the workspace |
| `08-syslog-via-ama-connector.png` | Syslog via AMA connector page |
| `09-data-collection-rule.png` | Data Collection Rule associated with the Debian VM |
| `10-sentinel-syslog-query.png` | Sentinel query showing SSH logs in the Syslog table |
| `11-attacker-ip-summary-kql.png` | KQL query summarizing attacker IP addresses |
| `12-sentinel-ssh-attack-map.png` | Sentinel Workbook SSH attack map |

## Notes

Screenshots should support the technical story of the lab:

1. The honeypot was deployed.
2. SSH was intentionally exposed.
3. Linux authentication logs were generated.
4. Logs reached Microsoft Sentinel.
5. KQL was used to analyze attacker IPs.
6. The results were visualized in a Sentinel Workbook map.
