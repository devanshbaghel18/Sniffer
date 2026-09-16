A Linux packet sniffer built in Python using raw sockets (AF_PACKET, SOCK_RAW) that captures live network traffic and manually parses packets layer by layer without using packet-analysis libraries like Scapy.

The project demonstrates how network packets travel through the Linux networking stack by decoding Ethernet, IPv4, TCP, UDP, and ICMP headers directly from raw bytes.

4
Features

Capture live Ethernet frames using raw sockets.

Parse source and destination MAC addresses.

Decode IPv4 packets.

Extract:

IP Version

Header Length

TTL

Protocol

Source IP

Destination IP

Parse ICMP packets.

Parse TCP segments.

Decode TCP flags:

URG

ACK

PSH

RST

SYN

FIN

Parse UDP segments.

Display packet payload as hexadecimal.

How It Works

The sniffer captures packets directly from the network interface using Linux's AF_PACKET socket family.

Each packet is decoded manually using Python's struct module.

The packet decoding pipeline follows the OSI/network stack:

Packet Parsing
Ethernet Frame

Extracts:

Destination MAC

Source MAC

EtherType

Example:

Ethernet Frame:
Destination: 01:00:5E:00:00:FB
Source: F2:9A:7C:85:4C:55
Protocol: 8
IPv4

Extracts:

Version

Header Length

TTL

Protocol

Source IP

Destination IP

TCP

Parses:

Source Port

Destination Port

Sequence Number

Acknowledgment Number

TCP Flags

UDP

Parses:

Source Port

Destination Port

Length

ICMP

Parses:

Type

Code

Checksum

Example Output
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

The packet above is an mDNS (Multicast DNS) packet commonly used for local network service discovery.

Installation
Requirements

Linux

Python 3

Root privileges

Clone the repository:

git clone https://github.com/yourusername/packet-sniffer.git
cd packet-sniffer

Run:

sudo python3 main.py

Raw sockets require administrator privileges on Linux.

Project Structure
packet-sniffer/
│
├── main.py
├── README.md
├── LICENSE
└── screenshots/
Technologies Used

Python 3

Linux Raw Sockets (AF_PACKET)

socket

struct

textwrap

Networking Concepts Demonstrated

Raw Socket Programming

Ethernet Frame Parsing

IPv4 Header Decoding

TCP Header Parsing

UDP Header Parsing

ICMP Packet Parsing

Bitwise Operations

Binary Data Parsing

Linux Network Stack

Limitations

Linux only (AF_PACKET is Linux-specific).

Requires root privileges.

Does not decrypt HTTPS/TLS traffic.

Currently supports IPv4 parsing (IPv6 decoding is not yet implemented).

Future Improvements
IPv6 packet parsing
ARP packet decoding
DNS packet parsing
HTTP request parsing
Packet filtering (--tcp, --udp, --icmp)
PCAP export
Live traffic statistics dashboard
Colorized terminal output
What I Learned

Building this project helped me understand:

how packets are captured before applications receive them,

how Ethernet, IPv4, TCP, UDP, and ICMP headers are structured,

binary parsing with struct.unpack,

bitwise operations for extracting protocol fields,

Linux raw socket programming,

and how packet analyzers like Wireshark decode traffic layer by layer.

License

This project is licensed under the MIT License.
