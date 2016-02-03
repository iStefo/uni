# Advanced Computer Networks
## Multimedia
### Adaptive Playout Delay
* Compress/elongate silent periods
* `t_i`: timestamp of ith packet
* `r_i`: time packet i is received
* `p_i`: time packet i is played at receiver
* `r_i - t_i`: network delay for ith packet
* `d_i`: estimate of average network delay afer receiving ith packet
* `d_i = (1 - u)*d_{i-1} + u*(r_i - t_i)`
* `u`: fixed constant (e.g. 0.01)

### Forward Error Correction
* Simple scheme
	* for n chunks, send n+1 with one parity chunk
	* larger n: less bandwidth waste, longer playout delay, higher probablity that 2 chunks get lost
* Piggyback lower quality stream from packet before
* Interleaving
	* Packet contains small units from different chunks
	* Distribute loss of units
	* No redundancy, but increased playout delay

### RTP Header
* Payload type (7 bit)
	* 0: PCM mu-law, 3: GSM, 26: Motion JPEG, 33: MPEG2
* Sequence number (16 bit)
	* Increments by one for each RTP packet
* Timestamp field (32 bit)
	* Sampling instant of first byte in this packet
	* E.g. 8KHz Audio: increments each 125 usec (1/8000 sec)
* SSRC field (32 bit)
	* Source of RTP Stream

### Session Initiation Protocol
* Call Setup (Codec negotiation)
* Get real IP from callee ID
* Call management (add streams, cancel call)

## Quality of Service
### Measures
> A set of quality requirements on the collective behaviour of one or more objects.

* **Bandwidth** (maximum bit rate, e.g. Video Conferencing with 128 kbps)
	* **not** Network throughput (average rate of successful data transfer)
	* **not** Data Transfer (quantity of data transferred ofer given period of time)
	* Sustained rate: required bandwidth.
	* Bursts are allowed if less bandwidth has been used since the last burst
	* **Token Bucket algorithm**
* **Delay** (e.g. 150 ms for Video Conferencing)
	* Clumping and Dispersion of packets (Delay Jitter)
	* Dispersion poses problem in real-time applications
	* After dispersion, playout is delayed and delay jitter of subsequent packets should be measured with respect to dispersed packet: **Fixed Playout Delay** strategy
* **Delay variation**
* **Packet loss**
	* Average value of loss rate or probability
	* Sources: Violation of QoS requirement, Buffer overflow

#### End-to-end QoS
* Bandwidth: **Minimum** of all individual bandwidths
* Delay: **Sum** of individual node delays
* Loss: Upper bounded by the **sum** of individual loss probabilities
* Delay Jitter: related to the performance in a complex manner

Best Effort Traffic: All users obtain unspecified variable bit rate and delivery time, depending on the current traffic load.

### Elastic & Inelastic Applications
* Elastic: most existing applications
	* Tolerate delays and losses
	* Adapt to congestion
* Inelastic
	* Continuous media applications
	* Lower and upper limit on acceptable performance
	* Audio and Video, Telephones with high delay impair human interaction
	* Hard real-time applications (control applications)
	
## Traffic Management Techniques
* Packet classficiation
	* Scheduling
	* Traffic Policing and Shaping
* Connection admission Control

### Connection Admission Control (CAC)
* Connection request inclused QoS requirements
* If request is accepted, network allocates resources to ensure QoS delivered as long as source conforms to contract
* Constant-bit-rate (**CBR**) admission control: Allocate bandwidth needed
* Variable-bit-rate (**VBR**): Connections are bursty	* Peak-rate admission control: Reserve peak rate of bandwidth.
		* Simple, works well for small number. No Delay and loss
		* Wastes bandwidth, only few connections possible
	* Worst-case admission control: Allocate by average rate and burst size
		* Use less bandwidth than peak, can achieve end-to-end delay guarantee
		* Loss at peaks, implementation complexity
	* Admission control with statistical guarantees
		* When number of connection increases, probability that multiple sources send a burst decreases
		* Buffers to make probability of loss low
		* "OFF" and "ON" exponentially distributed with `1/α` and `1/β` mean
		* Probability of "ON": `α/(α+β)`
		* Link Capacity `Nα/(α+β) < C < N`
	* Measurement-based admission control
		* Measure real average, user tells peak load. If peak + average < capacity, admit.
		
