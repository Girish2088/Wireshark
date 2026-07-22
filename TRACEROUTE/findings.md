# Traceroute Analysis - Findings

## Objective

Analyze how `traceroute` discovers the path between a source and a destination by using UDP probes and ICMP responses.

---

## Traffic Generated

Command Used:

```bash
traceroute google.com
```

Display Filters:

```text
udp
```

```text
icmp
```

---

## Packet 1 - UDP Probe

Purpose:

The client sends a UDP packet with a specific TTL value toward the destination.

Observation:

- Source IP: 10.0.2.15
- Destination IP: 142.251.106.139
- Destination Port: 33434 (high UDP port)

The first probe is transmitted with a low TTL value.

---

## Packet 2 - ICMP Time Exceeded

Purpose:

A router decreases the packet's TTL by one.

When TTL reaches 0, the router discards the packet and sends an ICMP **Time Exceeded** message back to the sender.

Observation:

Router:

```
10.0.2.2
```

returned:

```
ICMP Time-to-live exceeded
```

This identifies the first hop in the network path.

---

## Repeated Process

Traceroute repeats this process using higher TTL values.

Example:

```
TTL = 1
↓

Router 1
↓

ICMP Time Exceeded

TTL = 2
↓

Router 2
↓

ICMP Time Exceeded

TTL = 3
↓

Router 3
↓

ICMP Time Exceeded

...
```

Each router that causes the TTL to expire reveals itself by sending an ICMP reply.

---

## Final Destination

When the UDP probe finally reaches the destination host, no application is listening on UDP port **33434**.

The destination replies with:

```
ICMP Destination Unreachable
Code: Port Unreachable
```

Traceroute interprets this as:

> The destination has been reached.

---

## Why More Packets Than Hops?

Although the terminal displayed approximately **30 hops**, Wireshark captured **101 packets**.

Reason:

By default, Linux traceroute sends **3 UDP probes for every hop**.

Example:

```
Hop 1
├── UDP Probe 1
├── UDP Probe 2
└── UDP Probe 3

Hop 2
├── UDP Probe 1
├── UDP Probe 2
└── UDP Probe 3
```

Each UDP probe usually generates an ICMP response from a router.

Therefore:

```
30 Hops

≈ 90 UDP Probes

+

ICMP Replies

=

Over 100 Captured Packets
```

This behavior is normal.

---

## Packet Flow

```
Client
   │
   │ UDP Probe (TTL = 1)
   ▼
Router 1
   │
   └── ICMP Time Exceeded

Client
   │
   │ UDP Probe (TTL = 2)
   ▼
Router 2
   │
   └── ICMP Time Exceeded

...

Client
   │
   │ UDP Probe (TTL = n)
   ▼
Destination
   │
   └── ICMP Destination Unreachable
       (Port Unreachable)
```

---

## What I Learned

- Traceroute discovers network paths by manipulating the **TTL** field.
- Each router decreases the TTL by one.
- When TTL becomes zero, the router returns an **ICMP Time Exceeded** message.
- Linux traceroute uses **UDP probes** by default.
- The destination responds with **ICMP Port Unreachable**, indicating the end of the path.
- One hop displayed in the terminal usually represents **multiple packets** in Wireshark because several probes are sent for each hop.
