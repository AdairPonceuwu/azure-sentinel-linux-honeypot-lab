# Lab Setup

## Purpose

This document explains the initial setup of the Azure Sentinel Linux Honeypot Lab.

The goal of this phase was to deploy a Debian 12 virtual machine in Microsoft Azure and expose SSH over TCP/22 in a controlled way so that real authentication attempts could be collected and analyzed later in Microsoft Sentinel.

## Environment

| Component | Value |
|---|---|
| Cloud provider | Microsoft Azure |
| Operating system | Debian 12 Bookworm |
| Exposed service | OpenSSH Server |
| Public port | TCP/22 |
| SIEM | Microsoft Sentinel |
| Log source | Linux Syslog |

## High-Level Setup Steps

1. Create an Azure resource group.
2. Deploy a Debian 12 virtual machine.
3. Configure inbound SSH access using the Network Security Group.
4. Validate that SSH is running on the VM.
5. Generate or observe SSH authentication activity.
6. Prepare the VM for log forwarding to Microsoft Sentinel.

## 1. Resource Group

A dedicated resource group was created to keep all lab resources isolated.

Suggested name:

```text
rg-sentinel-linux-honeypot
```

The resource group contains the Debian VM, networking resources, Log Analytics Workspace, and Microsoft Sentinel configuration.

## 2. Debian Virtual Machine

A Debian 12 virtual machine was created as the honeypot.

Suggested VM name:

```text
vm-debian-honeypot
```

Recommended configuration:

| Setting | Recommended Value |
|---|---|
| Image | Debian 12 Bookworm |
| Size | Small student/free-tier compatible size |
| Public IP | Enabled |
| Authentication | SSH key |
| Inbound port | SSH TCP/22 |

## 3. Network Security Group

The Network Security Group was configured to allow inbound SSH traffic.

Recommended inbound rule:

| Setting | Value |
|---|---|
| Source | Any |
| Source port | Any |
| Destination | VM |
| Destination port | 22 |
| Protocol | TCP |
| Action | Allow |
| Purpose | Allow SSH traffic to reach the honeypot |

This exposure was intentional and limited to SSH.

## 4. Validate SSH Service

After connecting to the VM, the SSH service was validated locally:

```bash
systemctl status ssh
```

Expected result:

```text
Active: active (running)
```

Useful log command:

```bash
sudo journalctl -u ssh --since "30 minutes ago"
```

Filtered SSH authentication logs:

```bash
sudo journalctl -u ssh --since "30 minutes ago" | grep -Ei "invalid|failed|accepted|closed|reset"
```

## 5. Observed SSH Events

During the lab, SSH activity from public IP addresses was observed.

Relevant log patterns included:

```text
Invalid user <username> from <source-ip>
Connection closed by invalid user <username> from <source-ip>
Connection reset by invalid user <username> from <source-ip>
Accepted publickey for <user> from <source-ip>
```

## Screenshots to Capture

Recommended screenshots for this phase:

| Evidence | Suggested File |
|---|---|
| Resource group | `screenshots/01-resource-group.png` |
| Debian VM overview | `screenshots/02-debian-vm-created.png` |
| NSG inbound SSH rule | `screenshots/03-network-security-group-ssh-rule.png` |
| SSH service running | `screenshots/04-ssh-service-running.png` |
| Local SSH logs | `screenshots/05-local-ssh-invalid-user-events.png` |

## Notes

This lab was intentionally exposed to collect security telemetry. The VM should not contain sensitive data and should remain isolated from production systems.
