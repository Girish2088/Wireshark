# HTTPS/TLS Analysis - Findings

## Objective

Analyze the TLS handshake and observe how HTTPS encrypts communication between a client and a web server.

---

## Traffic Generated

Started a local HTTPS server.

Accessed:

```text
https://127.0.0.1:4443
```

Display Filter:

```text
tls
```

---

## Packet 1 - Client Hello

Purpose:

The client initiates a secure connection with the server.

Information Sent:

- Supported TLS Version
- Supported Cipher Suites
- Random Number
- TLS Extensions

Observation:

The client proposes the encryption methods that can be used for the session.

---

## Packet 2 - Server Hello

Purpose:

The server accepts the TLS negotiation.

Information Sent:

- Selected TLS Version (TLS 1.3)
- Selected Cipher Suite
- Server Random Number

Observation:

The client and server agree on the encryption algorithm for the secure connection.

---

## Packet 3 - Change Cipher Spec

Purpose:

Indicates the transition to encrypted communication.

Observation:

In TLS 1.3, this packet exists mainly for compatibility with older implementations and has little functional importance.

---

## Packet 4 - Application Data

Purpose:

Transfer encrypted application data between the client and server.

Observation:

The actual HTTP request, HTTP response, and webpage content are encrypted.

Unlike HTTP, Wireshark cannot display the HTML page or HTTP headers after encryption.

Only encrypted TLS records are visible.

---

## TLS Communication Flow

Client
    │
    │ Client Hello
    ▼
Server
    │
    │ Server Hello
    ▼
Client & Server
    │
    │ Change Cipher Spec
    ▼
Encrypted Channel Established
    │
    │ Application Data
    ▼
Secure Communication

---

## HTTP vs HTTPS

| HTTP | HTTPS |
|------|--------|
| Plain Text | Encrypted |
| HTML Visible | HTML Hidden |
| No Authentication | Certificate Authentication |
| Data Readable | Data Encrypted |

---

## Observations

- HTTPS uses TLS to encrypt communication.
- The TLS handshake occurs before any application data is exchanged.
- The client and server negotiate the encryption parameters during the handshake.
- Once the secure session is established, all application data is encrypted.
- Wireshark can identify TLS packets but cannot read the encrypted webpage content.

---

## What I Learned

- HTTPS is HTTP running over TLS.
- TLS creates a secure encrypted channel before data transfer.
- The handshake begins with **Client Hello** and **Server Hello**.
- **Change Cipher Spec** marks the transition to encrypted communication.
- After the handshake, only **Application Data** packets are visible because the contents are encrypted.
