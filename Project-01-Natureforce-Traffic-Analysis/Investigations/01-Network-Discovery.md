# 01 – Network Discovery and Host Identification

## Objective

Identify the Windows client's IP address, MAC address and hostname from the provided packet capture using Wireshark.

## Tool Used

Wireshark

## Step 1 – DHCP Analysis

**Wireshark display filter:**

```text
dhcp
```

Observed the four-stage DHCP process:

1. **Discover:** The client broadcasts a request to locate a DHCP server.
2. **Offer:** The DHCP server offers an available IP address.
3. **Request:** The client requests the offered IP address.
4. **Acknowledgement:** The DHCP server confirms the IP address lease.

### Findings

| Attribute | Value |
|---|---|
| DHCP server | 10.10.1.10 |
| Assigned client IP | 10.10.1.128 |
| Client MAC | 00:22:fb:ec:7e:e7 |
| Subnet mask | 255.255.255.0 |
| Hostname | DESKTOP-4VGYQX7 |

The DHCP Request contained the client's requested IP address and hostname. The DHCP ACK confirmed the IP lease.

## Step 2 – NBNS Analysis

**Wireshark display filter:**

```text
nbns
```

Examined NetBIOS Name Service registration packets originating from `10.10.1.128`.

Observed hostname registration traffic for:

`DESKTOP-4VGYQX7`

This provided additional evidence linking the hostname to the identified Windows client.

## Conclusion

The network discovery phase identified the Windows client as:

- **IP address:** 10.10.1.128
- **MAC address:** 00:22:fb:ec:7e:e7
- **Hostname:** DESKTOP-4VGYQX7

These identifiers were used to investigate subsequent Windows authentication and suspicious external network communication.
