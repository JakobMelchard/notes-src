# introduction-to-networking (verified cards)

Cards kept after the 2026W check against the downloaded lecture material. Unverified cards and prose were removed.

## 01-foundations-and-governance

```card id=net-01-nodal-delay
What are the four components of nodal delay on one link?
---
$d_{\text{nodal}} = d_{\text{proc}} + d_{\text{queue}} + L/R + d/s$
$L/R$ is transmission (push bits onto the wire), $d/s$ is propagation (one bit crosses).
```

```card id=net-01-store-forward
Compute: a 1 kB packet crosses three 10 Mbit/s store-and-forward links of 100 km each, propagation $2\cdot10^8$ m/s, no queueing or processing.
---
Transmission $3 \cdot 8000/10^7 = 2.4$ ms, propagation $3 \cdot 0.5 = 1.5$ ms.
Total 3.9 ms.
```

```card id=net-01-traffic-intensity
What is traffic intensity and what happens to queueing delay as it approaches 1 and beyond?
---
$I = La/R$ with arrival rate $a$.
Near 0 the average queueing delay is small, near 1 it is large. Above 1 more work arrives than can be served and the average delay is unbounded; full queues drop packets.
```

```card id=net-01-switching
How does packet switching differ from circuit switching in resource allocation and guarantees?
---
Circuit: resources reserved end to end, fixed bandwidth and delay.
Packet: no reservation, store and forward, best effort; statistical multiplexing shares links.
```

```card id=net-01-ietf
How does the IETF make decisions, and who can join?
---
"Rough consensus and running code", without formal voting.
No formal membership; anyone can participate (volunteers, often employer-funded).
```

```card id=net-01-icann
When was ICANN formed and what changed in 2016?
---
1998, taking over the IANA functions (Jon Postel alone before) under a US Department of Commerce contract.
2016: transition to a multi-stakeholder model free of US oversight.
```

```card id=net-01-bcp14
What is BCP 14 and when do MUST, SHOULD, MAY carry normative meaning?
---
BCP 14 = RFC 2119 + RFC 8174.
They are normative only in capitals.
```

```card id=net-01-fwd-vs-routing
What is the difference between forwarding and routing in a network core?
---
Forwarding is a local action: a router moves an arriving packet from its input link to the right output link.
Routing is a global action: it determines the source-to-destination path, using routing algorithms.
```

```card id=net-01-packet-tradeoff
Why is packet switching not automatically better than circuit switching?
---
It suits bursty traffic (shared links, no call setup), but when arrivals exceed link capacity packets queue and can be lost.
Congestion therefore needs protocols for reliable transfer and congestion control.
```

```card id=net-01-loss
When does a router drop a packet, and who may retransmit it?
---
When the arrival rate to a link exceeds its transmission rate for some time and the output queue (buffer) is full, the arriving packet is dropped.
The previous node, the source end system, or nobody may retransmit it.
```

```card id=net-01-throughput
What limits the end-to-end throughput of a path?
---
The bottleneck link, the slowest link on the path; for a sender with rate Rs, receiver with rate Rc and a shared link R, the rate is min(Rs, Rc, R/10) in the lecture scenario.
```

```card id=net-01-traceroute
How does traceroute obtain a delay for each router on the path?
---
For each TTL value i it sends three probes that expire at router i; router i returns an error reply.
The sender measures the time between sending each probe and its reply.
```

```card id=net-01-stack
Name the five layers of the Internet protocol stack with one example protocol each.
---
Application (HTTP, SMTP, DNS), transport (UDP, TCP), network (IP, routing protocols), link (Ethernet, 802.11, PPP), physical (bits on the wire).
```

```card id=net-01-layering
Why is the network organised in layers, and what does encapsulation do?
---
Layers give an explicit structure and let one layer's implementation change without affecting the others.
Encapsulation wraps each layer's data in its own header (and trailer at link layer) as it goes down the stack; the receiver strips them in reverse.
```

