# ICMP Analysis - Findings

## Objective

Analyze ICMP Echo Request and Echo Reply packets generated using the `ping` command.

---

## Traffic Generated

```bash
ping -c 2 google.com
```

Display Filter:

```text
icmp
```

---

## Packet 1 - ICMP Echo Request

Source IP:
10.0.2.15

Destination IP:
142.250.193.14

Protocol:
ICMP

Type:
8 (Echo Request)

Code:
0

Identifier:
0x9125

Sequence Number:
1

Purpose:

The client sends an ICMP Echo Request to check whether the destination host is reachable.

---

## Packet 2 - ICMP Echo Reply

Source IP:
142.250.193.14

Destination IP:
10.0.2.15

Protocol:
ICMP

Type:
0 (Echo Reply)

Code:
0

Identifier:
0x9125

Sequence Number:
1

Purpose:

The destination host replies, confirming that it is reachable.

---

## Packet 3 - ICMP Echo Request

Source IP:
10.0.2.15

Destination IP:
142.250.193.14

Protocol:
ICMP

Type:
8 (Echo Request)

Code:
0

Identifier:
0x9125

Sequence Number:
2

Purpose:

The client sends the second Echo Request as part of the ping operation.

---

## Packet 4 - ICMP Echo Reply

Source IP:
142.250.193.14

Destination IP:
10.0.2.15

Protocol:
ICMP

Type:
0 (Echo Reply)

Code:
0

Identifier:
0x9125

Sequence Number:
2

Purpose:

The destination host successfully replies to the second Echo Request.

---

## Packet Flow

Client (10.0.2.15)
        │
        │ Echo Request (Type 8)
        ▼
Google Server (142.250.193.14)
        │
        │ Echo Reply (Type 0)
        ▼
Client

        │
        │ Echo Request (Type 8)
        ▼
Google Server
        │
        │ Echo Reply (Type 0)
        ▼
Client

---

## Observations

- ICMP does not use TCP or UDP ports.
- Echo Request packets use **Type 8**.
- Echo Reply packets use **Type 0**.
- The Identifier remains the same for all packets in the same ping session.
- The Sequence Number increases with each Echo Request.
- The successful Echo Reply confirms that the destination host is reachable.

---

## What I Learned

- ICMP is used for network diagnostics and connectivity testing.
- The `ping` command uses ICMP Echo Request and Echo Reply messages.
- ICMP operates directly over IP and does not use port numbers.
- Each Echo Request should normally receive a matching Echo Reply.
- Wireshark allows easy analysis of request/reply timing, identifiers, and sequence numbers.
