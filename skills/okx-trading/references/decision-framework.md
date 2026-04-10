# Decision Framework Reference

> Load this file when the user asks "should I buy?", "is this a good trade?", "what should I do?", or needs help making a trading decision.

## Table of Contents

1. Trade Decision Flowchart
2. Pre-Trade Checklist
3. Token Quality Scorecard
4. Entry Decision Matrix
5. Exit Decision Matrix
6. DeFi Deposit Decision Matrix
7. Common Mistakes to Avoid

---

## 1. Trade Decision Flowchart

When considering any trade, follow this decision tree in order:

```
START: User wants to make a trade
│
├─ Step 1: SECURITY CHECK
│  └─ onchainos security token-scan
│     ├─ isHoneyPot=true → STOP. Do not buy.
│     ├─ action="block" → STOP. Do not proceed.
│     ├─ action="warn" → WARN user, require explicit confirmation.
│     └─ action=null → PASS. Continue to Step 2.
│
├─ Step 2: LIQUIDITY CHECK
│  └─ onchainos token liquidity
│     ├─ <$1K → STOP. Cannot realistically exit.
│     ├─ $1K–$10K → WARN. Very high slippage risk. Tiny position only.
│     ├─ $10K–$100K → CAUTION. Monitor slippage on quote.
│     └─ >$100K → PASS. Normal trading.
│
├─ Step 3: HOLDER CONCENTRATION
│  └─ onchainos token holders + cluster-overview
│     ├─ Top 10 > 50% → HIGH RISK. Whale dump risk.
│     ├─ Top 10 30–50% → MEDIUM RISK. Reduce position size.
│     └─ Top 10 < 30% → PASS. Healthy distribution.
│
├─ Step 4: MARKET SIGNALS
│  └─ onchainos signal list + market kline
│     ├─ Smart money selling (soldRatio > 70%) → BEARISH. Avoid or reduce.
│     ├─ No signal → NEUTRAL. Proceed with sizing.
│     └─ Smart money buying (soldRatio < 30%) → BULLISH. Favorable entry.
│
├─ Step 5: POSITION SIZING
│  └─ Apply risk framework (references/risk-management.md)
│     ├─ Calculate max position based on 1–2% risk rule
│     ├─ Verify total portfolio heat ≤ 6%
│     ├─ Set stop-loss level
│     └─ Confirm risk/reward ≥ 1:2
│
├─ Step 6: QUOTE & EXECUTE
│  └─ onchainos swap quote → confirm with user → onchainos swap execute
│     ├─ Price impact > 5% → WARN before confirming
│     ├─ Enable MEV protection if ≥ threshold
│     └─ Use appropriate slippage preset
│
└─ Step 7: POST-TRADE
   └─ Verify tx, check balance, set monitoring
```

---

## 2. Pre-Trade Checklist

Run through EVERY item before executing a trade. No exceptions.

### Essential Checklist (Required for All Trades)

- [ ] **Security**: `token-scan` returns no `block` action, no honeypot
- [ ] **Liquidity**: Pool depth sufficient for position size (slippage < 2%)
- [ ] **Risk budget**: This trade risks ≤ 2% of portfolio (≤ 0.5% for meme tokens)
- [ ] **Portfolio heat**: Total open risk ≤ 6% after this trade
- [ ] **Stop-loss**: Defined BEFORE entry (price level or percentage)
- [ ] **Risk/reward**: Minimum 1:2 ratio confirmed
- [ ] **Position size**: Calculated per sizing rules, not emotion
- [ ] **Quote checked**: `swap quote` run and reviewed for price impact and fees
- [ ] **MEV assessed**: Enabled for trades meeting value threshold

### Enhanced Checklist (For Meme/Speculative Tokens)

- [ ] **Dev check**: `memepump token-dev-info` → no rug history, dev holding < 5%
- [ ] **Bundle check**: `memepump token-bundle-info` → low bundle and sniper activity
- [ ] **Holder distribution**: Top holders < 30%, new wallet % < 20%
- [ ] **Security advanced**: `token advanced-info` → no red flags in risk level
- [ ] **Position limit**: Max 0.25% risk, position capped at speculative allocation

### DeFi Checklist (For Yield Products)

- [ ] **APY sustainability**: `defi rate-chart` shows stable/declining APY, not spiking
- [ ] **TVL stability**: `defi tvl-chart` shows stable or growing TVL
- [ ] **Protocol audit**: Verified 2+ independent audits
- [ ] **IL estimate**: Calculated and included in real yield assessment
- [ ] **Balance check**: Verified sufficient balance before depositing
- [ ] **APY warning**: If APY > 50%, explicit user acknowledgment required

