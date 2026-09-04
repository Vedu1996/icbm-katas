# Architecture Decision Record (ADR) Template

## ADR [003]: Asymmetric Cryptography & Delegate Key Chains for Offline-Capable Ticketing

### Status
- PROPOSED

### Context
The Von Digitalis Estates system must validate ticket purchases at entrance gates and 40 ride turnstiles for up to 15,000 daily visitors. The estate suffers from intermittent and patchy Wi-Fi coverage across its grounds.  
Relying on synchronous network calls to a central database to authorize ticket scans creates single points of failure and causes severe gate entry bottlenecks during network drops. Additionally, tickets purchased at entrance POS counters during network outages must be issued, signed, and validated at turnstiles without contacting cloud infrastructure. 

### Decision
We will adopt Asymmetric Cryptography (Public/Private Key Infrastructure) to sign and verify all ticket credentials offline, supporting individual, gate-purchased, and family pass entitlements.

### Key Architectural Components

1. Daily Key Rotation & Distribution:
- Every 24 hours, the Cloud Auth Service generates a master daily key pair (Cloud_PrivKey, Cloud_PubKey).
- The active Cloud_PubKey is distributed to all park turnstiles and edge devices during daily off-peak maintenance syncs.  

2. Delegated Edge Signing (Offline Counter Sales):
- Physical entrance POS gateways are pre-provisioned with time-bound local private keys (Edge_PrivKey) stored on hardware-level Security Modules (HSM/TPM).
- During cloud network drops, local POS gateways sign tickets using Edge_PrivKey. Turnstiles verify these tickets offline using matching cached Edge_PubKey certificates.

3. Self-Contained Signed Ticket Payloads:
- Tickets are delivered as encrypted JSON Web Signatures (JWS) rendered inside dynamic QR codes or app payloads:
$$\text{Payload} = \text{Sign}_{\text{PrivKey}}(\text{ticket\_id}, \text{valid\_date}, \text{pass\_type}, \text{family\_id}, \text{sub\_id})$$
- Family Passes: Issued as master-delegated sub-tickets, assigning distinct signed sub_id tokens to individual family members to allow independent offline scanning across separate rides.

4. Zero-Latency Turnstile Verification:
- Turnstile optical readers scan the payload, execute asymmetric decryption using their locally cached public keys in <5ms, and grant entry without network dependencies.  
- Replay attacks are mitigated offline by appending valid ticket_ids to a local turnstile state cache, while asynchronously emitting TicketScanned events to edge store-and-forward queues for cloud sync.

### Alternatives Considered
#### Physical Smart Cards / NFC Wearables
##### Description
Issue encrypted hardware wristbands that store ticket permissions and balances directly on an embedded NFC chip.
##### Why it won't work
- Rejection Reason: High physical manufacturing and logistical overhead for issuing, recovering, and sanitizing 15,000 wristbands daily compared to near-zero cost digital/paper QR codes.

### References
[Finlo - Secure QR Ticket Generation and Encryption Methods](https://finlo.in/secure-qr-ticket-generation-and-encryption-methods.html)

### Date
2026-09-04