### 🔌 Understanding Device Connection Types: Foundation for Cybersecurity and Networking

**Why Connection Types?**  
Understanding how devices connect — by wire, air, light, or field — is the foundation of computer networking. Every protocol, packet, and vulnerability depends on the physical and link-layer realities of these connections. This map builds the mental model needed before exploring OSI layers, TCP/IP, packet captures, or infrastructure.

_Note: This map does not cover system architecture, OSI layers, full protocol stacks, or embedded logic — those are covered in separate companion maps._

---

### 🧠 FOUNDATIONAL STRUCTURE: CONNECTIONS & COMMUNICATION

## 🔹 Part A: Core Concepts (Before Even the Connection Types)

| Topic                            | Why it Matters                                                   | Resource                                                                 |
|----------------------------------|-------------------------------------------------------------------|--------------------------------------------------------------------------|
| Analog vs Digital Signals        | You can't decode RF or cables without this                        | Khan Academy – [Digital vs Analog](https://www.khanacademy.org/science/grade-7-science/xa9c5124c69e541e2:wavess/xa9c5124c69e541e2:digital-signals/v/analog-vs-digital-signals)         |
| Modulation (AM, FM, QAM, etc.)   | How data is embedded into signals                                 | YouTube: [Modulation Visualized](https://youtu.be/8e4Sf6rL3zk?si=UFOnMBKfDcCg4gOy) GfG: [Digital Modulation Techniques](https://www.geeksforgeeks.org/electronics-engineering/digital-modulation-techniques/)      |
| Spectrum, Frequency, Bandwidth   | Frequency conflicts, throughput limits                            | Khan Academy or [IEEE RF Fundamentals](https://ieeetv.ieee.org/)                                    |
| Signal Attenuation & Noise       | Why signals weaken, how interference works                        | GreatScott! – [Noise & EMI](https://www.youtube.com/watch?v=a7BXoB9z28Y)                            |
| Duplex: Half, Full, Simplex      | Important for how connections behave in real time (like UART)     | Tektronix – [What is UART](https://www.tek.com/en/learning/primer/what-uart)                       |

---

## 🔹 Part B: Wired Connection Types (Updated)

| Type               | Use Case & Notes                                                               | Resource                                                                 |
|--------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------|
| Ethernet (RJ45)    | Core of all LANs. Learn about Cat5/Cat6, full-duplex, MAC addressing            | Computerphile – [Ethernet & MAC](https://www.youtube.com/watch?v=6aGk7CjQ_Sw)                      |
| USB                | Host vs device roles, data + power channels, common security vector             | [USB In-Depth](https://www.usbmadesimple.co.uk/)                          |
| HDMI/DisplayPort   | Used in AV, can carry EDID info, potential attack surface                        | [EDID Explained](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data)               |
| UART               | Found on almost all embedded devices; used for debug and data dumps             | [SparkFun UART Tutorial](https://learn.sparkfun.com/tutorials/serial-communication)                |
| I2C & SPI          | Internal chip communication, used to access EEPROM, sensors, etc.               | Hackaday – [I2C/SPI Explained](https://hackaday.com/tag/spi/)                                      |
| CAN Bus            | Found in vehicles, industrial automation. Attackable via OBD-II ports           | [CAN Bus Basics](https://www.csselectronics.com/pages/can-bus-simple-intro-tutorial)              |
| RS-232 / RS-485    | Legacy/industrial devices (SCADA, PLC, POS machines)                            | [RS-485 Guide](https://www.ni.com/en-us/innovations/rs-485.html)                                  |
| Ethernet over Powerline (PLC) | Found in smart homes; transmits over AC power lines                   | [Powerline Networking Explained](https://www.howtogeek.com/240348/how-do-powerline-networking-kits-work/) |
| PoE (Power over Ethernet) | Powers devices like IP cameras, phones through LAN cable               | [PoE Overview](https://www.cisco.com/c/en/us/solutions/small-business/resource-center/networking/what-is-poe.html) |

---

## 🔹 Part C: Wireless Connection Types (Complete)

| Type           | Notes                                                             | Resource                                                                 |
|----------------|-------------------------------------------------------------------|--------------------------------------------------------------------------|
| Wi-Fi          | Most used. Understand 802.11 stack, WPA2/WPA3                     | CodeBasics – [How Wi-Fi Works](https://www.youtube.com/watch?v=3aGzZ0x5b2k)               |
| Bluetooth/BLE  | Classic vs Low Energy, pairing models, sniffing risks            | GreatScott! – [Bluetooth vs BLE](https://www.youtube.com/watch?v=4EorvK66g0Q)             |
| Zigbee/Z-Wave  | Used in home automation, vulnerable to replay                     | Andreas Spiess – [Zigbee Security](https://www.youtube.com/watch?v=l-KaZnY_wMM)          |
| LoRa/LoRaWAN   | Long-range IoT, slow but secure                                   | Andreas Spiess – [LoRa Explained](https://www.youtube.com/watch?v=3Qn6j1_XGvo)           |
| GPS/GNSS       | Receives satellite signal, vulnerable to spoofing                 | Real Engineering – [How GPS Works](https://www.youtube.com/watch?v=9N4xwGv5iVg)          |
| RFID/NFC       | Used in cards, tags, mobile payments                              | [NFC Security Primer](https://www.nfcworld.com/nfc-security/)                            |
| Cellular (2G–5G)| SIM cards, baseband radios, man-in-the-middle risks              | Arun Maini – [How Mobile Phones Work](https://www.youtube.com/watch?v=ZfNhnDBzLBk)       |
| IR (Infrared)  | Used in remotes, line-of-sight only                               | [How IR Remote Works](https://www.youtube.com/watch?v=8Vb1s_E2zbI)                        |
| Ultrasonic     | Rare, used in secure pairing or location                          | [Ultrasound Data Transfer](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7010283/)       |

---
## 🔹 Part D: Wireless Coordination and Coexistence Techniques

| Concept                   | Why It Matters                                                 | Resource |
|---------------------------|----------------------------------------------------------------|----------|
| Modulation (QAM, PSK, FSK)| Converts binary to analog in noise-resistant ways              | [Ben Eater: Modulation](https://www.youtube.com/watch?v=3MghfGybD80) |
| FDMA (Frequency Division) | Separates different services like Wi-Fi, LTE, Bluetooth         | [FDMA Explained](https://www.youtube.com/watch?v=Om2aGQm0sYQ) |
| TDMA (Time Division)      | Devices take turns in time slots to avoid conflict              | [TDMA Animation](https://www.youtube.com/watch?v=l2pq1XbEhNM) |
| CDMA / DSSS / FHSS        | Spread-spectrum: avoid jamming and overlap                      | [Spread Spectrum – Wikipedia](https://en.wikipedia.org/wiki/Spread_spectrum) |
| CSMA/CA                   | Used in Wi-Fi to avoid collisions by "listening before speaking"| [Wi-Fi CSMA/CA](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access_with_collision_avoidance) |
| Channel Hopping           | BLE/Zigbee hop between frequencies to dodge interference        | [Frequency Hopping – Wikipedia](https://en.wikipedia.org/wiki/Frequency-hopping_spread_spectrum) |
| Power Control             | Minimizes interference by adjusting transmit power              | Typically explained in embedded system RF tuning guides |

---
## 🔹 Part E: Layers and Framing Concepts (Extended)

| Concept             | Why It Matters                                                  | Resource                                                                 |
|---------------------|------------------------------------------------------------------|--------------------------------------------------------------------------|
| Framing             | Packets/frames distinguish boundaries between bits              | [Wireshark Framing Tutorial](https://www.wireshark.org/docs/wsug_html_chunked/)           |
| MAC addressing      | Low-level address needed before IP routing                      | [MAC Address Explained](https://www.youtube.com/watch?v=wQ5W7C-2FJQ)                      |
| CSMA/CD, CSMA/CA    | Collision handling in shared medium (Ethernet/Wi-Fi)            | [Medium Access Control](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access)     |
| Channel hopping     | Zigbee/Bluetooth avoid interference via frequency hopping       | [Bluetooth Hopping](https://en.wikipedia.org/wiki/Frequency-hopping_spread_spectrum)     |
| Addressing schemes  | MAC, IP, UUID, IMSI, PAN ID – all coexist at different layers   | [OSI vs TCP/IP Overview](https://www.youtube.com/watch?v=vv4y_uOneC0)                    |
| MTU & Fragmentation | Crucial for understanding how large packets get split; affects performance and security | [MTU in Wireshark](https://www.geeksforgeeks.org/maximum-transmission-unit-mtu-in-networking/) |
| Endianness          | Shows up in memory and packet captures; affects decoding        | [Endianness Explained](https://www.youtube.com/watch?v=JzhlfbVmXAE)                     |

---

## 🔹 Part F: Security Risks Per Type

| Topic                     | Why It Matters                                           | Resource                                                                 |
|---------------------------|-----------------------------------------------------------|--------------------------------------------------------------------------|
| Wi-Fi Security (WEP–WPA3)| Password cracking, deauth attacks                         | [Wi-Fi Hacking Intro](https://www.aircrack-ng.org/)                     |
| BLE pairing               | MITM attacks, static key usage                            | DEFCON – [BLE Hacking Talk](https://www.youtube.com/watch?v=Vwq9BdP-snM) |
| GPS Spoofing              | Redirect or mislead location tracking                     | [GPS Spoofing Analysis](https://spectrum.ieee.org/gps-spoofing)          |
| Zigbee/Z-Wave Replay      | Weak encryption and key reuse                             | [Zigbee Attacks](https://owasp.org/www-community/attacks/ZigBee_Attacks) |
| Serial/UART               | Data dump, bootloader access                              | [UART Hacking Intro](https://www.jonhoare.uk/2021/11/14/hardware-uart.html) |

---

## 🔹 Part G: Legacy or Specialized Wired Interfaces (Optional)

| Type              | Notes                                                            |
|-------------------|------------------------------------------------------------------|
| FireWire          | Rare now, but useful in forensic imaging and high-speed capture  |
| PS/2              | Can be used in HID spoofing or keyboard logging                  |
| VGA / DVI / SCART | Found in legacy A/V systems, sometimes carry EDID or serial data |
| PCMCIA / ExpressCard | Found in older laptops; used with SDR or custom tools        |
| BNC (Coaxial)     | Used in labs, industrial sensors, or RF setups                   |

---

## 🔹 Part H: What’s Next

When this map is complete, continue to:
- 🧠 *Computer & Network Architecture Map* (CPU, memory, buses)
- 🌐 *Networking Protocol Map* (OSI, TCP/IP, DNS, DHCP, HTTP)
- 📡 *Signal Processing in Security Map* (sampling, filters, side channels)

Each of these builds directly from the foundations set here.

