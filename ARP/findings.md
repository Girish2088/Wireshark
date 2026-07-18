# Findings

## Packet 1 - ARP Request

**Source:**
Client (My System)

**Destination:**
Broadcast (`ff:ff:ff:ff:ff:ff`)

**Purpose:**
Client bolta hai: **"10.0.2.2 ka MAC address kis ke paas hai?"**

**Observations:**

* Protocol: ARP
* Message Type: Request
* Request broadcast hoti hai.
* LAN ke sabhi devices ko receive hoti hai.
* Sirf jis device ka IP match karta hai, wahi reply karta hai.

---

## Packet 2 - ARP Reply

**Source:**
Gateway (Router / Mobile Hotspot)

**Destination:**
Client (My System)

**Purpose:**
Gateway bolta hai: **"Ye mera MAC address hai."**

**Observations:**

* Protocol: ARP
* Message Type: Reply
* Reply unicast hoti hai.
* Client MAC address ko ARP cache me store kar leta hai.

---

# What I Learned

* ARP ka kaam **IP address se MAC address find karna** hai.
* ARP Request **Broadcast** hoti hai.
* ARP Reply **Unicast** hoti hai.
* ARP sirf **Local Network (LAN)** me kaam karta hai.
* Client future communication ke liye MAC address **ARP cache** me save kar leta hai.
