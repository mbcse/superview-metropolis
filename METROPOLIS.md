# Metropolis — SuperView on Monad

SuperView turns a one-line belief into a researched basket of tokenized stocks. For Metropolis, the **Monad desk** is the focus: xStocks on chain 143, **AUSD** cash, and optional **passkey (Mera)** client-signed live trades.

## What to try

1. Run locally (`README.md`). Seed the catalog so Monad xStocks are active.
2. **New view** — write a thesis (e.g. AI infra, megacap tech). Research should screen the **Monad** catalog.
3. **Wallet** — passkey unlock shows EVM (Monad) and Solana addresses; Monad **AUSD** balance reads from the desk API.
4. **Paper invest** on a published view — fills use live marks; Monad legs route through Kuru Flow when a quote exists.
5. **Live** (optional) — with flags and keys in `.env.example`, Mera signs the transaction in-browser; explorer links come from confirmed fills.

## Stack (Monad path)

| Piece | Role |
| --- | --- |
| xStocks API | Monad deployment list + marks |
| Kuru Flow | Monad xStock ↔ AUSD quotes |
| Aurora Intents | Base USDC → Monad AUSD (funding path) |
| Mera | Passkey-derived keys; API prepares, browser signs |

## Judge checklist

- Passkey login (not password custody)
- Research log mentions **Monad** listings / desk census
- Basket holds Monad xStock symbols (`…x`)
- Wallet shows **AUSD** on Monad
- At least one paper or live fill with a real tx hash on Monadvision when live is enabled

## Track

Onchain Finance & Trading — [Metropolis](https://monad.xyz/developers/hackathons/metropolis).
