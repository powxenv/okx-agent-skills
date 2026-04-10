# OKX X Layer Skills Suite

Three **fully independent** agent skills for autonomous on-chain trading, earning, and competing. Each skill works standalone — install only what you need, no cross-skill dependencies.

## Skills

### 1. `okx-trading` — Comprehensive On-Chain Trading

The full trading lifecycle: 30+ CLI commands for token research, security scanning, market data, smart money tracking, DEX swap execution, DeFi yield farming, meme token trenches, portfolio management, wallet operations, and transaction broadcasting.

**Capabilities:**
- Token research & analysis: search, info, price-info, holders, liquidity, advanced-info
- Security scanning: token-scan, dapp-scan, tx-scan, sig-scan, approvals
- Market & signals: price, kline, portfolio PnL, smart money tracking, buy signals
- Swap execution: quote → confirm → execute with MEV protection
- DeFi yield: search, invest, withdraw, collect, positions
- Wallet management: login, balance, send, history
- Meme trenches: hot tokens, dev info, bundle detection
- Detailed workflows: buy, sell, research, DeFi yield, meme trading
- Best practices: risk framework, trading strategies, market analysis, decision checklists

### 2. `okx-xlayer-agent` — Autonomous Agent on X Layer

An automation framework for building autonomous agents on X Layer. Provides agent decision logic, monitoring loops, silent mode rules, risk limits, and four complete agent workflow patterns — all leveraging X Layer's near-zero gas and 1-second finality.

**Capabilities:**
- **Sense → Decide → Act** loop architecture with mandatory security gates
- Four agent patterns: DCA, Smart Money Follower, DeFi Auto-Compounder, X402 Self-Funding
- Configurable risk parameters: per-trade risk, portfolio heat, daily limits, stop-losses
- X Layer optimizations: zero-gas DeFi compounding, auto-rebalancing, MEV-free small trades
- WebSocket monitoring for real-time price and signal data
- Silent mode for production agents (heartbeat logging, action-only output)
- x402 payment integration for self-funding agent economics
- Complete CLI reference for all 30+ onchainos commands used in agent loops
- Full trading workflows, risk framework, and decision checklists included

### 3. `okx-uniswap` — Uniswap Protocol Integration for Agents

Direct Uniswap protocol interaction for autonomous agents — V3 concentrated liquidity management, Trading API swaps, x402 payments, and LP rebalancing on X Layer and EVM chains.

**Capabilities:**
- **Swap execution**: OKX aggregator (best price across 500+ DEXes) or Uniswap Trading API (direct protocol)
- **V3 LP management**: mint, monitor, rebalance, collect fees — complete lifecycle via `cast` + OnchainOS
- **X Layer LP advantage**: Rebalancing every 5 minutes costs ~$4.32/day vs. impractical on Ethereum
- **Route comparison**: Quote on both OKX aggregator and Uniswap, pick best execution
- **x402 payments**: Pay for API access via OnchainOS or Tempo CLI with auto-swap funding
- **Five agent patterns**: auto-rebalancing LP, fee harvesting, yield farming, cross-chain arbitrage, smart money + Uniswap
- **Trading API reference**: Full 3-step flow, Permit2 integration, routing types, error handling
- **Liquidity management reference**: Complete V3 position lifecycle with tick math and IL calculator

## Architecture

```
┌──────────────────────────────────────────────────────┐
│                   Agent Platform                     │
│                  (OpenClaw, Claude, etc.)              │
├──────────────────────────────────────────────────────┤
│                                                      │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │ okx-trading  │  │okx-xlayer-  │  │ okx-uniswap  │ │
│  │              │  │   agent      │  │              │ │
│  │ Research     │  │ Automation  │  │ V3 LP Mgmt   │ │
│  │ Security     │  │ Risk Gates  │  │ Trading API   │ │
│  │ Market Data  │  │ Monitoring  │  │ Swap Routing   │ │
│  │ Swap Execute │  │ Agent Loop  │  │ x402 Payments │ │
│  │ DeFi Yield   │  │ x402 Pay    │  │ Agent Patterns│ │
│  │ Portfolio    │  │ All 30+ CLI │  │ Fee Harvesting│ │
│  └──────┬───────┘  └──────┬──────┘  └──────┬───────┘ │
│         │                 │                 │         │
│         └────────┬───────┴─────────┬───────┘         │
│                  │                 │                  │
│         ┌────────▼───────┐ ┌──────▼────────┐         │
│         │  OnchainOS CLI  │ │  Uniswap API  │         │
│         │   (onchainos)   │ │  + Contracts   │         │
│         └────────┬───────┘ └──────┬────────┘         │
│                  │                 │                  │
└──────────────────┼─────────────────┼──────────────────┘
                   │                 │
         ┌─────────▼─────────────────▼──────────┐
         │           X Layer (Chain 196)          │
         │    ~$0.0005/tx  ·  1s finality  ·  EVM  │
         └────────────────────────────────────────┘
```

