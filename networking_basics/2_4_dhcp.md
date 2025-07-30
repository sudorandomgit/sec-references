# 📦 DHCP – Dynamic Host Configuration Protocol

## What I Know (Kind of)
DHCP assigns IP addresses dynamically to devices in a network. Instead of manually setting IPs, devices go through a discovery/negotiation/ack process.

## The Process (DHCP DORA)
1. **Discover** – Device sends out a request looking for a DHCP server
2. **Offer** – Server offers an available IP
3. **Request** – Device says “yes please” to the offered IP
4. **Ack** – Server confirms and assigns the IP

## Questions
- What's inside these DHCP messages?
- Is the initial Discover broadcasted? Can this be spoofed?
- How does the DHCP server pick an IP to offer?
- Can DHCP lease expire? Will the device go through DORA again?
- Will the server try to give the same IP again?
- Can we configure DHCP to **always** give the same IP to a specific device (aka DHCP reservation)?
