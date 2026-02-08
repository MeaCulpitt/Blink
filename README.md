# Zap: Decentralized Edge Gaming

Zap is a Bittensor subnet that commoditizes low-latency compute for cloud gaming. By incentivizing a global network of residential and edge-based GPUs, Zap eliminates the latency floor inherent in centralized cloud gaming — bringing local-feel performance to players worldwide.

---

## The Problem

Cloud gaming is broken by physics. Players connect to distant data centers, introducing unavoidable lag that makes competitive gaming impossible. Even with fiber, the speed of light imposes a floor that software cannot fix.

**The result:** 60% of gamers want cloud gaming but cite latency as the primary blocker. Billions of users in underserved regions are excluded entirely.

**Zap fixes this** by moving the hardware to the network edge — physically closer to players. Instead of one data center serving a continent, thousands of residential GPUs serve their local neighborhoods.

---

## How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│                        ZAP ARCHITECTURE                          │
├─────────────────────────────────────────────────────────────────┤
│  MINERS          Host gaming hardware at the network edge        │
│                  RTX 40-series GPUs in residential locations     │
│                  Stream gameplay via WebRTC with <15ms latency   │
├─────────────────────────────────────────────────────────────────┤
│  VALIDATORS      Verify performance claims are real              │
│                  Speed-of-Light triangulation (no geo-spoofing)  │
│                  Input-to-Photon audits (real rendering, not relay)│
├─────────────────────────────────────────────────────────────────┤
│  PLAYERS         Connect to nearest Zap node                     │
│                  Play AAA games on any device                    │
│                  Experience local-feel latency                   │
└─────────────────────────────────────────────────────────────────┘
```

**Miners** earn TAO by providing high-fidelity, low-latency gaming sessions.
**Validators** verify that performance claims are backed by physical reality.
**Players** get cloud gaming that actually feels responsive.

---

## Key Features

### Edge-First Architecture
Hardware lives in neighborhoods, not distant data centers. Sub-15ms round-trip times to players.

### Physical Verification
Speed-of-Light triangulation prevents geographic spoofing. Input-to-Photon audits ensure real rendering, not pre-recorded streams.

### Hardware Incentives
Scarcity Bounties reward miners in underserved regions. The network grows where players need it most.

### Developer SDK
One-click integration for Unity and Unreal Engine. Developers can offer instant-play demos without downloads.

---

## Documentation

| Document | Description |
|----------|-------------|
| [Incentive & Mechanism Design](./docs/incentive_mechanism.md) | Emission logic, scoring formula, anti-gaming mechanisms |
| [Miner Architecture](./docs/miner.md) | Hardware requirements, streaming pipeline, proof of effort |
| [Validator Architecture](./docs/validator.md) | Verification methodology, scoring dimensions, consensus |
| [Business Logic & Market Rationale](./docs/market_rationale.md) | Problem statement, competitive landscape, sustainability |
| [Go-To-Market Strategy](./docs/GTM_strategy.md) | Target users, growth channels, early incentives |

---

## Scoring Formula

Miners are scored on three dimensions:

```
S = Q × L × A

Q = Quality (VMAF/SSIM visual fidelity)
L = Latency multiplier (sub-15ms = highest tier)
A = Availability (uptime + session completion rate)
```

| Metric | Threshold | Impact |
|--------|-----------|--------|
| Round-trip latency | <15ms | Maximum L multiplier |
| Encoding latency | <8ms | Required for scoring |
| Frame jitter | <2ms variance | Penalty if exceeded |
| VMAF score | >90 | Required for Tier 1 |

---

## For Miners

Earn TAO by:
1. Hosting gaming hardware (RTX 40-series or better)
2. Streaming gameplay to players via WebRTC
3. Maintaining sub-15ms latency and >90 VMAF quality
4. Achieving 99%+ uptime and session completion

**Scarcity Bounties:** 2.5x rewards for first miners in underserved regions.

---

## For Validators

Earn dividends by:
1. Verifying miner location via Speed-of-Light triangulation
2. Running Input-to-Photon audits (synthetic gameplay challenges)
3. Measuring visual fidelity and frame jitter
4. Reaching consensus with other validators

---

## For Developers

Integrate Zap to offer:
- Instant-play demos (no downloads)
- Cloud-native game distribution
- Global reach without infrastructure costs

**Zap SDK** for Unity and Unreal Engine coming soon.

---

## Links

- **Bittensor:** https://bittensor.com

---

## License

MIT

---
