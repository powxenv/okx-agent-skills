# OKX X Layer Skills

Three independent agent skills for on-chain trading, automation, and Uniswap integration on X Layer. Each one works on its own — install whichever you need.

## okx-trading

Full-lifecycle on-chain trading — from research to execution. 30+ CLI commands covering token analysis, security scanning, market data, smart money tracking, DEX swaps, DeFi yield, meme trenches, portfolio management, and wallet operations.

- **Research**: search tokens, pull price info, check holders, liquidity, and advanced metrics
- **Security**: token-scan (honeypot detection), dapp-scan, tx-scan, signature scan, approval checks
- **Market & signals**: real-time prices, klines, portfolio PnL, smart money tracking, buy signals
- **Swaps**: quote → confirm → execute with MEV protection across 500+ DEX sources
- **DeFi yield**: search products, invest, withdraw, claim rewards, track positions
- **Wallets**: login, balance, send, history
- **Memes**: hot tokens, dev reputation, bundle/sniper detection
- **Workflows**: step-by-step guides for buy, sell, research, DeFi yield, and meme trading
- **Best practices**: risk framework, trading strategies, market analysis, decision checklists

## okx-xlayer-agent

Automation framework for building autonomous agents on X Layer. Leverages X Layer's near-zero gas ($0.0005/tx) and 1-second finality so agents can actually afford to run continuously.

- **Sense → Decide → Act** loop with mandatory security gates on every trade
- **Four agent patterns**: DCA, smart money follower, DeFi auto-compounder, x402 self-funding
- **Configurable risk limits**: per-trade risk caps, portfolio heat limits, daily trade/loss limits, stop-losses
- **X Layer optimizations**: zero-gas compounding, auto-rebalancing, MEV-free small trades
- **WebSocket monitoring**: real-time price and signal data
- **Silent mode**: heartbeat logging, action-only output for production agents
- **x402 integration**: agents can pay for services and receive payments from other agents
- **Full CLI reference**: every onchainos command used in agent loops, with all trading workflows, risk framework, and decision checklists included

## okx-uniswap

Direct Uniswap protocol interaction for agents — V3 concentrated liquidity management, Trading API swaps, x402 payments, and LP rebalancing on X Layer and EVM chains.

- **Swaps**: OKX aggregator (best price across 500+ DEXes) or Uniswap Trading API (direct protocol access)
- **V3 LP management**: full position lifecycle — mint, monitor, rebalance, collect fees using `cast` (Foundry)
- **X Layer advantage**: rebalancing every 5 minutes costs ~$4.32/day, making active LP strategies viable
- **Route comparison**: quote both OKX aggregator and Uniswap, pick the better price
- **x402 payments**: pay for API access via OnchainOS or Tempo CLI, with auto-swap funding
- **Five agent patterns**: auto-rebalancing LP, fee harvesting, yield farming, cross-chain arbitrage, smart money + Uniswap
- **Trading API reference**: complete 3-step flow, Permit2 integration, routing types, error handling
- **Liquidity management reference**: V3 position lifecycle, tick math, impermanent loss calculator

**Prerequisites**: OnchainOS CLI for all operations. Foundry (`cast`) for V3 LP management. Uniswap API key for Trading API.

## Quick Start

Install OnchainOS (required for all three skills):

```bash
curl -fsSL https://raw.githubusercontent.com/okx/plugin-store/main/install-local.sh | bash
```

Install Foundry (only needed for Uniswap V3 LP management in `okx-uniswap`):

```bash
curl -L https://foundry.paradigm.xyz | bash && foundryup
```

Authenticate:

```bash
onchainos wallet login <email>
onchainos wallet verify <code>
onchainos wallet status
```

Research → scan → buy:

```bash
onchainos token search --query "USDC" --chains xlayer
onchainos security token-scan --address <addr> --chain xlayer
onchainos swap execute --from <native> --to <token> --readable-amount "10" --chain xlayer --wallet <addr>
```

## Why X Layer

X Layer makes agent strategies economically viable that would be impractical on Ethereum:

| | X Layer | Ethereum |
|---|---|---|
| Gas per tx | ~$0.0005 | ~$2–50 |
| Block time | ~1 second | ~12 seconds |
| Rebalancing daily | $0.015/day | $300/day |
| Rebalancing hourly | $0.36/day | $7,200/day |
| Rebalancing every 5 min | $4.32/day | Impractical |
| Auto-compounding | Profitable | Gas > rewards |

## File Structure

```
skills/
├── okx-trading/          # 27 files — full trading lifecycle
├── okx-xlayer-agent/     # 18 files — autonomous agent framework
└── okx-uniswap/          # 13 files — Uniswap protocol integration
```

Each skill contains its own `SKILL.md`, `agents/openai.yaml`, `_shared/` files, and `references/` — no cross-skill dependencies.

## Resources

- [OKX OnchainOS](https://github.com/okx/onchainos-skills)
- [X Layer Docs](https://docs.xlayer.io/)
- [Uniswap AI Tools](https://github.com/Uniswap/uniswap-ai)

## License

MIT