Each skill is fully self-contained with its own reference files, shared files, and agent config — no cross-skill dependencies.

## Quick Start

### Install OnchainOS

```bash
curl -fsSL https://raw.githubusercontent.com/okx/plugin-store/main/install-local.sh | bash
```

### Install Foundry (for Uniswap V3 LP management)

```bash
curl -L https://foundry.paradigm.xyz | bash && foundryup
```

### Authenticate

```bash
onchainos wallet login <email>
onchainos wallet verify <code>
onchainos wallet status  # Verify: Ready=true
```

### Example: Research, Scan, Buy

```bash
# 1. Research a token
onchainos token search --query "USDC" --chains xlayer
onchainos token price-info --address <addr> --chain xlayer

# 2. Security check (MANDATORY before every swap)
onchainos security token-scan --address <addr> --chain xlayer

# 3. Execute swap on X Layer
onchainos swap execute --from <native> --to <token> --readable-amount "10" --chain xlayer --wallet <addr>
```

### Example: V3 LP Agent on X Layer

```bash
# 1. Find a high-APY pool
onchainos defi list --chain xlayer

# 2. Check pool depth
onchainos defi depth-price-chart --investment-id <id>

# 3. Monitor price
onchainos ws start --channel price --token-pair "196:<addr>"

# 4. Open position (see okx-uniswap skill for full V3 mint process)
# 5. Auto-rebalance when price exits range (see agent-uniswap-patterns.md)
```

## X Layer Advantages for Autonomous Agents

| Feature | X Layer | Ethereum |
|---|---|---|
| Gas per tx | ~$0.0005 | ~$2-50 |
| Block time | ~1 second | ~12 seconds |
| Daily rebalance cost | $0.015 | $300 |
| Hourly rebalance cost | $0.36 | $7,200 |
| 5-min rebalance (agent) | $4.32/day | Impractical |
| Auto-compounding viable | Yes (gas < reward) | No (gas > reward) |

These economics make X Layer uniquely suited for autonomous agent strategies — what costs thousands on Ethereum costs pennies on X Layer.

## File Structure

```
skills/
├── okx-trading/                    # Comprehensive on-chain trading (27 files)
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   ├── _shared/                     # preflight, chain-support, native-tokens
│   └── references/                  # workflows, CLI refs, risk, strategies, etc.
│
├── okx-xlayer-agent/               # Autonomous agent on X Layer (18 files, standalone)
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   ├── _shared/                     # preflight, chain-support, native-tokens
│   └── references/                  # automation, risk, strategies, workflows, etc.
│
└── okx-uniswap/                    # Uniswap protocol for agents (13 files, standalone)
    ├── SKILL.md
    ├── agents/openai.yaml
    ├── _shared/                     # preflight, chain-support, native-tokens
    └── references/                  # trading-api, liquidity, x402, patterns, etc.
```

## Key Differentiators

1. **Each skill is fully independent** — install only the one you need, no cross-skill dependencies
2. **Built for X Layer** — every strategy and example exploits X Layer's zero-gas, 1-second finality economics
3. **Security-first** — mandatory `security token-scan` before every swap, risk gates before every agent action
4. **Full Uniswap integration** — Trading API, V3 LP lifecycle, route comparison, x402 payments, 5 LP agent patterns
5. **Agent-ready** — configurable loops with circuit breakers, risk limits, and logging for 24/7 autonomous operation
6. **Chinese language support** — keyword glossary maps Chinese trading terms to OnchainOS commands
7. **Progressive disclosure** — SKILL.md under 300 lines for quick start, detailed references loaded on demand
8. **Battle-tested workflows** — buy, sell, research, DeFi yield, and meme trading all have step-by-step guides

## Resources

- [OKX OnchainOS](https://github.com/okx/onchainos-skills) — CLI tool powering all on-chain operations
- [X Layer Docs](https://docs.xlayer.io/) — X Layer documentation
- [Uniswap AI Tools](https://github.com/Uniswap/uniswap-ai) — Uniswap AI skill reference

## License

MIT