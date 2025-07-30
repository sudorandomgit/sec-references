# 📡 ARP – Address Resolution Protocol

## What I Know
ARP stands for Address Resolution Protocol. It's used to resolve **IP addresses into MAC addresses**. If I need to send a message to a particular IP, I ask the network, “Who owns this IP?” via an ARP request. The device that has that IP responds with its MAC address.

## Misnamed
Honestly, it resolves **MAC addresses**, not IPs. “MARP” (MAC Address Resolution Protocol) would've been more accurate, if it didnt sound so awful.

## Steps:
1. ARP Request: Broadcast with my MAC + target IP
2. ARP Reply: Device with target IP sends its MAC address
3. Info is cached in ARP cache for future lookups

## Questions
- What happens if multiple devices claim the same IP?
- Can I view ARP cache data manually? Can it be edited or injected?
- Can someone poison the cache? How fast would the device realize something's wrong?
