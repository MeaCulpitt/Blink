# Zap: Incentive & Mechanism Design

Zap’s incentive structure is designed to commoditize low-latency compute by rewarding miners who provide high-fidelity gaming sessions at the physical "Edge" of the network.

---

## 1. Emission and Reward Logic

Zap utilizes the Bittensor dTAO emission schedule (2026 standard):

* **Incentive Distribution:** 41% to Miners, 41% to Validators, and 18% to the Subnet Owner.
* **Scoring Formula:** $S = (Q \times L) \times A$
    * **Quality ($Q$):** Measured via VMAF and SSIM benchmarks.
    * **Latency ($L$):** Multiplier favoring sub-15ms round-trip times.
    * **Availability ($A$):** Uptime and session completion rate.

---

## 2. Incentive Alignment

### For Miners
Miners are incentivized to optimize for **Geographic Proximity**. Higher rewards are allocated to nodes in high-demand, low-supply regions (Scarcity Bounties). Miners utilize consumer-grade GPUs (RTX 40-series) to minimize capital expenditure while maximizing encoding efficiency.

### For Validators
Validators are incentivized via **Consensus Accuracy**. Under Yuma Consensus, validators maximize dividends by reaching agreement on miner performance. Providing outlier scores (collusion) leads to a reduction in "Consensus Trust" and a corresponding drop in $TAO$ emissions.

---

## 3. Mechanisms to Discourage Adversarial Behavior

* **Speed-of-Light (SoL) Checks:** Triangulation of miner IP addresses to prevent geographic spoofing.
* **Input-to-Photon (I2P) Audits:** Non-deterministic movement challenges that prevent miners from re-streaming pre-recorded content.
* **Jitter Penalties:** Mathematical penalties for inconsistent frame delivery ($>2ms$ variance), ensuring miners do not over-provision their hardware.

---

## 4. Proof of Intelligence & Effort

Zap qualifies as a **Proof of Useful Effort (PoUE)** subnet:
* **Effort:** Miners perform real-time, high-wattage GPU rendering and encoding that is immediately verified by validators.
* **Intelligence:** The "Intelligence" is found in the **Dynamic Orchestration Layer**—miners must intelligently manage resource allocation, network congestion, and thermal throttling to maintain a stable 60 FPS stream.

---

## 5. High-Level Algorithm

1. **Assignment:** Validators issue synthetic "Game Challenges" (input sequences) to miners.
2. **Submission:** Miners render the scene and stream the output back with cryptographic frame-hashes.
3. **Validation:** Validators check for "Input Coherence" (visual response within 33ms) and visual fidelity.
4. **Scoring:** Validators aggregate metrics into a 2D weight matrix.
5. **Reward Allocation:** Weights are committed to the Subtensor, and emissions are distributed based on the stake-weighted median of all validator scores.
