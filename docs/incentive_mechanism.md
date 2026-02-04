# Blink: Incentive & Mechanism Design

Blink is a decentralized physical infrastructure (DePIN) subnet on Bittensor designed to provide ultra-low latency cloud gaming. The incentive mechanism is engineered to reward the delivery of high-fidelity compute within strict geographic and temporal constraints.

---

## 1. Emission and Reward Logic

Blink utilizes a **Performance-Adjusted Availability** model. Unlike AI subnets that prioritize raw throughput, Blink prioritizes **Synchronous Reliability**.

* **Emission Split:** Following the Bittensor standard, emissions are distributed as:
    * **41% to Miners:** Rewarding the provision of low-latency GPU compute.
    * **41% to Validators:** Rewarding accurate auditing and quality control.
    * **18% to the Subnet Owner:** Directed to the Blink Treasury for infrastructure and ecosystem growth.
* **The Weighting Formula:**
    A miner's score ($S$) is calculated as:  
    $$S = (Q \times L) \times U$$  
    * **$Q$ (Quality):** Visual fidelity measured via SSIM (Structural Similarity Index) and frame-rate consistency.
    * **$L$ (Latency Multiplier):** A decaying multiplier based on Round-Trip Time (RTT). Ping under 20ms yields a $1.0$ multiplier, while ping over 100ms decays the multiplier to $0.0$.
    * **$U$ (Uptime):** A binary or fractional multiplier based on session completion rates.
* **Regional Scarcity Bonus:** To ensure global coverage, miners in high-demand/low-supply geographic zones receive a scarcity multiplier ($1.5x$ or $2.0x$) to encourage the expansion of the "Edge."

## 2. Incentive Alignment

### For Miners
Miners are incentivized to optimize for the **"Last Mile."** Higher rewards are granted to those who place hardware in residential areas with high-speed fiber rather than centralized data centers. Because Blink favors consumer-grade GPUs (RTX 4080/4090) for their specialized low-latency encoding (NVENC), miners can achieve high rank without enterprise-level capital expenditures.

### For Validators
Validators are incentivized to be the network's **"Quality Police."** Through Yuma Consensus, validators earn dividends by reaching agreement on miner performance. If a validator attempts to inflate the score of a low-performing miner, their **Consensus Weight** will drop as other validators report lower scores, leading to a reduction in the dishonest validator's $TAO$ yield.

## 3. Mechanisms to Discourage Adversarial Behavior

To maintain network integrity, Blink implements the following guardrails:

* **Speed-of-Light Distance Checks:** Validators use a global mesh of watchtower nodes to triangulate a miner's physical location. If the reported latency contradicts the laws of physics for the miner's claimed IP location, the miner is immediately deregistered.
* **Frame-Hash Watermarking:** Miners must submit periodic cryptographic hashes of the frames they are rendering. Validators verify these hashes against random "Spot-Check" renders to ensure the miner is not re-streaming content or spoofing work.
* **Collusion Resistance:** The scoring algorithm requires a high degree of consensus among the top 64 validators (by stake) to finalize a miner's reward, making it economically unfeasible to bribe a majority of the audit force.

## 4. Proof of Effort & Intelligence

Blink qualifies as a **Proof of Useful Effort** (PoUE) subnet. 

* **Proof of Effort:** Unlike static hashing, Blink requires miners to maintain a high-wattage, active hardware state. Rendering 60+ frames per second and encoding them in real-time is a continuous, verifiable expenditure of physical resources.
* **Intelligence:** The "intelligence" is embedded in the **Dynamic Orchestration**. Miners must run sophisticated resource-management layers to handle concurrent user inputs, manage thermal throttling, and adapt bitrates to fluctuating network conditions without dropping frames.

## 5. High-Level Algorithm

The Blink lifecycle follows a continuous loop of request, execution, and audit:

1.  **Task Assignment:** A user requests a session through a Blink Gateway. The Gateway identifies the top 5 miners with the lowest latency to that user's IP.
2.  **Submission:** The selected miner initiates a WebRTC tunnel, streaming the game output while simultaneously submitting metadata (frame hashes and input-to-render timestamps) to the validator.
3.  **Validation:** Validators join sessions as "Passive Observers" to measure RTT and jitter. Separately, they send "Synthetic Challenges" (requesting a specific complex scene be rendered) to verify GPU capabilities.
4.  **Scoring:** Validators aggregate performance data over a 360-block epoch. Scores are normalized against the subnet's performance baseline.
5.  **Reward Allocation:** Final weights are committed to the Subtensor blockchain. Emissions flow to miners and validators based on their stake-weighted contribution to the network's quality.