```card id=net-01-protocol
What does a network protocol define?
---
The format and order of messages exchanged between entities, and the actions taken on sending and on receiving a message.
Protocols are needed because devices, unlike humans, need precisely specified rules.
```

```card id=net-01-internet-structure
Which kinds of networks make up the Internet, and how do they connect?
---
Access ISPs connect hosts, regional ISPs connect access networks to larger ISPs, and Tier-1 ISPs form the centre.
Networks interconnect via private peering or Internet Exchange Points; content provider networks often bypass Tier-1 and regional networks.
```

```card id=net-01-rfc-types
What are the main RFC types, and what is an RFC?
---
An RFC is a technical or organisational note about the Internet.
Types: BCP (best current practice), STD (standards track), FYI (informational).
```

```card id=net-01-registries
Which body allocates IP addresses at each level of the registry chain?
---
ICANN gives blocks to the five RIRs; RIRs assign to LIRs (ISPs, universities, large enterprises); LIRs assign to their customers.
```

```card id=net-01-rirs
Name the five RIRs and the one that serves Europe.
---
AfriNIC, APNIC, ARIN, LACNIC and RIPE NCC; RIPE NCC serves Europe, West Asia and the former USSR.
```

```card id=net-01-clock-recovery
Which line codes allow clock recovery, so that no separate clock wire is needed?
---
Manchester and RZ allow clock recovery; NRZ, NRZI and AMI do not.
Manchester gets it by a voltage transition in every bit interval, at the cost of at most 50 percent line efficiency.
```

```card id=net-01-block-codes
Why do block codes such as 4b/5b pair well with NRZ?
---
They map N data bits to M code bits and choose code words with no DC component (zero running digital sum), so the line stays DC free and the receiver can recover the clock.
```

```card id=net-01-baud
What is baud rate and what is its trade-off?
---
The rate at which the voltage level on the wire changes.
A higher baud rate carries more information per second but is more error prone and reaches shorter distances.
```

```card id=net-01-modulation
What do ASK, FSK and PSK vary, and what is each one's main property?
---
ASK varies amplitude: simple and low bandwidth, but prone to interference.
FSK varies frequency at constant amplitude: higher bandwidth possible. PSK varies phase at constant frequency and amplitude: robust, but harder to demodulate.
```

## 02-ethernet-and-arp

```card id=net-02-switch-learning
What does a learning switch do with a frame arriving on port $p$ from source $S$ to destination $D$?
---
Learn or refresh $S \to p$. If $D$ is in the table, forward to that port only (filter if it is $p$).
Otherwise flood to all ports except $p$. Broadcast and multicast are always flooded.
```

```card id=net-02-domains
What ends a collision domain and what ends a broadcast domain?
---
A switch port ends a collision domain (a hub extends it).
A router ends a broadcast domain (a switch extends it, a VLAN splits it).
```

```card id=net-02-vlan-ports
What is the difference between a VLAN access port and a trunk port?
---
Access: untagged, belongs to one VLAN.
Trunk: frames carry the 4-byte 802.1Q tag with the 12-bit VLAN ID, so several VLANs share one link.
```

```card id=net-02-arp-request
Which fields of an ARP request are filled in, and where is it sent?
---
All except THA (zeros); SHA/SPA are the requester's, TPA is the wanted IP.
Sent to Ethernet broadcast with EtherType 0x0806; the reply is unicast.
```

```card id=net-02-arp-gateway
A host 10.0.0.5/24 with gateway 10.0.0.1 sends to 10.0.0.77 and to 8.8.8.8. For which IP does it ARP in each case?
---
10.0.0.77 directly (same subnet), and 10.0.0.1 (gateway) for 8.8.8.8.
It never ARPs for 8.8.8.8.
```

```card id=net-02-csma-ca
Why does Wi-Fi use CSMA/CA with ACKs instead of CSMA/CD?
---
Collision detection is hard on wireless LANs: the local transmission overwhelms the weaker received signal.
So senders avoid collisions and confirm each frame with an ACK.
```

