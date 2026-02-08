# Zap: Miner Architecture & Operations

The Zap miner is a real-time compute node that hosts game instances and streams high-fidelity video with sub-millisecond precision. Unlike batch-processing subnets, Zap miners operate synchronously — every frame must be rendered, encoded, and delivered within a 16.6ms window to maintain 60fps.

---

## Hardware Requirements

Zap is designed for consumer-grade gaming hardware, not industrial infrastructure.

### Minimum Specifications

| Component | Requirement | Notes |
|-----------|-------------|-------|
| GPU | RTX 4070 or better | NVENC encoder required |
| VRAM | 12GB+ | Higher = more concurrent sessions |
| CPU | 8-core, 3.5GHz+ | For game logic + orchestration |
| RAM | 32GB | Per concurrent session overhead |
| Network | 100Mbps symmetric | Low jitter connection critical |
| Storage | NVMe SSD, 500GB+ | Fast game loading |

### Recommended Specifications

| Component | Recommendation |
|-----------|----------------|
| GPU | RTX 4080 / 4090 |
| Network | 1Gbps symmetric, <5ms to ISP |
| Location | Residential, urban area |

Industrial GPUs (A100, H100) provide no advantage. Zap rewards **proximity and availability**, not raw compute power.

---

## The Miner Pipeline

Zap miners run four concurrent execution blocks:

```
┌─────────────────────────────────────────────────────────────────┐
│                      MINER PIPELINE                              │
├─────────────────────────────────────────────────────────────────┤
│  1. INSTANCE       Game binary in container                     │
│     EXECUTION      Process inputs at engine tick rate           │
│                    64-128Hz state updates                       │
├─────────────────────────────────────────────────────────────────┤
│  2. FRAME          GPU buffer capture (no CPU copy)             │
│     ORCHESTRATION  NVENC/AMF hardware encoding                  │
│                    H.264 or AV1, adaptive bitrate               │
├─────────────────────────────────────────────────────────────────┤
│  3. TRANSPORT      WebRTC peer-to-peer streaming                │
│                    STUN/TURN NAT traversal                      │
│                    <8ms encode-to-packet latency                │
├─────────────────────────────────────────────────────────────────┤
│  4. VERIFICATION   Cryptographic heartbeats (500ms)             │
│                    I2P challenge responses                      │
│                    Frame hash generation                        │
└─────────────────────────────────────────────────────────────────┘
```

### 1. Instance Execution

**Containerized Hosting:**
- Game binaries run in Docker with NVIDIA Container Toolkit
- Hardened, isolated environments prevent cross-session interference
- Fast cold-start (<10s) for on-demand session allocation

**Input Virtualization:**
- Incoming HID packets (keyboard, mouse, controller) converted to virtual system commands
- Target: <1ms input processing overhead
- Support for standard protocols: XInput, DirectInput, raw HID

**State Synchronization:**
- Game engine tick rate: 64-128Hz depending on title
- State updates processed synchronously with rendering
- No frame interpolation — real rendered frames only

### 2. Frame Orchestration

**GPU Buffer Capture:**
- Direct capture from GPU frame buffer
- Bypasses system memory to eliminate copy latency
- Uses NVIDIA Capture SDK or equivalent

**Hardware Encoding:**
- NVENC (NVIDIA) or AMF (AMD) required
- Supported codecs: H.264 (compatibility), AV1 (efficiency)
- Target: <4ms encoding latency per frame

**Adaptive Bitrate (ABR):**
- Real-time bitrate adjustment based on network conditions
- Range: 5-50 Mbps depending on resolution and motion
- Graceful degradation under congestion

### 3. Transport Layer

**WebRTC Streaming:**
- Peer-to-peer connections for minimum latency
- UDP-based RTP for video/audio delivery
- DTLS encryption for security

**NAT Traversal:**
- STUN for direct peer connection discovery
- TURN fallback for restrictive NAT configurations
- Target: >95% direct connection rate

**Latency Budget:**

| Stage | Target | Maximum |
|-------|--------|---------|
| Input processing | <1ms | 2ms |
| Game tick | ~8ms | 16ms |
| Frame capture | <1ms | 2ms |
| Encoding | <4ms | 8ms |
| Network transit | varies | — |
| **Total (excluding network)** | **<14ms** | **28ms** |

