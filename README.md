# 🐟 WireGoldfish

> A Linux packet sniffer built in **Python** using **raw sockets (`AF_PACKET`, `SOCK_RAW`)** that captures live network traffic and manually parses packets layer by layer without using packet-analysis libraries like Scapy.

WireGoldfish demonstrates how network packets travel through the Linux networking stack by decoding **Ethernet**, **IPv4**, **TCP**, **UDP**, and **ICMP** headers directly from raw bytes using Python's `struct` module.

---

## Features

- Capture live Ethernet frames using Linux raw sockets.
- Parse source and destination MAC addresses.
- Decode IPv4 packets.
- Extract:
  - IP Version
  - Header Length
  - TTL
  - Protocol
  - Source IP
  - Destination IP
- Parse ICMP packets.
- Parse TCP segments.
- Decode TCP flags:
  - URG
  - ACK
  - PSH
  - RST
  - SYN
  - FIN
- Parse UDP segments.
- Display packet payloads as hexadecimal.

---

## How It Works

The sniffer captures packets directly from the network interface using Linux's `AF_PACKET` socket family.

Every packet is received as raw bytes and decoded manually using Python's `struct.unpack()`.

### Packet Flow

```text
Network Card
      │
      ▼
AF_PACKET Raw Socket
      │
      ▼
Ethernet Parser
      │
      ▼
IPv4 Parser
      │
      ▼
TCP / UDP / ICMP Parser
      │
      ▼
Payload Decoder
      │
      ▼
Terminal Output
```

The parsing pipeline follows the network stack:

```text
Ethernet
    ↓
IPv4
    ↓
TCP / UDP / ICMP
    ↓
Payload
```

---

## Packet Parsing

### Ethernet Frame

Extracts:

- Destination MAC
- Source MAC
- EtherType

Example:

```text
Ethernet Frame:
Destination: 01:00:5E:00:00:FB
Source: F2:9A:7C:85:4C:55
Protocol: 8
```

### IPv4

Extracts:

- Version
- Header Length
- TTL
- Protocol
- Source IP
- Destination IP

### TCP

Parses:

- Source Port
- Destination Port
- Sequence Number
- Acknowledgment Number
- TCP Flags

### UDP

Parses:

- Source Port
- Destination Port
- Length

### ICMP

Parses:

- Type
- Code
- Checksum

---

## Example Output

```text
Ethernet Frame:
Destination: 01:00:5E:00:00:FB
Source: F2:9A:7C:85:4C:55
Protocol: 8

    - IPv4 Packet:
        - Version: 4
        - Header Length: 20
        - TTL: 255
        - Protocol: 17
        - Source: 192.168.1.5
        - Target: 224.0.0.251

    - UDP Segment:
        - Source Port: 5353
        - Destination Port: 5353
        - Length: 94
```

The packet above is an **mDNS (Multicast DNS)** packet commonly used for local network service discovery.

---

## Installation

### Requirements

- Linux
- Python 3
- Root privileges

Clone the repository:

```bash
git clone https://github.com/devanshbaghel18/WireGoldfish.py.git
cd Sniffer
```

Run:

```bash
sudo python3 main.py
```

> Raw sockets require administrator privileges on Linux.

---

## Project Structure

```text
Sniffer/
├── main.py
├── README.md
├── LICENSE
└── screenshots/
```

---

## Technologies Used

- Python 3
- Linux Raw Sockets (`AF_PACKET`)
- `socket`
- `struct`
- `textwrap`

---

## Networking Concepts Demonstrated

- Raw Socket Programming
- Ethernet Frame Parsing
- IPv4 Header Decoding
- TCP Header Parsing
- UDP Header Parsing
- ICMP Packet Parsing
- Bitwise Operations
- Binary Data Parsing
- Linux Network Stack

---

## Limitations

- Linux only (`AF_PACKET` is Linux-specific).
- Requires root privileges.
- Cannot decrypt HTTPS/TLS traffic.
- IPv6 parsing is not yet implemented.

---

## Future Improvements

- [ ] IPv6 packet parsing
- [ ] ARP packet decoding
- [ ] DNS packet parsing
- [ ] HTTP request parsing
- [ ] Packet filtering (`--tcp`, `--udp`, `--icmp`)
- [ ] PCAP export
- [ ] Live traffic dashboard
- [ ] Colorized terminal output

---

## What I Learned

Building WireGoldfish helped me understand:

- how packets are captured before applications receive them,
- Ethernet, IPv4, TCP, UDP, and ICMP header structures,
- binary parsing with `struct.unpack()`,
- bitwise operations for extracting protocol fields,
- Linux raw socket programming,
- and how tools like Wireshark decode packets layer by layer.

---

## Similar Tools

| Tool | Purpose |
|------|---------|
| Wireshark | Packet capture and analysis |
| Scapy | Packet manipulation library |
| tcpdump | Command-line packet capture |
| Suricata | Intrusion Detection System |
| Snort | Rule-based IDS |

---

## License

This project is licensed under the **MIT License**.
