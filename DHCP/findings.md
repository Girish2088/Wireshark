# Findings:-

## Packet 1 - DHCP Request

**Source:**
Client (My System)

**Destination:**
DHCP Server / Broadcast

**Purpose:**
Client bolta hai: **"Mujhe ye IP address chahiye, please assign/confirm karo."**

**Observations:**

* Protocol: DHCP
* Message Type: Request
* Client network settings (IP) request karta hai.

---

## Packet 2 - DHCP ACK

**Source:**
DHCP Server

**Destination:**
Client

**Purpose:**
DHCP Server bolta hai: **"IP address assign ho gaya, ab tum ise use kar sakte ho."**

**Observations:**

* Protocol: DHCP
* Message Type: ACK
* IP address successfully assign ho gaya.
* Client ab network aur internet use kar sakta hai.

