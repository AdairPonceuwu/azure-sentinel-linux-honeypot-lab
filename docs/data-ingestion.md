# Data Ingestion

## Purpose

This document explains how Linux SSH authentication logs were forwarded from the Debian honeypot to Microsoft Sentinel.

The lab uses the `Syslog via AMA` connector, Azure Monitor Agent, and a Data Collection Rule to collect Linux authentication logs.

## Data Flow

```text
Debian VM
  ↓
Linux Syslog / sshd logs
  ↓
Azure Monitor Agent
  ↓
Data Collection Rule
  ↓
Log Analytics Workspace
  ↓
Microsoft Sentinel
  ↓
Syslog table
```

## Connector Used

The correct connector for this Linux-based lab is:

```text
Syslog via AMA
```

The Windows connector was not used because this lab was implemented on Debian Linux instead of Windows.

## Data Collection Rule

A Data Collection Rule was created and associated with the Debian VM.

Recommended DCR name:

```text
dcr-linux-syslog-honeypot
```

## Syslog Facilities

The DCR was configured to collect authentication-related Syslog facilities.

Recommended configuration:

| Facility | Minimum Level |
|---|---|
| LOG_AUTH | Info |
| LOG_AUTHPRIV | Info |

Other facilities can remain disabled unless additional Linux telemetry is needed later.

## Validate Local Logging

Before validating Microsoft Sentinel, local SSH logs were checked on the Debian VM:

```bash
sudo journalctl -u ssh --since "30 minutes ago"
```

Filtered command:

```bash
sudo journalctl -u ssh --since "30 minutes ago" | grep -Ei "invalid|failed|accepted|closed|reset"
```

## Validate Agent

The Azure Monitor Agent can be checked with:

```bash
systemctl status azuremonitoragent
```

If logs do not appear in Sentinel, useful checks include:

```bash
systemctl status rsyslog
```

```bash
sudo journalctl -u ssh --since "10 minutes ago"
```

## Validate Logs in Microsoft Sentinel

In Microsoft Sentinel, the logs should appear in the `Syslog` table.

Basic validation query:

```kql
Syslog
| where TimeGenerated > ago(1h)
| sort by TimeGenerated desc
```

SSH-specific validation query:

```kql
Syslog
| where TimeGenerated > ago(1h)
| where ProcessName has "sshd"
| project TimeGenerated, Computer, HostName, ProcessName, SyslogMessage
| sort by TimeGenerated desc
```

## Expected Results

Expected SSH log messages include:

```text
Invalid user
Connection closed by invalid user
Connection reset by invalid user
Failed password
Accepted publickey
```

## Troubleshooting

### No logs in Sentinel

Check:

1. The Data Collection Rule is associated with the Debian VM.
2. `LOG_AUTH` and `LOG_AUTHPRIV` are set to `Info`.
3. Azure Monitor Agent is installed and running.
4. SSH logs exist locally.
5. Enough time has passed for logs to arrive in Log Analytics.

### Wrong table

For Linux, use:

```text
Syslog
```

Do not use:

```text
SecurityEvent
```

`SecurityEvent` applies to Windows security logs, not this Debian-based lab.

## Screenshots to Capture

| Evidence | Suggested File |
|---|---|
| Syslog via AMA connector | `screenshots/08-syslog-via-ama-connector.png` |
| Data Collection Rule | `screenshots/09-data-collection-rule.png` |
| Sentinel Syslog query results | `screenshots/10-sentinel-syslog-query.png` |
