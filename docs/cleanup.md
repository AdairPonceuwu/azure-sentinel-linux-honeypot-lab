# Cleanup

## Purpose

This document explains how to safely pause or clean up the Azure Sentinel Linux Honeypot Lab.

Because the VM is intentionally exposed to the public Internet through SSH, it should not remain running longer than necessary.

## Stop the VM

When enough data has been collected, stop the VM from the Azure Portal:

```text
Azure Portal → Virtual Machines → Select VM → Stop
```

Confirm the VM reaches this state:

```text
Stopped (deallocated)
```

This is important because a VM that is only shut down from inside the operating system may not always be fully deallocated.

## Resources That May Still Generate Cost

Even after stopping the VM, some resources may still exist:

- Managed disk
- Public IP address
- Network Security Group
- Virtual network
- Log Analytics Workspace
- Microsoft Sentinel data retention
- Stored logs

Review these resources if the lab will not be used again.

## Recommended Temporary Pause

If continuing the lab later:

1. Stop and deallocate the VM.
2. Keep the resource group.
3. Keep the Log Analytics Workspace.
4. Keep the Sentinel configuration.
5. Keep the screenshots and queries in the repository.

## Recommended Full Cleanup

If the lab is complete and no longer needed:

1. Export or save any required screenshots.
2. Save KQL queries and Workbook JSON.
3. Confirm no sensitive data is present in the repository.
4. Delete the resource group.

Deleting the resource group removes the VM and related Azure resources.

## Security Cleanup

Before publishing the project, remove or censor:

- Public IP addresses
- Private IP addresses, if not needed
- Usernames
- Subscription IDs
- Tenant IDs
- Resource IDs
- SSH key names
- Any secrets or credentials

Do not upload:

```text
*.pem
*.key
*.pfx
*.env
credentials.*
secrets.*
```

## Suggested .gitignore Entries

```gitignore
# Secrets and keys
*.pem
*.key
*.pfx
*.cer
*.crt
*.env
secrets.*
credentials.*

# OS files
.DS_Store
Thumbs.db

# Local notes
private-notes/
```

## Final State

At the end of the lab, the VM should be:

```text
Stopped (deallocated)
```

or the resource group should be deleted if the lab will not be continued.