## Traffic Policing and Shaping
**Traffic shaping**: Alter traffic characteristics of the connection to ensure negotiated bandwidth parameters.
**Traffic policing**: Monitor traffic for conformity with a traffic contract and drop traffic if required.

### Token Bucket Algorithm
Bucket receives one token every `1/r` seconds, bucket can hold at most `b` tokens, every time a packet arrives `N` token (e.g. number of bytes) are withdrawn. Without remaining tokens, pakcets get dropped.

**Sustained rate** is `r` tokens/second. Bucket is initialized full, i.e. with `b` tokens. Number of data units over any interval `t` is `B(t) = r*t + b`.

Variation: When bucket is empty, packages get queued instead of dismissed.

## Traffic Scheduling
Deciding service order & managing queue and deciding which packets should be transmitted or dropped.

Scheduling give different users different QoS to provide fairness among different users.

#### Max-min Fairness
Each flow receives no more than what it wants, the excess is equally shared.

#### Scheduling Disciplines
* First Come First Serve: Guarantees no fair order
* Round Robin: Process incomming queues with equal shares (leaves empty timeslots for empty queues)
* Priority Scheduling: Packet served only if no packets with higher priority exist (surge in high priority can cause low priority queue to saturate)
* Weighted Round Robin: Serve packet from each non-empty queue, multiplied according to weights (normalized to integer values)
* Weighted Fair Queuing (WFQ) Max-min: Flows get resource share according to normalized weight. Flows with unsatisfied demands obtain free resources in proportion to their weights (un-normalized)
* Generalized Processor Sharing: Own queue for each flow, serve in turns. But: not fair since packets have different sizes

## Network Design
* **P**lan
	* Identify requirements
	* Review existing network
* **D**esign
	* Design according to the initial requirements
	* Refined with the client
* **I**mplement
	* Network is built according to the approved design
* **O**perate
	* Operational and being monitored
* **O**ptimize
	* Issues are detected and corrected
	* Eventually redesign if too many problems
	
### Hierarchical Network Design
* Access layer: User and workgroup access
	* Local access (LAN, WiFi), Remote access (modem), Internet
	* Switches, Hubs, VPN
* Distribution layer: Organization policies and connect workgroups and core
	* Filtering, prioritizing and queueing traffic
	* Routing between access and core layers
	* Redundant connections, aggregate connections
* Core layer: High-speed transport between distribution layer devices
	* Low-latency backbone
	* Quick-converging routing protocoll to adapt to changes

### Switched Network DEsign
* Layer 2 Switch: MAC addresses
* Layer 3 Switch: Router with some functions implemented in hardware
* Virtual LAN: Broadcast domain regardless of physical locations

### IP Network Design
* How many IP Addresses are required
* Reservation for Future Need (10-20%)
* Private/Public Addresses and NAT
* Hierarchical IP
	* Saves space in routing tables
	* Less traffic for routing protocolls (RIP, IGRP, EIGRP, OSPF, IS-IS, BGP)
	* Core layer: fast converging routing protocol. EIGRP, OSPF, IS-IS
	* Distribution layer: RIPv1, RIPv2, IGRP, EIGRP, OSPF, IS-IS)
	* Access layer: less powerfull devices, RIPv3, IGRP, EIGRP, OSPF, static routes

### Security Design
* Vulnerability: Characteristics that allow someone to use the system in a suboptimal manner.
	* Design issues: inherent problem with functionality because of operating system, application or protocols
	* Human issue: administrator and user errors, such as unsecured user accounts or devices
* Reconnaissance Attacks:
	* Ping sweeping (discover network addresses)
	* Network & port scanning, enumeration (for network topology)
	* Stack fingerprinting (determine target OS and applications)
