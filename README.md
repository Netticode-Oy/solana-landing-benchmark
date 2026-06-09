# Solana Transaction Landing Benchmark
 
> The first neutral, on-chain verifiable leaderboard for transaction landing latency across all major Solana providers.
 
**[→ Live Leaderboard](https://leaderboard.netticode.com)**
 
---
 
## Why this exists
 
Every Solana RPC and transaction relay service claims to be fast. Until now, there was no independent, verifiable way to compare them — no shared methodology, no neutral infrastructure, no on-chain proof.
 
This benchmark changes that.
 
We race identical transactions across all major providers in the same slot window, measure landing latency at shred resolution, and publish cumulative results anyone can verify on-chain.
 
---
 
## Live results
 
| # | Provider | Landing rate | p50 ms | p95 ms |
|---|----------|-------------|--------|--------|
| 1 | **Netticode RPC** | 100% | 139 ms | 480 ms |
| 2 | Solana mainnet-beta | 100% | 162 ms | 1055 ms |
| 3 | beta.helius-rpc.com | 100% | 248 ms | 800 ms |
| 4 | spectrum-02.simplystaking.xyz | 100% | 248 ms | 1334 ms |
| 5 | Helius | 100% | 379 ms | 3315 ms |
| 6 | mainnet.block-engine.jito.wtf | 100% | 473 ms | 1885 ms |
 
*EU West · updated continuously · [full leaderboard →](https://leaderboard.netticode.com)*
 
---
 
## Methodology
 
### 1 — Same transaction, raced
 
For each run we build identical transactions and fire them at every endpoint at the exact same instant. The only variable is how fast each provider gets the transaction onto the chain.
 
### 2 — Shred-resolution landing detection
 
Instead of polling an RPC with "did it land yet?", we tap the validator **Turbine** stream directly and timestamp the moment the *first shred carrying your transaction* arrives. That's the earliest observable sign a block producer included the tx — single-shred precision instead of the hundreds-of-milliseconds blur of polling.
 
### 3 — Location-neutral latency
 
From the raw send-to-observe time we subtract network transit on both ends:
- A TCP-connect handshake to the endpoint (outbound)
- The leader-to-our-node propagation time (inbound)
The reported milliseconds reflect the **provider's relay and inclusion speed**, not how far our measurement node sits from the endpoint. RPC processing time stays in the number — because that's the provider's job.
 
### 4 — Slot distance
 
Alongside milliseconds we report how many **slots after send** the transaction landed. Slots are integer and identical for everyone on the network — immune to clock skew and geographic effects. `+0` means same-slot inclusion, the gold standard for latency-sensitive strategies.
 
### 5 — On-chain confirmed
 
Every landing is cross-checked against the chain. Each transaction signature and landing slot is publicly verifiable on Solscan. We publish a sample of recent confirmed transactions directly on the leaderboard.
 
---
 
## Request a measurement
 
Anyone can benchmark their own RPC endpoint against the field.
 
1. Go to [leaderboard.netticode.com](https://leaderboard.netticode.com/#order)
2. Add your endpoint URL
3. Configure TPS and duration
4. Pay a small SOL fee (service fee + real transaction costs)
5. Results appear in your private dashboard and feed into the cumulative leaderboard
Current pricing: **0.001 SOL** service fee + network fees (launch promo).
 
---
 
## API
 
The leaderboard data is publicly available:
 
```
GET https://leaderboard.netticode.com/v1/api/leaderboard
```
 
```json
{
  "updated_ms": 1748985069670,
  "rows": [
    {
      "provider": "Netticode RPC",
      "region": "eu-west",
      "runs": 6,
      "sent": 618,
      "confirmed": 618,
      "landing_rate": 1.0,
      "p50_ms": 139,
      "p95_ms": 480,
      "p50_slot": 0,
      "mean_slot": 0.51,
      "rank": 1
    }
  ]
}
```
 
---
 
## Coverage
 
| Region | Status |
|--------|--------|
| EU West (Frankfurt) | ✅ Live |
| US East | 🔜 Coming soon |
| APAC | 🔜 Coming soon |
 
---
 
## License
 
Benchmark methodology and data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — you may use and cite this work with attribution to Netticode.
 
---
 
## About
 
Built by [Netticode](https://netticode.com) — high-performance software for Solana and fintech infrastructure. Based in Finland.
 
For inquiries: [info@netticode.com](mailto:info@netticode.com)
