# Blink: Validator Design

The Blink Validator acts as a high-frequency auditor, ensuring that the network's "Edge" claims are backed by physical reality and high-performance hardware.

---

## 1. Scoring and Evaluation Methodology

Validators rank miners based on a composite score derived from three primary verification layers:

### A. Speed-of-Light (SoL) Distance Check (30%)
* **Mechanism:** Round-trip-time (RTT) triangulation.
* **Verification:** Validators use a global network of "watchtower" nodes to ensure a miner's response time matches the physical limits of fiber-optic travel from their claimed IP location.

### B. Input-to-Photon (I2P) Audit (40%)
* **Mechanism:** Synthetic User Simulation.
* **Verification:** The validator sends a sequence of game inputs and verifies that the miner's return video stream reflects these changes within a 33ms window (2-frame delay at 60fps). 

### C. Visual Fidelity & Jitter Analysis (30%)
* **Fidelity:** Automated sampling of frames to calculate VMAF (Video Multi-Method Assessment Fusion) scores.
* **Jitter:** Precise measurement of inter-frame arrival variance. Anything >2ms is penalized as "Stream Stutter."

---

## 2. Evaluation Cadence

Blink's validation operates on a tiered temporal structure:

* **Real-Time (10s):** Latency and heartbeat pings.
* **Random Audits (Monthly Hourly):** Synthetic gaming sessions to test I2P and frame hashes.
* **Blockchain Epoch (360 Blocks):** Weight commitments to the Subtensor.

---

## 3. Validator Incentive Alignment

Blink follows the standard Bittensor Yuma Consensus (YC) model with specialized gaming guardrails:

* **Consensus-Driven Rewards:** Validators earn $TAO$ dividends by correctly identifying high-performing miners. Deviating from the consensus (e.g., scoring a slow miner highly) results in a "Trust Penalty" and reduced emissions.
* **Anti-Collusion:** Because scoring is based on deterministic physical metrics (Latency/SSIM), it is difficult for a validator and miner to collude without being mathematically flagged as an outlier by other validators.
* **Stake-Weighted Authority:** The influence of a validator’s score is proportional to their total stake, ensuring that the most "heavily invested" actors have the strongest incentive to maintain network quality.
