# Project 01 – Natureforce Network Traffic Analysis

## Project Overview

A hands-on Security Operations Center (SOC) investigation using Wireshark to analyse suspicious network communication involving a Windows client in an Active Directory environment.

The investigation focused on identifying the affected Windows client, examining domain authentication activity, analysing suspicious HTTP traffic and extracting a transferred Windows executable.

## Scenario

Two security alerts were reported:

1. An executable file was transferred from external IP `107.175.82.242` over TCP port `9000`.
2. Suspicious communication involving `195.64.128.106` over TCP port `443`.

A packet capture (PCAP) from the associated Windows environment was provided for investigation.

## Tools Used

- Wireshark – Network traffic analysis and protocol investigation.
- PowerShell – File verification and SHA-256 calculation.

## Investigation Environment

| Attribute | Value |
|---|---|
| Internal Windows client | 10.10.1.128 |
| Client hostname | DESKTOP-4VGYQX7 |
| MAC address | 00:22:fb:ec:7e:e7 |
| Associated user principal | bdiaz |
| Domain Controller | 10.10.1.10 |
| Domain | NATUREFORCE.COM |

## Investigation Workflow

| Phase | Investigation | Result |
|---|---|---|
| 01 | DHCP analysis | Client IP and MAC identified |
| 02 | NBNS analysis | Hostname identified |
| 03 | SMB2 analysis | Domain file-sharing communication examined |
| 04 | Kerberos analysis | Domain authentication activity examined |
| 05 | TCP analysis | External connection established |
| 06 | HTTP analysis | Executable download identified and extracted |
| 07 | SHA-256 verification | Extracted file hash calculated |

## Key Findings

### Suspicious External Communication

- External IP: `107.175.82.242`
- Destination port: `9000`
- Requested file: `runner_ilove.exe`
- HTTP response: `200 OK`
- File size: `1,646,592 bytes`

### Extracted File

**Filename:** `runner_ilove.exe`

**SHA-256:**
`B789ADCE70C028A6D17BE45EC34667AA95586350EE0C4057E75B138B1AD35061`

## Investigation Documentation

Detailed investigation reports are available below:

1. [Network Discovery](Investigations/01-Network-Discovery.md)
2. [Windows Authentication](Investigations/02-Windows-Authentication.md)
3. [Suspicious HTTP File Transfer](Investigations/03-Suspicious-HTTP-File-Transfer.md)
4. [Findings and IOCs](Investigations/04-Findings-and-IOCs.md)

## Evidence

Screenshots supporting the investigation are available in the [Evidence folder](Evidence/).

## Security Recommendations

- Investigate the executable using safe static malware analysis.
- Review endpoint process creation and execution logs.
- Validate the external IP and file hash against threat intelligence.
- Investigate other systems communicating with the same destination.
- Follow incident response procedures if malicious activity is confirmed.

## Project Status

**Network traffic investigation and initial file extraction: Completed.**

**Static malware analysis and endpoint execution verification: Pending.**

## Disclaimer

This project was conducted for cybersecurity learning and SOC investigation practice. The executable was extracted and hashed but was not executed. Malicious execution has not been confirmed.
