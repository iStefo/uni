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

### Forward Errr Correction
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
	* Worst-case admission control: Allocate by average rate and bust size
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