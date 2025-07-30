# 📘 Ping & ICMP

## 🧠 What Is Ping?

**Ping** is a command-line utility that checks if a host is reachable.  
It uses **ICMP (Internet Control Message Protocol)** to:
- Send echo requests
- Receive echo replies
- Measure round-trip time (RTT)

## 🎯 Why Use Ping?

- Check if a device or server is alive/reachable
- Diagnose connectivity issues
- Test latency (what is latency?)
- Determine if DNS resolution is working (with domain names)

## 📊 What Ping Tells You

- Response time (ms)
- Packet loss (if some pings fail)
- Whether the host is reachable at all
- IP address associated with domain (if used with URL)

## 🛡️ Security Thoughts

- Innocent use: checking connectivity  
- Recon use: "Is this machine alive?"  
  Used in **host discovery** by attackers

> Disabling ICMP can prevent ping scans, but may also interfere with diagnostics.

> Ping provides **very little detail** beyond presence and latency. It’s a **first step**, not an info leak.

## 🧩 Related Tools & Concepts

- **ICMP** = the protocol behind ping
- **traceroute / tracert** = shows the hops between you and the target
- **hping** = more customizable ping-like tool (also supports TCP/UDP)
- **ICMP flood** = DoS attack that abuses ping

## ❓ Things to Revisit

- How ICMP is handled by routers and firewalls  
- How attackers use ICMP in combination with other scans  
- Can ICMP be spoofed or filtered to mislead scans?