```card id=net-02-link-services
Is Ethernet reliable, and how does its MAC protocol work?
---
No. The receiving NIC sends no ACK or NACK, and there is no handshake; dropped frames are recovered only by a reliable higher layer such as TCP.
The MAC protocol is unslotted CSMA/CD with binary exponential backoff.
```

```card id=net-02-nic-accept
Which frames does a NIC pass up to the network layer?
---
Frames with its own unicast MAC, the broadcast address, or a matching multicast address (for example NDP traffic).
Any other frame is discarded.
```

```card id=net-02-frame-fields
Name the fields of an Ethernet II frame in order.
---
Preamble and start delimiter, destination MAC (6 bytes), source MAC (6 bytes), EtherType, payload, CRC.
The optional 802.1Q tag sits between the source MAC and the type field.
```

```card id=net-02-hub-switch
How does a hub differ from a switch?
---
A hub is a physical-layer repeater: it copies bits to all other ports and does not isolate collision domains.
A switch is a link-layer device: it stores frames, forwards them by destination MAC, and gives each port its own collision domain, so full duplex is possible.
```

```card id=net-02-mac-families
Name the three families of MAC protocols and one example of each.
---
Channel partitioning (TDMA, FDMA), random access (ALOHA, CSMA, CSMA/CD, CSMA/CA), and taking turns (polling, token passing).
```

```card id=net-02-aloha
What is the maximum efficiency of slotted ALOHA, and how does pure ALOHA compare?
---
About 1/e, roughly 37 percent of slots carry a successful frame with many busy nodes.
Pure (unslotted) ALOHA reaches about 18 percent, half of the slotted value.
```

```card id=net-02-backoff
After the m-th collision, how long does a CSMA/CD NIC wait before retrying?
---
It picks K at random from 0 to 2^m minus 1 and waits K times 512 bit times, then senses the channel again.
```

```card id=net-02-crc
What can a CRC detect, and what does a non-zero remainder mean?
---
A CRC with r check bits detects all burst errors of up to r bits.
The receiver divides the received bits by the generator; a non-zero remainder means an error was detected.
```

```card id=net-02-parity
What can single-bit parity and two-dimensional parity do?
---
Single-bit parity detects a single bit error.
Two-dimensional parity detects and corrects a single bit error.
```

```card id=net-02-arp-table
What does an ARP table hold and how do its entries expire?
---
IP address, MAC address and a TTL for each neighbour; the lecture gives about 20 minutes as typical.
Entries are soft state: they time out unless refreshed.
```

## 03-ipv4

```card id=net-03-ihl
In what unit is the IPv4 IHL field, and what are its typical and maximum values?
---
32-bit words. 5 = 20 bytes (no options), max 15 = 60 bytes.
```

```card id=net-03-subnet-decode
Compute: network, broadcast and usable hosts of 192.168.13.77/26.
---
Block size 64, $77 = 64 + 13$: network 192.168.13.64, broadcast 192.168.13.127.
Hosts .65 to .126 (62), mask 255.255.255.192.
```

```card id=net-03-hosts
How many usable hosts does a /n IPv4 prefix have, and what are the /31 and /32 cases?
---
$2^{32-n} - 2$ (network and broadcast excluded).
/31 has 2 (RFC 3021, point-to-point, neither reserved); /32 is one host.
```

```card id=net-03-ip-network
What makes a set of interfaces one IP network?
---
Hosts can reach each other without an intervening router, and their addresses share the same high-order prefix bits.
```

```card id=net-03-interface
Does an IP address belong to a host, a router or an interface?
---
To an interface. Hosts and routers may each have several interfaces, and each interface has its own address.
```

```card id=net-03-cidr-motivation
Why was classful addressing replaced by CIDR?
---
Class A blocks were too large and class C blocks too small, so class B blocks ran out.
CIDR gives a flexible prefix length a.b.c.d/x in place of fixed classes.
```

```card id=net-03-classful
Should classful A, B and C terms be used in modern operations?
---
No. Classful subnetting wastes address space and is deprecated; the lecture asks that it not be used or referred to in modern operations.
```

