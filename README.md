<div align="center">

# BotWallet

**Your AI has a brain. Give it a wallet.**

[![Website](https://img.shields.io/badge/website-botwallet.co-black)](https://botwallet.co)
[![npm CLI](https://img.shields.io/npm/v/@botwallet/agent-cli?label=CLI&color=blue)](https://www.npmjs.com/package/@botwallet/agent-cli)
[![npm MCP](https://img.shields.io/npm/v/@botwallet/mcp?label=MCP&color=blue)](https://www.npmjs.com/package/@botwallet/mcp)
[![License](https://img.shields.io/badge/license-Apache%202.0-green)](LICENSE)

Open-source wallet infrastructure for AI agents.<br/>
Earn, spend, and trade USDC on Solana with human oversight and FROST threshold signing.

[Website](https://botwallet.co) · [Dashboard](https://app.botwallet.co) · [Docs](https://docs.botwallet.co) · [Pricing](https://botwallet.co/pricing/)

</div>

---

BotWallet gives AI agents a wallet they can actually use. Your agent creates invoices, pays other agents, accesses paid APIs, and withdraws funds. You set the spending limits, approve big transactions, and see every dollar that moves.

All money is USDC on Solana. No proprietary token. No volatile crypto. 1 USDC = $1.

## Quick start

```bash
npm install -g @botwallet/agent-cli
botwallet register --name "My Agent" --owner you@email.com
botwallet wallet balance
```

Three commands. Your agent has a wallet.

If you're using an MCP client (Claude Desktop, Cursor, Windsurf, Cline), add this to your config instead:

```json
{
  "mcpServers": {
    "botwallet": {
      "command": "npx",
      "args": ["-y", "@botwallet/mcp"]
    }
  }
}
```

Then tell your agent: *"Create a BotWallet for yourself."*

## What agents can do

**Earn.** Your agent creates a payment link and sends it to a client. Humans pay by card or crypto. Other agents pay on-chain.

```bash
botwallet paylink create 25.00 --desc "Research report"
botwallet paylink send <id> --to client@example.com
```

**Spend.** Pay other agents, merchants, or any Solana address. Guard rails check every transaction before it goes through.

```bash
botwallet pay @merchant 10.00
botwallet pay confirm <tx_id>
```

**Access paid APIs.** The x402 protocol lets agents pay per API call. Browse what's available, check prices, pay when ready.

```bash
botwallet x402 discover "weather"
botwallet x402 fetch https://api.example.com/forecast
botwallet x402 fetch confirm <fetch_id>
```

**Request funds.** When the wallet runs low, your agent asks you directly.

```bash
botwallet fund 50.00 --reason "API costs for the week"
```

## How signing works

Every wallet uses [FROST](https://eprint.iacr.org/2020/852) 2-of-2 threshold signatures. During wallet creation, a key generation ceremony produces two shares:

- **S1** lives on the agent's machine
- **S2** lives on BotWallet's server

The full private key never exists. Both shares must cooperate to produce a valid signature. A compromised server can't drain your wallet. A compromised agent can't either.

## Human oversight

You control what your agent can do on its own:

| Rule | What it does |
|------|-------------|
| Spending limits | Per-transaction caps and daily maximums |
| Merchant allowlists | Restrict who your agent can pay |
| Approval gates | Anything outside the rules pauses for your OK |
| Kill switch | Freeze a wallet or pull all funds, instantly |

Manage everything from the [dashboard](https://app.botwallet.co).

## Repositories

| Package | Description | Install |
|---------|------------|---------|
| [`agent-cli`](https://github.com/botwallet-co/agent-cli) | CLI for AI agents | `npm i -g @botwallet/agent-cli` |
| [`mcp`](https://github.com/botwallet-co/mcp) | MCP server for Claude, Cursor, Windsurf, Cline | `npx -y @botwallet/mcp` |
| [`cursor-plugin`](https://github.com/botwallet-co/cursor-plugin) | Cursor plugin (MCP + skill) | [Setup guide](https://github.com/botwallet-co/cursor-plugin) |
| [`botwallet-sign`](https://github.com/botwallet-co/botwallet-sign) | Browser-based transaction signing | [sign.botwallet.co](https://sign.botwallet.co) |
| [`botwallet-recovery`](https://github.com/botwallet-co/botwallet-recovery) | Standalone fund recovery tool | [recovery.botwallet.co](https://recovery.botwallet.co) |

## Architecture

```
┌─────────────┐     ┌──────────────────┐     ┌──────────┐
│  Your Agent │────▶│  BotWallet API   │────▶│  Solana  │
│  (S1 key)   │     │  (S2 key + rules)│     │  (USDC)  │
└─────────────┘     └──────────────────┘     └──────────┘
       │                     │
       │              ┌──────┴──────┐
       │              │  Dashboard  │
       └──────────────│  (You)      │
        guard rails   └─────────────┘
```

Your agent holds one key share. BotWallet holds the other. Neither can sign alone. You set the rules from the dashboard and approve anything that falls outside them.

## Why BotWallet

The code is open source. You can read every line that handles your agent's money.

It's non-custodial. Your agent's key share stays on your machine. We can't access it, even if we wanted to.

Transactions settle in USDC on Solana. Dollar-stable value, sub-second finality, fees under a penny. No speculative token.

Any agent that can make an HTTP call can use BotWallet. There's a CLI, an MCP server, a REST API, and a TypeScript SDK.

No setup fees. You pay only when your agent moves money.

## Links

- [botwallet.co](https://botwallet.co) &mdash; website
- [app.botwallet.co](https://app.botwallet.co) &mdash; dashboard
- [docs.botwallet.co](https://docs.botwallet.co) &mdash; documentation
- [@botwallet/agent-cli](https://www.npmjs.com/package/@botwallet/agent-cli) &mdash; npm (CLI)
- [@botwallet/mcp](https://www.npmjs.com/package/@botwallet/mcp) &mdash; npm (MCP server)
- [@botwallet_co](https://x.com/botwallet_co) &mdash; Twitter/X

## License

Apache 2.0
