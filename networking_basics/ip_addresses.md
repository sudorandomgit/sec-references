# 📘 IP Addresses

## 🧠 What are they?

**IP = Internet Protocol.**  
An IP address is a device’s logical “location” on a network.

IPv4 addresses are in the format: `x.x.x.x`, where each `x` is a number from `0` to `255`.  
These are 4 **octets**, usually written in **decimal**, though under the hood it's all binary/hex. (xx.xx.xx.xx)

Example: `192.168.1.1`

There are **public** and **private** IPs:
- **Private** = Internal communication (e.g., `192.168.x.x`)
- **Public** = Assigned by ISP, used for internet-facing connections

IPv4 addresses are running out. Enter **IPv6**:
- Format: `xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx`
- Each section is **16 bits** (two octets), in **hex**
- Offers `2^128` possible addresses. Why not IPv8? Unknown. Maybe branding. Maybe chaos.

> ❓ Unsure how IP addressing ties into routing and global internet structure yet.

---

## 🔁 Dynamic vs Static IPs

- **Dynamic**: Assigned temporarily, often via DHCP
- **Static**: Manually set, doesn't change
- IPs can change, as long as they remain unique per network.

> 👀 Spoofable? Yes. Trackable? No without a lot of updating and logging.

---

## 🧩 Related Concepts

- DHCP
- NAT (Network Address Translation)
- Subnets + CIDR (e.g., `/24`)
- IPv6 autoconfiguration

---

## ❓ Things to Revisit

- How public IPs are managed/allocated (by ISPs, IANA, etc)
- How NAT works with one public IP for many devices
- How IPv6 actually gets routed and resolved

---