```card id=net-03-allocation
Which bodies hand out IPv4 addresses, and when did the pools run out?
---
ICANN gives blocks to the RIRs, RIRs assign to LIRs, and LIRs assign to their customers.
The ICANN pool has been empty since 31 January 2011; all RIRs are exhausted since 25 November 2019 but still allocate recovered blocks.
```

```card id=net-03-dhcp
What messages does DHCP exchange, and what does the server usually sit in?
---
Discover and offer (both optional), then request and ack; the host obtains a lease it can renew.
The DHCP server is typically co-located in the router and serves all subnets attached to it.
```

```card id=net-03-tos
What do the bits of the IPv4 type-of-service byte carry?
---
Bits 0 to 5 carry diffserv; bits 6 and 7 carry ECN.
```

## 04-ipv6

```card id=net-04-payload-length
Does the IPv6 Payload Length include the header?
---
No, it excludes the fixed 40-byte header (IPv4 Total Length includes it).
Payload Length 0 is a legal empty packet.
```

```card id=net-04-removed-fields
Name three IPv4 header fields removed in IPv6 and why.
---
Header checksum: redundant with L2 CRC and mandatory L4 checksum, costly per hop.
IHL and options: fixed header, extension headers instead. Fragmentation fields: routers never fragment.
```

```card id=net-04-fragmentation
What does an IPv6 router do with a packet too big for the next link?
---
Drops it and sends ICMPv6 Packet Too Big (type 2); routers never fragment.
So PMTUD is mandatory; minimum link MTU is 1280.
```

```card id=net-04-compress
Shorten 2001:0db8:0000:0000:0001:0000:0000:0001 per RFC 5952.
---
2001:db8::1:0:0:1
Two zero runs of equal length: the leftmost gets the ::.
```

```card id=net-04-solicited-node
Compute the solicited-node multicast address and its Ethernet MAC for 2001:db8::abcd:1234:5678.
---
ff02::1:ff34:5678 (ff02::1:ff00:0/104 plus the low 24 bits).
MAC 33:33:ff:34:56:78 (33:33 plus the low 32 bits of the group).
```

```card id=net-04-hop-limit
Why are NDP messages sent with Hop Limit 255?
---
A receiver MUST discard any with a lower hop limit; a router would have decremented it.
255 proves the packet was not forwarded: NDP's only off-link protection.
```

```card id=net-04-ndp-types
Give the ICMPv6 types of RS, RA, NS, NA and Redirect.
---
RS 133, RA 134, NS 135, NA 136, Redirect 137.
NS replaces the ARP request, NA the ARP reply.
```

```card id=net-04-dad
How does Duplicate Address Detection work?
---
Send an NS for your own tentative address from the unspecified source ::.
If an NA comes back, the address is taken.
```

```card id=net-04-default-router
True or false: a host using stateful DHCPv6 gets its default gateway from DHCPv6.
---
False. DHCPv6 cannot convey a default gateway.
It is the source link-local address of an RA with nonzero Router Lifetime.
```

```card id=net-04-eui64
How is a modified EUI-64 IID formed from MAC 02:00:00:00:00:01?
---
Insert ff:fe between OUI and device part, then invert the universal/local bit.
Result 0:ff:fe00:1. Deprecated for privacy.
```

```card id=net-04-ra-flags
What do the A flag and the M flag in a Router Advertisement cause?
---
A (on a prefix option): autoconfigure an address from that prefix (SLAAC).
M: use stateful DHCPv6 for addresses. Both can be set.
```

```card id=net-04-p2p-prefix
Which prefix length is recommended for inter-router point-to-point IPv6 links, and why?
---
/127 (RFC 6164).
A /64 there invites a neighbour-cache exhaustion attack.
```

```card id=net-04-double-colon-once
Why may the double colon appear only once in an IPv6 address?
---
With two, the address is ambiguous: you cannot tell how many zero groups each one stands for.
Leading zeros in each group may be dropped.
```

```card id=net-04-allocation
What are the typical IPv6 allocation sizes for an LIR and for a site?
---
An LIR typically receives a /29 to /32; a site typically receives a /48 to /56.
```
