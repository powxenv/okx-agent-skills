# Market Analysis Reference

> Load this file when the user wants to analyze market conditions, evaluate a token, interpret on-chain data, or make trading decisions based on market signals.

## Table of Contents

1. Token Evaluation Framework
2. On-Chain Analysis Methods
3. Smart Money & Signal Interpretation
4. Technical Analysis with On-Chain Data
5. DeFi Protocol Analysis
6. Market Condition Indicators

---

## 1. Token Evaluation Framework

### 5-Pillar Token Assessment

Before investing in any token, evaluate all five pillars. A token should pass at least 4 of 5 before considering entry.

#### Pillar 1: Security

```bash
onchainos security token-scan --address <addr> --chain <chain>
```

| Factor | Green | Yellow | Red |
|---|---|---|---|
| `isHoneyPot` | `false` | — | `true` = BLOCK |
| `taxRate` | 0–3% | 3–10% | > 10% = WARN |
| `communityRecognized` | `true` | — | `false` = verify manually |
| DApp safety | Passes `dapp-scan` | — | Malicious = BLOCK |

#### Pillar 2: Liquidity

```bash
onchainos token liquidity --address <addr> --chain <chain>
onchainos token price-info --address <addr> --chain <chain>
```

| Total Liquidity | Assessment | Position Guidance |
|---|---|---|
| > $1M | Deep liquidity | Normal position sizing |
| $100K–$1M | Adequate | Standard sizing, monitor slippage |
| $10K–$100K | Shallow | Reduce position size 50%; widen stop-loss |
| $1K–$10K | Very thin | Only tiny speculative positions; high slippage expected |
| < $1K | Trapped | Cannot realistically exit; AVOID |

#### Pillar 3: Holder Distribution

```bash
onchainos token holders --address <addr> --chain <chain>
onchainos token cluster-overview --address <addr> --chain <chain>
onchainos token cluster-top-holders --address <addr> --chain <chain> --range-filter 1
```

| Top 10 Holding | Risk Level | Action |
|---|---|---|
| < 15% | Low concentration | Proceed normally |
| 15–30% | Moderate | Monitor, set tighter stops |
| 30–50% | High | Reduce position size, wider stops |
| > 50% | Very high | Consider avoiding; whale dump risk |

Also check:
- New wallet percentage (from `cluster-overview`): > 40% signals potential Sybil/bot activity
- Dev holding percentage (from `memepump token-dev-info` if meme token): > 10% is a red flag

#### Pillar 4: Smart Money & Signals

```bash
onchainos signal list --chain <chain> --token-address <addr>
onchainos tracker activities --tracker-type smart_money --chain <chain>
```

| Signal | Interpretation |
|---|---|
| Smart money accumulating | Bullish — consider entry on pullback |
| Smart money distributing | Bearish — consider exit or avoid |
| Multiple whale entries in 24h | Strong bullish signal — but validate with other pillars |
| KOL promoting but smart money not buying | Potential paid promotion — be cautious |
| High `soldRatioPercent` in signals | Whales exiting — bearish signal |

#### Pillar 5: Price & Market Data

```bash
onchainos market price --address <addr> --chain <chain>
onchainos token price-info --address <addr> --chain <chain>
onchainos market kline --address <addr> --chain <chain>
```

| Factor | Green | Yellow | Red |
|---|---|---|---|
| 24h change | Up moderately (1–10%) | Flat or slightly down | Down > 10% or up > 50% (unsustainable) |
| Volume trend | Rising volume confirms trend | Flat volume | Declining volume on price rise = divergence |
| Market cap | > $100M established | $10M–$100M growing | < $10M speculative |
| Price vs. major support | Above support | At support | Below support |

---

## 2. On-Chain Analysis Methods

### Accumulation vs. Distribution Detection

Use on-chain data to determine whether smart money is accumulating (buying quietly) or distributing (selling).

**Accumulation signals**:
- Exchange outflows increasing (whales moving to cold storage)
- On-chain transaction count rising while price is flat or down
- `onchainos tracker activities --tracker-type smart_money` showing net buy volume
- `onchainos signal list` showing increasing wallet count buying the token
- Holder count increasing while price stays range-bound

**Distribution signals**:
- Exchange inflows increasing (whales depositing to sell)
- Large transfers to exchange wallets
- Smart money activity showing net sell volume
- `soldRatioPercent` rising in signal data
- Top holders reducing positions (check `token holders` over time)

### Wallet Tracking Workflow

1. **Identify smart money**:
   ```bash
   onchainos leaderboard list --chain <chain> --time-frame 3 --sort-by 1
   # Find top PnL traders
   ```

