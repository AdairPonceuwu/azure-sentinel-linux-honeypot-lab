# Lessons Learned

## Overview

This document summarizes the main lessons learned while building the Azure Sentinel Linux Honeypot Lab.

## 1. The Lab Had to Be Adapted

The original project concept used a Windows virtual machine and Windows Security Events, especially Event ID `4625`.

However, the Azure student subscription did not allow creating the required Windows VM, so the project was adapted to use Debian 12 Linux instead.

This changed the data source from:

```text
SecurityEvent
```

to:

```text
Syslog
```

And changed the detection logic from:

```text
EventID == 4625
```

to SSH log patterns such as:

```text
Invalid user
Connection closed by invalid user
Connection reset by invalid user
Failed password
```

## 2. Linux Honeypots Are Valid for SOC Labs

The Debian SSH honeypot still achieved the original goal: collecting authentication attempts from the public Internet and analyzing them in Microsoft Sentinel.

This version also added useful Linux and Syslog experience.

## 3. Syslog via AMA Is the Correct Connector

For Linux logs, the correct Sentinel connector is:

```text
Syslog via AMA
```

The `Windows Security Events via AMA` connector only applies to Windows systems.

## 4. The Correct Table Matters

At first, querying `SecurityEvent` did not return results because the lab was not using Windows logs.

For this Linux-based lab, the correct table is:

```text
Syslog
```

## 5. SSH Logs Can Have Different Formats

Not all SSH attempts generate `Failed password`.

Many real-world bot attempts generated messages such as:

```text
Invalid user ubuntu from <source-ip>
Invalid user debian from <source-ip>
Connection closed by invalid user <username> from <source-ip>
```

Because of this, the KQL detection logic should include multiple SSH log patterns instead of relying on only one message.

## 6. Watchlist Was Not Required

The original tutorial used a Sentinel Watchlist for GeoIP enrichment.

In this lab, Watchlist creation was not available, so geolocation enrichment was performed directly with:

```kql
geo_info_from_ip_address()
```

This simplified the project and kept the attack map working.

## 7. Cost and Exposure Need to Be Managed

Because the honeypot was intentionally exposed to the public Internet, it should be stopped and deallocated when not in use.

Important cleanup action:

```text
Azure Portal → Virtual Machines → Stop → Confirm Stopped (deallocated)
```

## 8. Documentation Is Part of the Lab

Captures, KQL files, architecture notes, and cleanup notes make the project more useful as a portfolio piece.

The repository was structured so future detections and analytic rules can be added later.
