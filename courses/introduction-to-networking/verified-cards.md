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

## recordings, 2026W

```card id=net-rec-01-pdu-names
What are the data units at layers 4, 3 and 2 called?
---
Layer 4 carries a segment, layer 3 a datagram, and layer 2 a frame, which encapsulates the datagram.
The application-level unit is a message.
```

```card id=net-rec-01-edge-core
What do the network edge and the network core consist of?
---
The edge holds the hosts (clients and servers, many in data centres) and the access networks over physical media.
The core is an interconnected mesh of routers, a network of networks.
```

```card id=net-rec-01-access-rates
What rates does the lecture give for HFC cable and DSL access?
---
HFC is asymmetric: up to 40 Mbit/s to 1.2 Gbit/s down and 30 to 100 Mbit/s up, shared by the homes on one cable segment to the headend.
DSL gives a dedicated line to the DSLAM: 24 to 52 Mbit/s down and 3.5 to 16 Mbit/s up.
```

```card id=net-rec-01-copper-fiber
What does the lecture say about twisted pair and fibre?
---
Category 5 twisted pair gives 100 Mbit/s or 1 Gbit/s, Category 6 gives 10 Gbit/s.
Fibre carries light pulses point to point at thousands of Gbit/s with a low error rate, is immune to electromagnetic noise, and has repeaters spaced far apart.
```

```card id=net-rec-01-wireless-link
Why does the lecture class a radio link as broadcast and half duplex?
---
The signal goes into the air with no physical wire, so every receiver in range hears the sender.
The link carries one direction at a time, from sender to receiver.
```

```card id=net-rec-01-wireless-reach
What reach does the lecture give for wireless LAN, cellular and geostationary satellite?
---
Wireless LAN covers tens of metres, wide-area cellular (4G and 5G) about 10 km, and a geostationary satellite link has about 270 ms end-to-end delay.
```

```card id=net-rec-01-tier1
How many tier-1 providers does the lecture count, and how do they connect?
---
Around 11 to 13, depending on the definition.
They peer with each other under contracts, and global connectivity depends on those peerings.
```

```card id=net-rec-01-table-size
How does a home router's forwarding table compare with a backbone router's?
---
A home router usually holds only a default route to its upstream provider.
A backbone router holds roughly a million prefixes covering the reachable Internet.
```

```card id=net-rec-01-attacks
Which three attack types on networks does the lecture cover?
---
Packet sniffing on shared media with a promiscuous interface, IP spoofing with a forged source address, and denial of service through floods of bogus traffic, often from botnets.
Spoofed requests can also be reflected and amplified towards a victim.
```

```card id=net-rec-01-defences
Which lines of defence against network attacks does the lecture list?
---
Authentication (SIM hardware in cellular networks, no equivalent in the classic Internet), encryption for confidentiality (HTTPS runs TLS), digital signatures for integrity, access restrictions such as password-protected VPNs, and firewalls, which are off by default and can filter flood traffic.
```

```card id=net-rec-02-history-early
Which early milestones does the lecture's history slide date?
---
ALOHAnet, a satellite network in Hawaii (1970), Ethernet at Xerox PARC (1976), Cerf and Kahn's internetworking principles (1974), and about 200 ARPANET nodes by 1979.
```

```card id=net-rec-02-history-80s
Which protocols and milestones does the history slide place in the 1980s?
---
SMTP (1982), TCP/IP deployment and DNS (both 1983), FTP (1985), and TCP congestion control (1988).
```

```card id=net-rec-02-icann-roles
What does ICANN manage, and why do regional registries exist?
---
ICANN manages global IP address and AS number allocation and the DNS root zone, and sets global rules.
A single central body does not scale, so the RIRs receive resources from ICANN and redistribute them in their regions.
```

```card id=net-rec-02-rir-policy
How are RIR policies decided at RIPE NCC, and who may take part?
---
Policy is made in public working groups, on mailing lists and at RIPE meetings.
Anyone may take part, not only RIPE NCC members.
```

```card id=net-rec-02-layer-agnostic
What does layer agnosticism mean?
---
Each layer works with whatever lies below and above it: Ethernet can run over Wi-Fi, copper or fibre, IPv4 or IPv6 can run over Ethernet, and UDP or TCP can run over IPv4 or IPv6.
```

```card id=net-rec-02-best-effort
Does the Internet guarantee that a sent packet arrives, and in the order sent?
---
No. Under the best-effort model a packet can be dropped, and packets can arrive out of the order in which they were sent.
```

```card id=net-rec-02-fdm-tdm
What are FDM and TDM in circuit switching?
---
FDM divides the frequency band (for example, colours of light on a fibre) into bands, one per call.
TDM divides time into periodic slots, one per call.
```

```card id=net-rec-02-traceroute-rtt
Why does traceroute measure a round-trip time and not a one-way delay?
---
The sender can only time the reply, because clocks on machines it does not control are not synchronised to the millisecond.
The return path may also differ from the outgoing path.
```

