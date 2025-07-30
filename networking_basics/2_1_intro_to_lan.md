# 📘 Intro to LAN 

## 🧠 What is a LAN?

**LAN = Local Area Network.**  
Small, local network. Think homes, small offices, student dorms, cybercafés in 2003.  
Probably existed **before the internet**, because you need *some* networks to connect together to make a bigger one.  
Devices are usually limited—maybe 10–15 max, though no hard number was given.


## 🔁 Network Topologies

Topologies = the physical or logical layout of how devices are connected.  
We saw three in this room: **Ring**, **Bus**, and **Star**.


### 🔗 Ring Topology

- Devices connected in a closed loop (A → B → C → A).
- Traffic flows around the ring until it gets to the right device.
- ✅ Low setup complexity, handles traffic decently.
- ❌ *Huge failure point:* one link breaks = entire loop fails.
- ❌ Data passes through non-recipient devices. Not exactly privacy-friendly.

---
> 👀 Security thought: Data passing through others is DoS or tampering waiting to happen.

---

### 🚌 Bus Topology

- One main **backbone cable** that all devices plug into.
- Data flows in *both* directions.
- ✅ Cheap and simple.
- ❌ Can’t handle heavy traffic well.
- ❌ Everyone shares one highway = **eavesdropping risk**.

---
> 👀 Security thought: Probably easy to sniff traffic on a bus setup.

---

### ⭐ Star Topology

- All devices connect to a **central switch**.
- ✅ Reliable, fast, no data collisions.
- ❌ More expensive hardware, setup takes effort.
- ❌ Switch = **single point of failure** and **compromise target**.

Switches are supposed to be robust for this reason, but... yeah, if that switch goes down, *everything goes down*.


## 🔧 Mentioned But Not Explained:

- **Switch**: The central connecting device in a star topology.
- **Hub**: Mentioned as inferior to switches, but no details.
- **Repeater**: Same. 

> ❓ I don't know what hubs or repeaters actually *do* yet.

---

## ❓ Open Questions

- Are there **other topologies** besides these three?
- What would an *internet-connected version* of these topologies look like?
- Which topologies are **obsolete** vs. still used today (and *where*)?
- Do these appear in **non-device contexts**, like embedded systems or PCB layouts?
- **Security relevance**: Which topologies are most vulnerable to interception or compromise?
- Hardware layouts (barebones) of these devices ?
---

## 🚨 Router Mention

The room said routers “route” information.  
No elaboration.  
Zero explanation of:
- What’s inside a router
- How routing works
- How a router decides where to send data

> ❓ How do routers actually function? What logic do they use? What protocols?

---

## 🟡 Status

- [x] Topologies: covered and understood with light commentary
- [x] LANs: concept is familiar
- [ ] Routers: unclear
- [ ] Switches vs Hubs vs Repeaters: unknown

---

