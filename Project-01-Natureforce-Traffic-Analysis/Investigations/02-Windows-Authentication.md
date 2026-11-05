# 02 – Windows Authentication Analysis

## Objective

Investigate Windows domain authentication and file-sharing communication between the identified Windows client and the Domain Controller.

## Environment

| Attribute | Value |
|---|---|
| Windows client | 10.10.1.128 |
| Domain Controller | 10.10.1.10 |
| Domain | NATUREFORCE.COM |
| Client hostname | DESKTOP-4VGYQX7 |
| Identified user principal | bdiaz |

## Step 1 – SMB2 Traffic Analysis

**Wireshark display filter:**

```text
smb2
```

Examined SMB2 communication between the Windows client and Domain Controller over TCP port 445.

Observed the following activities:

- SMB2 protocol negotiation.
- Session setup requests and responses.
- Tree connection requests.
- Access to domain-related network resources.
- File and directory information requests.

This analysis helped establish the Windows client's communication with domain infrastructure.

## Step 2 – Kerberos Authentication Analysis

**Wireshark display filter:**

```text
kerberos
```

Investigated Kerberos authentication exchanges between the Windows client and Domain Controller over TCP port 88.

### Authentication activities observed

- AS-REQ: Authentication Service Request.
- AS-REP: Authentication Service Response.
- TGS-REQ: Ticket Granting Service Request.
- TGS-REP: Ticket Granting Service Response.

### User identification

Observed the Kerberos client principal:

`bdiaz`

The client also generated authentication requests using its computer account:

`DESKTOP-4VGYQX7$`

This provided evidence of both user and computer-account authentication activity.

### Additional observations

Observed service principal names associated with services such as:

- LDAP
- CIFS
- Netlogon

These indicate requests involving Windows domain services.

## Findings

The investigation identified domain authentication and SMB2 communication between the Windows client and Domain Controller.

The Kerberos principal `bdiaz` was identified in the captured authentication traffic.

## Conclusion

SMB2 and Kerberos analysis helped establish the Windows client's domain-related communication and identify an associated user principal.

These findings contributed to identifying the client involved in the broader network traffic investigation.

**Investigation limitation:** Authentication traffic alone does not establish that the identified user initiated the suspicious executable download.
