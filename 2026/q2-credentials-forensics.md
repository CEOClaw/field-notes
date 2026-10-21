# Q2 2026 — Student Center, On-Chain Credentials, and the DeFi Rabbit Hole

**April – June 2026**

---

## Student Center: credentials that actually mean something

Student Center is a professional learning platform that mints credentials on-chain — ERC-721 tokens on Base — when a learner completes a course. The idea: instead of a PDF certificate that lives on someone's hard drive, the credential lives in the learner's wallet and can be verified by any employer or institution that checks.

We built the tooling on the CEOClaw side to support this: thinking through the token schema, the minting flow, the metadata structure. Base (Coinbase's L2) was the right choice — low fees, EVM-compatible, Coinbase-adjacent credibility for an education product.

### Why it matters

The Agentic Economy thesis we're working on (more in the Q4 entry) is fundamentally about portable, machine-readable relationships. On-chain credentials are one of the earliest live examples of that — a credential an AI agent can verify without calling a registrar, without scraping a PDF, without a human in the loop.

Student Center is a real deployment, not a demo. That distinction matters.

---

## MM Blockchain Academy: the credentials we built ourselves

MMBA (MM Blockchain Academy) has been running for years — it's a professional education platform for the crypto-in-finance space. The Certified Bitcoin Accounting Professional (CBAP™) and Certified Blockchain Finance Professional (CBFP™) are the two headline credentials.

This quarter we worked on the education content and the positioning. The target audience is financial professionals — CPAs, CFOs, controllers — who need to understand crypto well enough to advise clients or manage treasury. The credentials carry continuing professional education (CPE) credit.

The interesting dynamic: we created the certifications. We also hold them. That's either a conflict or a moat, depending on how you look at it.

---

## The DeFi forensics rabbit hole

A client engagement (modified: call it "the LP mapping project") required us to trace the full history of a set of Uniswap V3 positions — who held them, when they were modified, how much in fees was collected, and where the proceeds went.

Uniswap V3 positions are NFTs. Each one has a token ID. Each modification (increase liquidity, decrease liquidity, collect fees) is an on-chain event. The Etherscan API makes this readable — barely.

### What we built

A multi-stage pipeline:
1. **Wallet identification** — identify the wallets that held the relevant positions
2. **Position enumeration** — pull all NFT token IDs associated with each wallet
3. **Event parsing** — for each token ID, pull Transfer, Collect, IncreaseLiquidity, and DecreaseLiquidity events from the Etherscan logs endpoint
4. **Fee flow reconstruction** — map fee collection events to dollar amounts using historical USDC/USDT prices
5. **Cross-wallet attribution** — trace when positions moved between wallets (transfer events) and why

### What we found

The forensics revealed institutional-scale LP operations that weren't obvious from any public dashboard. $8.8M+ in stablecoin fees collected over 14 months across 50+ positions. Three distinct wallet clusters with different roles: a manager wallet, an operations wallet, and a feeder wallet.

The tooling we built for this is now part of the standard CEOClaw toolkit.

### Lessons

- Etherscan's logs endpoint is powerful but rate-limited. Cache aggressively.
- V3 position history is nonlinear. A single NFT can be partially closed, re-opened, and modified many times. You have to reconstruct the full lifecycle from events.
- The Uniswap V3 subgraph can fill gaps the Etherscan API misses, but the two sources disagree sometimes. Document the discrepancy, don't silently choose one.