```card id=net-rec-02-traceroute-loss
Does loss at one intermediate traceroute hop mean that packets are lost there?
---
Not necessarily. A router under load may deprioritise answering probes over forwarding.
Loss matters when it persists to later hops or to the final destination.
```

```card id=net-rec-02-throughput-def
What is the difference between instantaneous and average throughput?
---
Instantaneous throughput is the rate at one point in time, and average throughput is the rate over a longer period.
```

```card id=net-rec-02-overprovision
Why can an access network sell many DSL lines that together exceed its uplink?
---
Subscribers rarely all use their line at full speed at the same time, so a shared uplink (for example 10 Gbit/s) is enough most of the time.
The lecture notes that cable is harder, because of interference.
```

```card id=net-rec-02-fiber-modes
How do single-mode and multimode fibre differ, according to the lecture?
---
Single-mode fibre suits long reach (the lecture cites a 10 km long-range optic at 1310 nm) but breaks if bent sharply.
Multimode fibre is more robust and suits shorter reach.
```

```card id=net-rec-02-l2-services
Which services does the link layer provide?
---
Framing and link access (MAC addresses in the frame header), channel access on shared media, reliable delivery between neighbours (seldom used on low-error links), flow control, and error detection, with error correction in some cases.
```

```card id=net-rec-02-nic
Where is the link layer implemented?
---
In the network adapter (NIC) or on a chip in the host.
It combines link and physical layer functions and attaches to the host bus.
```

```card id=net-rec-02-duplex
What is the difference between half duplex and full duplex?
---
Half duplex: both ends can transmit, but not at the same time.
Full duplex: both directions can carry data at full rate at once, and most modern wired links are full duplex.
```

```card id=net-rec-02-error-correction
Can error detection guarantee that a corrupted frame is caught, and how does correction differ?
---
No. A detection code can miss errors, and more check bits catch more of them.
Error correction goes further: the receiver computes what the bits should have been, so no retransmission is needed.
```

```card id=net-rec-02-checksum
How does the Internet checksum work, and what does a match prove?
---
The data is read as 16-bit words, summed in ones' complement, and the result is sent with the segment.
The receiver recomputes it: a mismatch proves an error, but a match does not prove there is none. UDP uses it.
```

```card id=net-rec-02-hash
Can a file hash such as SHA-1 repair a corrupted download?
---
No. It is an integrity check: it shows that something changed but cannot say which bits, so it is not redundancy.
```

```card id=net-rec-02-ideal-mac
What would an ideal multiple access protocol provide?
---
A lone sender gets the full channel rate, and M senders each get an average of R divided by M.
It is fully decentralised and needs no clock synchronisation or fixed slots.
```

```card id=net-rec-02-link-types
Which links does the lecture call point to point, and which broadcast?
---
Point to point: dial-up (PPP), and a link between a host and a switch.
Broadcast or shared: old-style Ethernet, upstream HFC, 802.11 Wi-Fi, and satellite or wireless links.
```

```card id=net-rec-02-tdma-fdma
What is the weakness of TDMA and FDMA at low load?
---
Each node owns a fixed slot or band even when idle, so unused slots or bands go empty.
A lone active node gets only one share, 1/N of the channel.
```

```card id=net-rec-02-csma
What does CSMA do before sending, and why do collisions still happen?
---
It listens: it sends when the channel is idle and defers when it is busy.
Collisions still happen because of propagation delay: a node may not yet hear a transmission that has started, so the whole frame is wasted.
```

```card id=net-rec-02-cd-jam
What does a CSMA/CD sender do after it detects a collision?
---
It aborts the frame and sends a jam signal so the other nodes know.
It then waits a random backoff that grows with each further collision.
```

```card id=net-rec-02-csma-cd-eff
What is the efficiency of CSMA/CD, and when does it approach 1?
---
$1/(1 + 5\,t_{prop}/t_{trans})$, where $t_{prop}$ is the maximum propagation delay and $t_{trans}$ the time to send a maximum-size frame.
It approaches 1 when propagation delay is small compared with frame time.
```

```card id=net-rec-02-taking-turns
What are the drawbacks of polling and token passing?
---
In polling, a primary node invites secondary nodes in turn: there is polling overhead, latency, and the primary is a single point of failure.
In token passing, passing the token costs overhead and latency, and a lost token stalls the ring.
```

```card id=net-rec-02-ethernet-speeds
Which speeds does Ethernet cover today?
---
From early 10 Mbit/s up to about 800 Gbit/s.
The 802.3 family shares one MAC protocol and frame format across these speeds and media.
```

```card id=net-rec-02-ethernet-topology
How has Ethernet's physical topology changed?
---
A bus, with all nodes in one collision domain, was popular until the mid 1990s.
Today a star around a switch is standard, and each link is its own collision domain.
```

```card id=net-rec-02-cable-reach
What is the reach of one copper Ethernet segment, and how does a hub change it?
---
About 100 m per segment. A hub adds a segment, so two segments reach about 200 m.
```

