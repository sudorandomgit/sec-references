# 📘 MAC Addresses

## 🧠 What They Are

**MAC = Media Access Control.**  
A MAC address is a device’s **physical identifier** on a network.

Format: `xx:xx:xx:xx:xx:xx` — six pairs of hexadecimal numbers.  
- First 3 pairs = manufacturer ID (like a credit card prefix)
- Last 3 pairs = unique to the device

MACs are **tied to the hardware** (network interface card), making them:
- Mostly static
- Difficult to change without specialized tools
- Tracable if leaked

---
> 👀 Once leaked, MAC addresses could potentially be spoofed or used for tracking. Very fun, very shady.
---

## 🎯 Use Cases

- Device identification on LANs
- Filtering (MAC whitelisting/blacklisting)
- Forensics + WiFi logging

---

## 🛡️ Security Notes

- **MAC Spoofing** is a thing.
- Not easily “protected,” but can be monitored
- Tools can generate fake MACs
- Make a list of manufacturers and their mac prefixes - understand how mac spoofing (by creating, not just borrowing) could work.
> ❓ Unsure how MAC address leaks are tracked or how to detect spoofing in real time

---
