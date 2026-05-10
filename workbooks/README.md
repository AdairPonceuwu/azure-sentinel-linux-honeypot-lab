# Workbooks

This folder contains Microsoft Sentinel Workbook JSON files used in the lab.

## Files

| File | Purpose |
|---|---|
| `ssh-attack-map-workbook.json` | Map visualization for SSH authentication attempts against the Debian honeypot |

## Notes

This workbook item uses the `Syslog` table and the `geo_info_from_ip_address()` KQL function. It does not require a Microsoft Sentinel Watchlist.

To use it:

1. Open Microsoft Sentinel.
2. Go to Workbooks.
3. Create or edit a workbook.
4. Add a Query element.
5. Open Advanced editor.
6. Paste the JSON from `ssh-attack-map-workbook.json`.