---

## 3. Token Quality Scorecard

Rate each token 1–5 on these dimensions. A score of 20+ (out of 35) suggests a reasonable investment.

| Dimension | 1 (Poor) | 2 (Below Avg) | 3 (Average) | 4 (Good) | 5 (Excellent) |
|---|---|---|---|---|---|
| **Security** | Honeypot or block action | High tax >10%, no recognition | Some concerns, tax 3–10% | Low tax <3%, community recognized | Fully audited, blue-chip |
| **Liquidity** | <$1K | $1K–$10K | $10K–$100K | $100K–$1M | >$1M |
| **Distribution** | Top 10 >70% | Top 10 50–70% | Top 10 30–50% | Top 10 15–30% | Top 10 <15% |
| **Smart Money** | Whales distributing, soldRatio >70% | Mixed signals | No signal (neutral) | Some smart money buying | Multiple smart money wallets buying, soldRatio <30% |
| **Momentum** | -20% or worse 24h | -5% to -20% 24h | Flat ±5% | +5% to +20% 24h | +5% to +20% with rising volume |
| **Fundamentals** | No use case, anonymous team | Speculative narrative | Some utility, early adoption | Clear use case, growing adoption | Established product, strong team |
| **Market Cap** | <$1M | $1M–$10M | $10M–$100M | $100M–$1B | >$1B |

**Score interpretation**:
- 28–35: Strong candidate, standard position sizing
- 20–27: Decent candidate, reduce position size by 25–50%
- 12–19: Risky, only tiny speculative allocation
- <12: AVOID

---

## 4. Entry Decision Matrix

Use this matrix when deciding whether to enter a trade.

### By Market Regime

| Market | Blue-Chip Tokens | Mid-Cap | Meme/Speculative | Stablecoin Yield |
|---|---|---|---|---|
| **Bull** | Accumulate, DCA | Selective momentum | Very selective, small size | Reduce |
| **Bear** | DCA slowly, hold quality | Reduce, move to stables | Exit entirely | Increase allocation |
| **Sideways** | Hold, range trade | Selective DeFi yield | Avoid | Increase allocation |
| **High volatility** | Tighter stops, smaller size | Reduce position size | Avoid | Hold in stablecoin yield |

### By Signal Strength

| Signal Strength | What You See | Action |
|---|---|---|
| Strong (5+ smart money wallets buying, volume confirming, trend intact) | Multiple confirmed signals | Standard position size (1–2% risk) |
| Medium (2–4 smart money wallets, mixed volume) | Some confirmation | Reduced position (0.5–1% risk) |
| Weak (1 wallet, no volume confirmation) | Unconfirmed | Monitor, don't enter yet |
| Counter (smart money selling, smart money moved to stables) | Distribution signals | Avoid or short |

### By Conviction Level

| Conviction | Risk/Trade | Sizing | Examples |
|---|---|---|---|
| A+ (highest) | 1.5–2% | Full position | Blue-chip with multi-signal confirmation |
| A | 1% | Standard position | Strong setup with good risk/reward |
| B | 0.5% | Reduced position | Decent setup with some uncertainty |
| C (experimental) | 0.25% | Tiny position | Testing new strategy; accept possible total loss |

---

## 5. Exit Decision Matrix

### When to Take Profit

| Scenario | Action |
|---|---|
| Hit 1R target | Sell 30–50% of position, move stop to breakeven |
| Hit 2R target | Sell additional 30%, tighten stop |
| Hit 3R+ target | Trail stop at 0.75× ATR; let winner run |
| Sudden 50%+ spike | Take partial profit (at least 50%); never let a big winner turn into a loser |
| Smart money starts selling (soldRatio > 50%) | Take profit or tighten stop significantly |

### When to Cut Losses

| Scenario | Action |
|---|---|
| Hit stop-loss | Exit immediately, no exceptions |
| Thesis invalidated (fundamental change) | Exit regardless of price |
| Security breach discovered | Exit immediately via `security token-scan` |
| Liquidity drying up (volume down 80%+) | Reduce position, set tighter stops |
| Smart money exited completely | Consider exit; at minimum, reduce by 50% |

### When to Add to a Position

| Scenario | Action |
|---|---|
| Price pulled back to support, original thesis intact | Scale in 30–40% of target additional size |
| Original entry was a scale-in strategy, first partial profitable | Add second scale-in at predetermined level |
| Never add to a losing position to "average down" | Averaging down increases risk; use stop-loss instead |

---

## 6. DeFi Deposit Decision Matrix