* Access Attacks:
	* Entry: access email
	* Collect: password
	* Plant: put back door
	* Occupy: control resource
* Information Disclosure Attacks:
	* Phishing
* Denial of Service Attacks
	* Render network or service worthless to users
	* E.g. SYN flood
#### Threat Defense
* Defending the edge
* Protecting the interior
* Guarding the end points
* Virus protection
* Traffic filtering
* Intrusion detection and prevention
* Content filtering
* Firewall
	* Public server located in the demilitarized zone (**DMZ**)
	* URL Filtering
	* E-mail Filtering
* Secure Communication (VPN, SSL, File encryption)

### Wireless Network Design
* Throughput inversely proportional to the distance between WAP and user
* Interference detection, Monitor RF usage
* Assisted site survey (propagation characteristics, shadowing)
* With overlapping channels, performance will suffer
* Is line-of-sight between antennas required?
* Federal or local regulations to be considered?

### Voice Transport Design
* Data over RTP over UDP
* Control packaets over TCP
* Traffic classification and policing to fulfil QoS requirements
* Separate regular data and voice services on VLAN basis
* Power over Ethernet to power phones from switch
* Call manager: component for call processing that provides enterprise telephony features, integrated voice applications and utilities (conferencing, monitoring)
* Voice Gateway: connects to Public Switched Telephone Network
	* H.323 protocol, Session Initiation Protocol
* Compressed Real-Time Transport Protocol **cRTP**
	* Voice samples are compressed
	* Headers are compressed from 40 byte to 24 byte
* Bandwidth Requirements: should be no more than 75% of available bandwidth on the link
* Capacity = Number of Calls * bandwidth per Call

### Network Availability
* Probability of availability at any point in time
* Availability = MeanTimeBetweenFailure / (MeanTimeBetweenFailure + MeanTimeToRepair)
	* 99%: 3d, 15h down per year
	* 99.9%: 8h down per year
	* 99.999%: 5min down per year ("high availability")
* Sources of failure:
	* User and process errors (change management and process issued): 40%
	* Software and applications (incl. load issues): 40%
	* Technology (incl. design, hardware, links): 20%
* Increasing avaialability:
	* Redundant links between devices
	* Redundant components within devices
	* Simple, logical design
	* Environmental conditions, spare parts
* AOT: Accumulated outage time since measurement started
* NAF: Number of accumulated failures since measurement started

## Network Management
Development, integration and coordination of hardware, software and human elements to monitor, test, poll, configure, analyze, evaluate and control the network and resources to meet the performance and Quality of Service required at reasonable cost.

* Monitor (Bandwidth, queue state at router, loss rate)
* Analyze (Identify source of congestion, loss)
* Reactive Control (Control the rate, admission control)
* Proactive Control (Expand link capacity, bandwidth reservation for different types of applications)

### Tools
* Detecting failure of an interface card
* Host monitoring (new applications, new users)
* Monitoring traffic to aid in resource deployment (new hardware should be deployed)
* Detect rapid changes in routing table (instabilities of routing)
* Intrusion detection

### Goals
* Performance management (quantify, measure, report, analyze and control performance)
* Fault management (log, detect and respond to fault conditions)
* Configuration management (track which devices are on the managed network)
* Accounting management (allow network manager to log and control user and device access, user quotas)
* Security management (control access according to well-defined policy)

### Infrastructure for network management
* **Managing entity** in centralized network management station, interface between human and network devices
* **Managed devices** is network equipment (incl. software) that resides on a managed network
* **Managed objects** actual pieces of hardware within the managed device (network cards)
* **Network management agent** is a process running in the managed device that communicates with the managing entity
* **Network management protocol** runs between managing entity and managed devices

### Network Management Standards
OSI CMIP (Common Management Information Protocol): too slowly standardized

SNMP (Simple Network Management Protocol): Internet roots (SGMP), started smimple, adopted rapidly, de facto standard

#### SNMP
* Management information base (**MIB**): distributed information store of network management data
* Structure of Management Information (**SMI**): data definition language, well-defined, unambiguous
* SNMP protocol (added in V3)