2. **Monitor their activity**:
   ```bash
   onchainos tracker activities --tracker-type smart_money --chain <chain>
   # Filter by trade-type 1 (buy) or 2 (sell)
   ```

3. **Validate their picks**:
   ```bash
   onchainos security token-scan --address <token_addr> --chain <chain>
   onchainos token liquidity --address <token_addr> --chain <chain>
   ```

4. **Check if the trend is still early**:
   ```bash
   onchainos token price-info --address <token_addr> --chain <chain>
   onchainos market kline --address <token_addr> --chain <chain>
   ```

### Meme Token Quick Assessment

For quick go/no-go on meme tokens:

```bash
# 3-command triage:
onchainos security token-scan --address <addr> --chain <chain>
onchainos token liquidity --address <addr> --chain <chain>
onchainos memepump token-dev-info --address <addr>
```

| Check | PASS | FAIL |
|---|---|---|
| Honeypot | `false` | `true` → NO TRADE |
| Liquidity | > $10K | < $1K → AVOID |
| Dev rug count | 0 | > 0 → AVOID |
| Dev holding | < 5% | > 10% → AVOID |

If all 3 pass, proceed to full assessment with holders, cluster, and bundle analysis.

---

## 3. Smart Money & Signal Interpretation

### Understanding Signal Types

| Signal Type | Command | What It Shows |
|---|---|---|
| Tracker activities | `tracker activities --tracker-type smart_money` | Raw buy/sell transactions by classified wallets |
| Aggregated signals | `signal list --chain <chain>` | Tokens bought collectively by smart money/KOL/whales |
| Leaderboard | `leaderboard list` | Top traders by PnL, win rate, volume, ROI |

### Signal Classification

**Smart Money** (`--wallet-type 1`): Wallets classified by consistent profitability over multiple market cycles. Most reliable signal.

**KOL/Influencer** (`--wallet-type 2`): Known influencers. Can be early but sometimes promote bags they're selling. Validate independently.

**Whale** (`--wallet-type 3`): Large holders by volume. Their trades move markets. Watch for accumulation (bullish) and distribution (bearish).

### How to Read Signal Data

When you see a signal from `onchainos signal list`:

| Field | Meaning | Action |
|---|---|---|
| Multiple wallets buying the same token | Accumulation | Research the token (Pillar 1–5) |
| `soldRatioPercent` < 30% | Smart money still holding | Bullish — entry may be appropriate |
| `soldRatioPercent` > 70% | Smart money has sold | Late signal — may be too late to enter |
| Signal from 1 wallet only | Weak signal | Not enough confirmation; monitor, don't act yet |
| Signal from 5+ wallets | Strong signal | Higher conviction; proceed with research |
| Signal accompanied by volume spike | Strong confirmation | Confirms conviction; proceed with standard risk management |
| Signal with declining volume | Weak conviction | May be fading; proceed with caution |

### Signal Timing

- **Early signal** (within hours of smart money entry): Best entry window; price may not have moved yet
- **Mid signal** (1–3 days after): Price likely moving; enter on pullback with tight risk management
- **Late signal** (> 1 week after): Price may have already run; risk of being exit liquidity
- **Counter-signal** (smart money selling while retail buys): DANGER — likely exit liquidity for whales

---

## 4. Technical Analysis with On-Chain Data

### K-Line Interpretation

```bash
onchainos market kline --address <addr> --chain <chain> --bar <size> --limit <n>
```

| Bar Size | Best For | Typical Limit |
|---|---|---|
| `1m` / `5m` | Scalping, exact entry timing | 60–200 |
| `15m` / `30m` | Day trading, intraday | 100–200 |
| `1H` | Swing trading (1–7 days) | 100–200 |
| `4H` | Swing to position trading | 50–100 |
| `1D` | Trend identification | 30–90 |

**Field mapping** (always translate to human-readable):
- `ts` → Time, `o` → Open, `h` → High, `l` → Low, `c` → Close, `vol` → Volume
- Never show raw field names to users

### Supporting On-Chain Metrics

Combine price data with on-chain metrics for better analysis:

| Metric | Command | What It Tells You |
|---|---|---|
| Price + volume | `market kline` | Trend strength (volume confirms price) |
| Market cap + liquidity | `token price-info` | How much capital can be deployed without moving price |
| Holder distribution | `token holders` | Whether whales control supply |
| Top trader PnL | `token top-trader` | Whether profitable traders are still in or exiting |
| Recent trades | `token trades` | Current buying/selling pressure |
| Smart money activity | `tracker activities` | What profitable wallets are doing right now |

