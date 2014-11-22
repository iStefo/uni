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