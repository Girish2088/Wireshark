````md
# OSI Layer Analysis

## Objective

Understand how a single HTTP request passes through multiple OSI layers and how Wireshark displays each layer separately.

---

## Traffic Generated

**Visited:**

http://neverssl.com

**Display Filter:**

```
http
```

---

# Packet Analysis

## Frame (Capture Information)

**Purpose:**

Represents the complete data captured by Wireshark.

**Observed:**

- Frame Number
- Arrival Time
- Frame Length
- Protocol Stack:
  ```
  Ethernet → IPv4 → TCP → HTTP
  ```

**What it means:**

Wireshark captures the complete communication and shows each protocol layer separately.

---

## Layer 2 - Ethernet II

**Purpose:**

Delivers data to the next device (Gateway/Router) inside the local network using MAC addresses.

**Observed:**

- Source MAC Address = 08:00:27:8a:35:d2
- Destination MAC Address = 52:55:0a:00:02:02
- EtherType = IPv4

**What it means:**

- Source MAC belongs to my system.
- Destination MAC belongs to the next hop (Gateway/Router).
- Ethernet Frame carries the IP Packet.
- The Frame changes at every router during the journey.

---

## Layer 3 - Internet Protocol (IPv4)

**Purpose:**

Routes packets from the source network to the destination network.

**Observed:**

- Source IP = **10.0.2.15**
- Destination IP = **34.223.124.45**
- TTL = 64
- Protocol = TCP

**What it means:**

- Source IP identifies my system.
- Destination IP identifies the web server.
- IP addresses are used for communication between different networks.
- IP addresses usually remain the same throughout the communication.

---

## Layer 4 - TCP

**Purpose:**

Provides reliable communication between the client and the server.

**Observed:**

- Source Port = **41794**
- Destination Port = **80 (HTTP)**
- Sequence Number
- Acknowledgment Number
- Flags = **PSH, ACK**

**What it means:**

- Source Port identifies my browser connection.
- Destination Port **80** indicates HTTP communication.
- TCP ensures data is delivered reliably and in the correct order.

---

## Layer 7 - HTTP

**Purpose:**

Transfers web requests between the browser and the web server.

**Observed:**

- Request Method = **GET**
- Host = **neverssl.com**
- Request URI = **/**
- HTTP Version = **HTTP/1.1**

**What it means:**

My browser is requesting the homepage (`/`) of **neverssl.com** using an HTTP GET request.

---

# Encapsulation

Each OSI layer adds its own header before sending the data.

```
Application Data
        ↓
HTTP Header
        ↓
TCP Header
        ↓
IP Header
        ↓
Ethernet Header
        ↓
Frame sent through the NIC
```

Wireshark displays each protocol header separately, making the OSI model easy to understand.

---

# What I Learned

- One network communication contains data from multiple OSI layers.
- Each layer adds its own header before transmission (Encapsulation).
- Layer 2 (Ethernet) uses MAC addresses.
- Layer 3 (IP) uses IP addresses.
- Layer 4 (TCP) uses Port Numbers and provides reliable communication.
- Layer 7 (HTTP) contains the actual web request.
- A Packet is created at Layer 3.
- A Frame is created at Layer 2 by encapsulating the Packet.
- Frames travel only to the next hop and change at every router.
- IP addresses usually remain the same from source to destination.
- Wireshark displays every protocol layer separately, helping visualize the OSI model.
````