### Volume Analysis

- **Rising volume + rising price** = confirmed uptrend (healthy)
- **Rising volume + falling price** = confirmed downtrend (bearish)
- **Falling volume + rising price** = weakening trend (potential reversal)
- **Falling volume + falling price** = weakening selling (potential bottom)
- **Volume spike** = often marks a turning point (bottom or top)

---

## 5. DeFi Protocol Analysis

### Before Depositing into Any Protocol

```bash
# 1. Search for products
onchainos defi search --token <token> --chain <chain>

# 2. Check product details
onchainos defi detail --investment-id <id>

# 3. Verify APY sustainability
onchainos defi rate-chart --investment-id <id> --time-range MONTH

# 4. Check TVL trend
onchainos defi tvl-chart --investment-id <id> --time-range SEASON

# 5. For V3 pools: check depth distribution
onchainos defi depth-price-chart --investment-id <id>
```

### Protocol Risk Scoring

Rate each protocol on these factors before depositing:

| Factor | 1 (Low Risk) | 2 | 3 | 4 | 5 (High Risk) |
|---|---|---|---|---|---|
| Audit status | 3+ audits, established | 2 audits | 1 audit | Community audit only | No audits |
| TVL | > $1B | $100M–$1B | $10M–$100M | $1M–$10M | < $1M |
| Age | > 2 years | 1–2 years | 6–12 months | 3–6 months | < 3 months |
| Governance | Multi-sig + timelock | Multi-sig | DAO, active | DAO, new | EOA only |
| Team | Public, reputable | Public, new | Semi-public | Pseudonymous | Anonymous |
| APY | < 10% | 10–25% | 25–50% | 50–100% | > 100% |

**Scoring**:
- Total < 10: Blue-chip DeFi, lower risk
- Total 10–15: Established, moderate risk
- Total 16–20: Higher risk, smaller allocation
- Total > 20: Speculative, tiny allocation only

### APY Reality Check

**Advertised APY vs Real Yield**:

```
Real Yield = Advertised APY − Impermanent Loss − Gas Costs − Token Price Decline
```

| APY Level | Sustainability | Likely Composition |
|---|---|---|
| 2–8% | Sustainable | Pure fee revenue from established protocols |
| 8–20% | Often sustainable | Mix of fees and moderate incentives |
| 20–50% | Often unsustainable | Heavily reliant on token emissions |
| 50–100% | Rarely sustainable | Almost entirely emissions; dilution offsets yield |
| > 100% | Almost never sustainable | Temporary emission boost; rug/depower risk |

**APY trend check** using `onchainos defi rate-chart`:
- Stable or gradually declining → likely real yield
- Spiking sharply → likely emission-based, expect decay
- Wildly fluctuating → variable fee revenue, plan for worst case

---

## 6. Market Condition Indicators

### Market Regime Identification

| Indicator | Bull Market | Bear Market | Sideways/Volatile |
|---|---|---|---|
| BTC dominance | Rising or stable | Rising (flight to safety) | Fluctuating |
| Altcoin ratio | Outperforming BTC | Underperforming BTC | Mixed |
| Smart money | Accumulating alts | Rotating to stables | Diversifying |
| DEX volume | Increasing | Decreasing | Variable |
| DeFi TVL | Growing | Declining | Flat |
| Gas fees | Rising | Low | Moderate |

### On-Chain Health Check

Quick portfolio health assessment:

```bash
# Portfolio overview
onchainos market portfolio-overview --address <addr> --chain <chain>

# Recent PnL
onchainos market portfolio-recent-pnl --address <addr> --chain <chain>

# DeFi positions with health ratios
onchainos defi positions --address <addr> --chains <chains>
onchainos defi position-detail --address <addr> --chain <chain> --platform-id <pid>
```

**Key health metrics**:
- Health ratio > 2.0: Safe
- Health ratio 1.5–2.0: Monitor closely
- Health ratio < 1.5: Reduce leverage, add collateral, or repay debt
- Overall PnL positive: On track
- Overall PnL negative: Review positions, consider cutting losers

### When to Reduce Exposure

Trigger | Action
---|---
Portfolio down > 10% from peak | Reduce all positions by 25%; move to stables
Health ratio < 1.5 (DeFi) | Repay debt or add collateral immediately
BTC drops > 20% from ATH | Expect alts to drop 30–50%; tighten stops
Smart money net selling (`soldRatioPercent` > 70%) | Reduce alt exposure; move to stables
Exchange inflows spiking | Large holders depositing to sell; reduce risk
TVL declining across DeFi protocols | Capital leaving DeFi; reduce DeFi allocation |