### 4. Verification

**Cryptographic Heartbeats:**
- Every 500ms, generate SHA-256 hash of randomized pixel blocks
- Proves active rendering (not idle or relaying)
- Validators can request proof of any historical heartbeat

**I2P Challenge Responses:**
- Validators send synthetic input sequences
- Miner must render correct visual response within 33ms
- Proves real-time rendering, not pre-recorded streams

**Frame Hash Metadata:**
- Each frame includes cryptographic hash
- Enables post-hoc verification of specific frames
- Prevents frame substitution attacks

---

## Input/Output Specification

### Input Streams

| Stream | Format | Source |
|--------|--------|--------|
| Control signals | Binary HID packets | Player client |
| ABR requests | JSON telemetry | Player client |
| Validator challenges | Signed input sequence + seed | Validator |

### Output Streams

| Stream | Format | Destination |
|--------|--------|-------------|
| Video | Encrypted RTP (H.264/AV1) | Player client |
| Audio | Encrypted RTP (Opus) | Player client |
| Frame metadata | SHA-256 hashes + timestamps | Validator |
| I2P responses | Rendered frame + timestamp | Validator |

---

## Scoring Dimensions

Miners are evaluated on three dimensions where **latency and reliability outweigh raw power**.

### Quality (Visual Fidelity)

| Metric | Target | Minimum |
|--------|--------|---------|
| VMAF | >95 | >90 |
| SSIM | >0.98 | >0.95 |

Quality is measured during synthetic audit sessions. The specific frames evaluated are unpredictable.

**Factors affecting quality:**
- Encoding bitrate and settings
- GPU rendering capability
- Network stability (packet loss degrades quality)

### Latency (Speed)

| Metric | Target | Maximum |
|--------|--------|---------|
| Round-trip time (RTT) | <15ms | <50ms |
| Encoding latency | <4ms | <8ms |
| Frame jitter | <1ms | <2ms |

**Latency multiplier tiers:**

| RTT | Multiplier |
|-----|------------|
| <15ms | 2.0x |
| 15-30ms | 1.5x |
| 30-50ms | 1.0x |
| 50-100ms | 0.5x |
| >100ms | 0.1x |

Geographic proximity is the primary determinant. Hardware cannot compensate for distance.

### Availability (Reliability)

| Metric | Target | Minimum |
|--------|--------|---------|
| Uptime | >99.9% | >99% |
| Session completion | >99% | >95% |
| Challenge response | 100% | >98% |

**Availability decay:**
- Missed pings reduce uptime score
- Dropped sessions reduce completion score
- Failed I2P challenges reduce response score

---

## Operational Considerations

### Thermal Management

Sustained 60fps rendering generates significant heat. Miners must:
- Ensure adequate cooling for continuous operation
- Monitor GPU temperatures (target: <80°C under load)
- Throttling causes frame drops and jitter penalties

### Network Stability

Residential connections vary in quality. Miners should:
- Use wired ethernet (not WiFi)
- Avoid shared bandwidth during peak hours
- Monitor for packet loss and jitter
- Consider dedicated connection if multi-session

### Concurrent Sessions

Higher-end hardware can serve multiple players:

| GPU | Concurrent Sessions |
|-----|---------------------|
| RTX 4070 | 1-2 |
| RTX 4080 | 2-3 |
| RTX 4090 | 3-4 |

Each session requires dedicated NVENC encoder channel. Overprovisioning causes quality degradation across all sessions.

### Game Library

Miners must have legal access to game binaries. Options:
- Steam library (personal ownership)
- Developer partnerships (SDK integration)
- Free-to-play titles (no licensing required)

Zap does not provide game licenses. Miners are responsible for compliance.

---

## Anti-Gaming Mechanisms

Miners cannot fake performance:

| Attack | Defense |
|--------|---------|
| Geographic spoofing | Speed-of-Light triangulation |
| Pre-recorded streams | I2P synthetic input challenges |
| Idle claiming | Cryptographic heartbeats |
| Quality inflation | Random frame sampling |
| Latency manipulation | Multi-validator RTT consensus |

Detected cheating results in:
1. Immediate score zeroing for the epoch
2. Stake slashing on repeated offenses
3. Potential network ban

---