```card id=net-rec-02-payload
What payload range does an Ethernet frame carry, and what are jumbo frames?
---
Payloads run from 46 to 1500 bytes. Jumbo frames go up to about 9000 to 9200 bytes.
```

```card id=net-rec-02-ethertype
Which EtherType values does the lecture give for IPv4 and IPv6?
---
0x0800 for IPv4 and 0x86DD for IPv6.
Other values exist for protocols such as IPX and AppleTalk.
```

```card id=net-rec-02-fib-aging
How do switch table entries expire, and what happens when the table is full?
---
Each entry has a TTL that traffic refreshes, and expired entries are removed.
A full table cannot learn new addresses, so frames to unknown destinations are flooded.
```

```card id=net-rec-02-vlan-purpose
What does a VLAN do?
---
It splits one physical switch into several virtual switches, a logical division of broadcast domains.
It is set in software, so fewer physical switches and cables are needed.
```

```card id=net-rec-02-vlan-hopping
How can an attacker try VLAN hopping, and how is it mitigated?
---
A rogue switch can spoof VLAN negotiation, or a forged double VLAN tag can be sent.
Mitigations: disable the negotiation protocols, manage switches centrally, check that the outer tag matches the port, disable the default VLAN, and accept only native tags on trunks.
```

```card id=net-rec-02-standards-bodies
Which standards body covers Ethernet, and which covers IP?
---
The IEEE covers the 802 standards, such as 802.3 Ethernet and 802.11 Wi-Fi.
The IETF covers Internet protocols such as IP, documented in RFCs.
```

```card id=net-rec-03-ipv4-format
How is an IPv4 address written, and how many are there?
---
32 bits, written as four 8-bit octets in decimal, separated by dots.
That gives about four billion addresses.
```

```card id=net-rec-03-ipv6-format
How is an IPv6 address written?
---
128 bits as eight 16-bit groups in hexadecimal, separated by colons.
Leading zeros in a group may be left out, and one run of zero groups may be replaced by a double colon.
```

```card id=net-rec-03-prefix
What does the x in a.b.c.d/x give?
---
The prefix length: the number of leading network bits. It carries the same information as the netmask.
```

```card id=net-rec-03-sub-block
How might an ISP split 192.0.2.0/24 among customers?
---
Into /27 blocks: 192.0.2.0/27, 192.0.2.32/27 and 192.0.2.64/27, each with its first 27 bits fixed.
```

```card id=net-rec-03-classes
Which prefix lengths did classes A, B and C have, and what was class D for?
---
Class A /8, class B /16, class C /24. Class D was for multicast.
This is history only: classful addressing is deprecated.
```

```card id=net-rec-03-multicast
What is a multicast address for?
---
Addressing a group of hosts at once, for example one stream from a source reaching many receivers, as in TV streaming or conferencing.
```

```card id=net-rec-03-router-vs-switch
What does a router examine that a switch does not?
---
A router examines IP headers and chooses routes.
A switch works on link-layer frames and forwards them by destination MAC address.
```

```card id=net-rec-03-icmp
What is ICMP used for?
---
Control and error messages carried inside IP datagrams, for signalling between routers and hosts.
```

```card id=net-rec-03-ttl
What does the TTL field do, and what does the header checksum cover?
---
Each router decrements TTL and discards the datagram at zero, which stops packets looping forever.
The header checksum checks the integrity of the header.
```

```card id=net-rec-03-fragments
What are the identification, flags and fragment offset fields for?
---
Reassembling a datagram that was split into fragments, for example because it was too large for a link.
```

```card id=net-rec-03-ipv4-length
What total length does an IPv4 datagram typically have, and what is the maximum?
---
About 1500 bytes is typical. The total length field allows up to 64 KB, header included.
```

```card id=net-rec-03-arp-layer
Which layer does ARP belong to?
---
It is a link-layer protocol with its own EtherType, and it works on the interface without an IP address.
Lecture 1 treated it as lying between layers 2 and 3, and lecture 3 called it a layer 2 protocol.
```

```card id=net-rec-ex1-private-block
Which address range must the lab interface use, and with what prefix?
---
An IPv4 address in the 10.0.0.0/8 private block, on a /24 prefix.
```

```card id=net-rec-ex1-interface
What two steps bring a Linux interface into use for IPv4?
---
Bring the link up, and assign an IPv4 address to it. The ip command does both.
```

```card id=net-rec-ex1-ipv6-host
What does the host need to reach the lab VM?
---
IPv6 connectivity, because the VM is reached over SSH on an IPv6 address.
```

```card id=net-rec-ex1-arp
What does a successful ARP request return, and to whom?
---
The MAC address of the host that owns the IP address asked for, sent as a unicast reply to the requester.
```

```card id=net-rec-ex1-mac-format
How is a 48-bit MAC address written?
---
Six hex byte pairs separated by colons, for example 00:1a:2b:3c:4d:5e.
```
