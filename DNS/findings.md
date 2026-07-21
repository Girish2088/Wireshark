# DNS Analysis - Findings

## Objective

Analyze how DNS resolves a domain name into an IP address.

---

## Traffic Generated

```bash
nslookup google.com
```

Display Filter:

```text
dns
```

---

## Packet 1 - DNS Query (A Record)

Source IP:
10.0.2.15

Destination IP:
10.38.214.27

Protocol:
DNS over UDP

Source Port:
49402

Destination Port:
53

Transaction ID:
0x32cb

Query Type:
A (IPv4 Address)

Query Name:
google.com

Purpose:

The client requests the IPv4 address associated with **google.com**.

---

## Packet 2 - DNS Response (A Record)

Source IP:
10.38.214.27

Destination IP:
10.0.2.15

Protocol:
DNS over UDP

Transaction ID:
0x32cb

Response:

google.com → 192.178.173.139

Purpose:

The DNS server replies with the IPv4 address of **google.com**.

---

## Packet 3 - DNS Query (AAAA Record)

Source IP:
10.0.2.15

Destination IP:
10.38.214.27

Protocol:
DNS over UDP

Transaction ID:
0x2efa

Query Type:
AAAA (IPv6 Address)

Query Name:
google.com

Purpose:

The client requests the IPv6 address associated with **google.com**.

---

## Packet 4 - DNS Response (AAAA Record)

Source IP:
10.38.214.27

Destination IP:
10.0.2.15

Protocol:
DNS over UDP

Transaction ID:
0x2efa

Response:

google.com → 2404:6800:...

Purpose:

The DNS server replies with the IPv6 address of **google.com**.

---

## Packet Flow

Client (10.0.2.15)
        │
        │ DNS Query (A)
        ▼
DNS Server (10.38.214.27)
        │
        │ DNS Response (IPv4)
        ▼
Client

        │
        │ DNS Query (AAAA)
        ▼
DNS Server
        │
        │ DNS Response (IPv6)
        ▼
Client

---

## Observations

- DNS uses UDP as the transport protocol.
- DNS queries are sent to destination port **53**.
- The client uses a temporary (ephemeral) source port (**49402** in this capture).
- The Transaction ID matches the query and its corresponding response.
- Two DNS lookups were performed:
  - A Record (IPv4)
  - AAAA Record (IPv6)

---

## What I Learned

- DNS translates domain names into IP addresses.
- DNS typically uses UDP for faster communication.
- The client first sends a query, then the DNS server returns the matching response.
- A records provide IPv4 addresses.
- AAAA records provide IPv6 addresses.
- The Transaction ID is used to match a DNS response with its original query.