### Product Selection

```bash
onchainos defi search --token <token> --chain <chain>
onchainos defi detail --investment-id <id>
onchainos defi rate-chart --investment-id <id>
onchainos defi tvl-chart --investment-id <id>
```

| Factor | Deposit | Consider Alternatives | Skip |
|---|---|---|---|
| APY | < 20% sustainable | 20–50% check emissions | > 50% WARNING |
| TVL trend | Stable or growing | Declining slowly | Declining rapidly |
| Protocol age | > 1 year | 6–12 months | < 3 months |
| Audits | 3+ independent | 1–2 audits | No audits |
| Token pair | Correlated (ETH/stETH, USDC/USDT) | Moderately correlated | Highly uncorrelated (high IL risk) |
| Product type | SINGLE_EARN (lending) | DEX_POOL (LP) | Complex multi-step strategies |

### Impermanent Loss Decision

| Pair Type | Expected IL | APY Threshold to LP |
|---|---|---|
| Stablecoin-stablecoin (USDC/USDT) | Near 0% | Any positive APY |
| Correlated (ETH/stETH, wBTC/renBTC) | Very low (~0.5% at 1.25× move) | APY > 3% |
| Major pair (ETH/USDC) | Moderate (~5.7% at 2× move) | APY > 10% to compensate |
| Volatile pair (MEME/ETH) | Very high (~25% at 5× move) | APY > 50% to even consider |
| Opposite direction pair | Catastrophic | Never LP |

### V3 Position Sizing (Concentrated Liquidity)

```bash
onchainos defi depth-price-chart --investment-id <id>
```

| Tick Range Width | Capital Efficiency | IL Risk | Best For |
|---|---|---|---|
| Very narrow (1–2% range) | Very high | High if prices move out of range | Ranging markets, high conviction on price |
| Narrow (5–10% range) | High | Moderate | Slightly trending markets |
| Wide (20–50% range) | Low | Lower | Uncertain direction, wider comfort |
| Full range | Very low | Lowest | New to LPing; want simple, low-maintenance |

**Rule**: Only enter V3 positions where you'd be comfortable holding both tokens at current prices.

---

## 7. Common Mistakes to Avoid

### Psychological Mistakes

| Mistake | Description | Prevention |
|---|---|---|
| FOMO buying | Buying because price is pumping | Wait for a pullback to support; use limit orders |
| Revenge trading | Immediately re-entering after a loss to "win it back" | 24-hour cooldown after a loss; reduce size, don't increase |
| Holding losers too long | Refusing to sell positions below stop-loss | Pre-commit to stops; no exceptions |
| Taking profits too early | Selling winners while holding losers | Use trailing stops; let winners run to targets |
| Overconfidence after wins | Increasing position sizes after a winning streak | Maintain fixed risk %; don't increase after wins |
| Anchoring | Refusing to sell because "I bought at $X" | The market doesn't care about your entry price; evaluate current data |

### Technical Mistakes

| Mistake | Description | Prevention |
|---|---|---|
| Skipping security scan | Buying a token without running token-scan | ALWAYS run `security token-scan` before buying |
| Ignoring liquidity | Entering a position too large for the pool's depth | Check `token liquidity` and `swap quote` slippage |
| Entering without stop-loss | No predefined exit if the trade goes wrong | Define stop BEFORE entry; use the ATR method for placement |
| Over-concentration | Too much in correlated positions | Count correlated positions as one; keep total sector risk <20% |
| No rebalancing | Portfolio drifting away from target allocation | Review monthly; rebalance when any asset class deviates >5% from target |
| Trading illiquid tokens | Entering positions in thin-order-book tokens without slippage awareness | Check quote first; if price impact >3%, reduce size or skip |

### On-Chain Specific Mistakes

| Mistake | Description | Prevention |
|---|---|---|
| Not checking approvals | Token approvals can be exploited | Run `security approvals` regularly; revoke unused approvals |
| Skipping MEV protection | Large trades front-run by MEV bots | Enable MEV protection when trade value exceeds chain threshold |
| Trusting token names | Fake tokens with similar names/symbols | Always verify contract address, not name/symbol |
| Reusing stale quotes | Price changed between quote and execution | Re-quote if >10 seconds elapsed |
| Ignoring tax rates | Some tokens have buy/sell taxes | Check `taxRate` from `security token-scan` or `swap quote` |
| Not checking tx simulation | Sending tx without checking if it will succeed | Use `gateway simulate` or `security tx-scan` for large transactions |
| Using unlimited approvals | Approving max uint256 for token transfers | Approve only the exact amount needed; revoke after use |