# Lab Analysis: Exploiting Wireless & Hardware Vulnerabilities

**Course Track:** Cisco CyberOps Associate  
**Lab Reference:** 1.0.6 Case Study & Vulnerability Assessment  
**Source Material:** *"Top Hacker Shows Us How It's Done"* — Pablos Holman (TEDx)  
**Analyst:** DoucouPhantomCore.  

---

### Executive Summary
This analysis evaluates real-world vulnerabilities in common wireless and embedded systems discussed by Pablos Holman. Modern "secure" devices frequently trade security for convenience, transmitting sensitive identification data without adequate mutual authentication or physical shielding. This case study breaks down an RFID/NFC skimming attack, examines the attack path, and outlines blue-team defensive countermeasures.

---

### Vulnerability Breakdown

#### 1. Targeted Vulnerability
- **Flaw:** Unencrypted, unauthenticated data broadcast over 13.56 MHz High-Frequency Radio-Frequency Identification (RFID) / Near-Field Communication (NFC) protocols.
- **Root Cause:** Proximity cards and early contactless payment mechanisms were designed to broadcast their payload immediately upon receiving an interrogation signal from any active reader within range, without verifying if the reader is an authorized terminal.

#### 2. Compromised Data & Hacker Gain
- **Primary Data Exposed:** Primary Account Number (PAN), cardholder expiration dates, and unique card identifiers (UIDs).
- **Secondary Impact:** For access control badges, an attacker can capture raw facility codes and badge IDs, enabling unauthorized physical entry into secure zones (e.g., SOC facilities, server rooms).

#### 3. Attack Methodology
1. **Tooling:** An attacker uses a portable, high-gain reader/writer (e.g., Proxmark3, Flipper Zero, or a long-range custom RFID coil hidden in a backpack).
2. **Proximity:** The attacker stands in close physical proximity to the target in crowded public settings (elevators, transit, coffee shops).
3. **Interrogation & Capture:** The attacker's device transmits an electromagnetic pulse that powers the passive RFID transponder in the victim's wallet, prompting it to broadcast its token.
4. **Replay or Fraud:** The captured data is cloned onto a blank RFID tag or used directly for unauthorized transactions.

#### 4. Analytical Takeaway
What makes this attack critical from an investigative standpoint is its silent nature: there is zero physical intrusion, no system crash logs on standard endpoints, and no overt indicator of compromise (IoC) until an unauthorized transaction or physical breach occurs. As an analyst, it underscores why endpoint and physical perimeter security must be validated alongside network firewalls.

---

### Defensive Countermeasures & Mitigations

| Defense Layer | Mitigation Strategy | Implementation Details |
| :--- | :--- | :--- |
| **Cryptographic** | Dynamic CVV / Tokenization | Replace static PAN broadcasts with one-time EMV tokens and rolling cryptographic codes (as used in modern Apple Pay / Google Wallet). |
| **Mutual Authentication** | Challenge-Response Handshakes | Mandate that cards only transmit data after the reader proves ownership of a cryptographic shared key. |
| **Physical Security** | Faraday / RFID-Shielded Enclosures | Use signal-dampening sleeves or wallets lined with conductive metal mesh to block interrogation signals. |
| **Operational/Monitoring** | Anomalous Transaction Triaging | Implement SIEM-driven fraud detection to flag consecutive transactions occurring across physically impossible distances. |
