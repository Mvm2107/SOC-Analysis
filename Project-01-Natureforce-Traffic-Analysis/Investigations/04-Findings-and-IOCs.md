# 04 – Investigation Findings and Indicators of Compromise

## 1. Executive Summary

A network traffic investigation was conducted using Wireshark to examine suspicious communication involving a Windows client in the Natureforce domain environment.

The investigation identified the internal Windows client, examined domain authentication activity, identified an HTTP executable download and reconstructed the transferred file.

The executable was retained for further analysis and was not executed.

## 2. Identified Windows Client

| Attribute | Finding |
|---|---|
| IP address | 10.10.1.128 |
| MAC address | 00:22:fb:ec:7e:e7 |
| Hostname | DESKTOP-4VGYQX7 |
| Associated user principal | bdiaz |
| Domain | NATUREFORCE.COM |
| Domain Controller | 10.10.1.10 |

## 3. Suspicious External Communication

| Attribute | Finding |
|---|---|
| External IP | 107.175.82.242 |
| Destination port | 9000 |
| Protocol | HTTP over TCP |
| HTTP method | GET |
| Requested resource | /wilow/runner_ilove.exe |
| HTTP response | 200 OK |
| User-Agent | curl/8.2.1 |

## 4. Extracted File Details

| Attribute | Finding |
|---|---|
| Filename | runner_ilove.exe |
| File type | Windows executable |
| File size | 1,646,592 bytes |
| SHA-256 | B789ADCE70C028A6D17BE45EC34667AA95586350EE0C4057E75B138B1AD35061 |

The extracted file size matched the HTTP Content-Length header.

## 5. Indicators of Interest

| Indicator | Value | Context |
|---|---|---|
| IP address | 107.175.82.242 | External HTTP server |
| Port | 9000/TCP | HTTP communication |
| URL path | /wilow/runner_ilove.exe | Requested executable |
| Filename | runner_ilove.exe | Extracted executable |
| SHA-256 | B789ADCE70C028A6D17BE45EC34667AA95586350EE0C4057E75B138B1AD35061 | File identification |

These are investigation indicators, not independently confirmed malicious IOCs.

## 6. Recommended SOC Actions

1. Validate the external IP and file hash using approved threat intelligence sources.
2. Investigate endpoint telemetry for evidence of file execution.
3. Review process creation and Windows security logs.
4. Check whether additional systems communicated with the external server.
5. If malicious activity is confirmed, follow the organisation's containment and incident response procedures.
6. Preserve relevant network and endpoint evidence.

## 7. Investigation Limitations

- The investigation was based on the supplied PCAP.
- File extraction and hash calculation were completed.
- The executable was not executed.
- Static and dynamic malware analysis have not yet been performed.
- The packet capture alone does not establish successful malware execution or confirmed endpoint compromise.

## 8. Final Conclusion

The investigation successfully identified the Windows client and its associated domain authentication activity, traced suspicious HTTP communication and reconstructed the transferred executable.

The extracted file's SHA-256 hash was calculated and documented for future identification.

Further malware analysis and endpoint investigation are required before confirming malicious execution or determining the full impact of the activity.
