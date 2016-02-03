# Netzsicherheit
## Security Goals
* Data Integrity
* Confidentiality
* Availability
* Authenticity
* Accountability
* Controlled Access

## Lanuage Security
### Problem 1: Input is controlled by attacker
Solution: Apply full recognition to inputs before processing.

#### Example: X.509 NULL Character issues
C code is used to stop sting processing on `\0` character.
May be used to fool string-compare functions implemented in C, used for cheching certificate validity.

Both parties may speak the same protocol, but have a different understanding of the data.

-----

### Problem 2: Udecidability
Solution: Use simpler grammars that have simpler parsers, so two parsers can be found equivalent or not.

##### Example: HTTP Grammar
Header can contain a Content-Length field of arbitrary length to indicate content of arbitrary length.
Chromsky Type 1 grammar.

-----

### Problem 3: Too much computing power
Solution: Use less computing power. Reduce complexity of problems to avoid *weird machines* that can be programmed by the attacker. Use context-free or regular grammaers/protocols.

##### Example: Find out visited pages in Browser
Browsers color visited and univisited links in different colors. Using JavaScript, one can find out the browser's state for a given page and thus find out the sites a user visited.

-----

### Problem 4: Mutual Understanding
Solution: Messages must be interpreted the same by all participants by using equivalent parsers. (only decidable for regular and context-free languages)

##### Example: Google query
Forge google request URL containing multiple search parameters. Google will search for the one, the other, or both combined, which is no predictable behaviour.


## TCP SYN-Flood Protection
### SYN Cookies
Cookies calculated using a one-way function α, the secret K and the source addr of the SYN PACKET S_syn: `α = h(K, S_syn)`.

##### Advantages:
- This number doesn't need to be stored but can be computed again and verified
- Works with the existing protocol, no change needed with the clients

##### Disadvantages:
- Calculation may be CPU consuming (it's actually not)
- TCP options can't be negitiated (use only during assumed attack)

## The 3 Security Components
- Requirements: Defined security goals
- Policy: Rules to implement the requirements
- Machenisms: Enforce the policy

## Firewalls
- Controlled Access at the network level
- *Incomming* and *outgoing* packets at each interface of the firewall
- Configured by a rule**list**

#### Configuration Strategies
- Whitelisting
	- Drop is default, accept others
	- More secure
	- **Best practice**
- Blacklisting
	- Accept is default, drop others
	- Less hassle with users

#### Statefull Filtering
- Matches on incoming interface, packets' l2-l4 fields and statefull parameters (connection tracking NEW/ESTABLISHED)
- "Allow established" rule will match most packets, put it first

#### Stateless Filtering
- More complicated to get right
- Can match on ACK flag ≈ not NEW (because not set for new connections)
	- But: attacker can send SYN/ACK as first packet (but hosts should ignore that)
	- UDP doesn't have ACK field -> Always set ACK to *

#### Shadowing
A case where all the packets one rule intends to deny have been accepted by preceding rules, or vice versa.

#### Bastion Hosts
- Hosts that are more exposed than the ones they intend to protect
- Kept simple, monitored and backuped
- No user accounts, only public key login

#### Firewall Architectures
- Simple Paket Filter Architecture (Router or rirewall with two interfaces)
- Dual-Homed Host Architecture (Host is part of two networks, is firewall and proxy)
- Screened Host Architecture (Packet filter protects network and bastion host, bastion host may be accessible from the Internet)
- Screened Subnet Architecture (Demilitarized Zone, requires two firewalls)

## Cryptography
#### Kerckhoff's Principle
The ciphier method must not be required to be secret, and it must be able to fall into the hands of the enemy without inconvenience.

#### Encryption Modes
- ECB: Electronic Code Book Mode
	- Idependently encrypt all input blocks
	- Identical plaintext blocks are encrypted to identical ciphertext
- CBC: Cipher Block Chaining Mode
	- XOR output ov previous round/IV to input before encrypting
- OFB: Output Feedback Mode
	- Transforms a block cipher to stream cipher
	- XORs the key stream to the plaintext
- CTR: Counter Mode
	- Encrypt incrementing counter (starting by IV) and XOR to plaintext

### Message Authentication Code
- Mac-then-Encrypt: `Enc(m, Mac(m))`
	- Used in: SSL
	- No ciphertext integrity
- Mac-and-Encrypt: `Enc(m), Mac(m)`
	- Used in: SSH
	- No ciphertext integrity, may leak plaintext information if Mac doesn't include a counter
	- **Considered the weakest**
- Encrypt-than-Mac: `Enc(m), Mac(Enc(m))`
	- Used in: IPSec, Signal, maybe TLS 1.3
	- **Considered the most secure**

### Hash Functions
#### Properties
* 1st pre-image resistance:
	* for a given `y` computationally infeasible to find a `x` s.t. `H(x) = y`
* 2nd pre-image resistance:
	* for a given `x` computationally infeasible to find a `x'` s.t. `H(x) = H(x')`
* Collision resistance:
	* infeasible to find `x, x'` s.t. `H(x) = H(x')`
* Random oracle property:
	* computationally infeasible to distinguish `H(m)` from random n-bit
