# SuperView

**Say what you believe. Watch it play out on Monad.**

SuperView is a social investing app built around **views**: you write one sentence, an agent turns it into a sized basket of tokenized stocks, you publish it, and the book is marked live against the S&P 500. Stock research and execution in this tree focus on **Monad (chain 143)**: **xStocks**, **AUSD** cash, issuer catalog sync, and **passkey** signing for live legs through **Mera**.

More detail on the Monad path: [METROPOLIS.md](./METROPOLIS.md).

---

## Monad desk

| Area | Implementation |
| --- | --- |
| Catalog | Worker paginates the xStocks Monad deployment list, upserts symbols, and retires rows only after a full sync |
| Research | Committee pipeline (interpret, screen, diligence, construct, critic) scoped to **Monad** listings |
| Marks | Issuer marks feed paper fills and the vs-SPY chart on each view |
| Cash | **AUSD** on Monad in wallet; funding paths in `.env.example` (Aurora / Base USDC when enabled) |
| Live | Kuru Flow quotes for xStock ↔ AUSD; API prepares txs, **Mera passkey** signs in the browser, API broadcasts |
| Auth | Passkey login; EVM + Solana addresses derived client-side |

Paper mode runs end-to-end without live keys. Live mode needs the flags and keys in `.env.example`.

---

## User flow (5 minutes)

1. Open **New view** and enter a thesis (example: “NVDA and hyperscalers win the AI capex cycle”).
2. Watch the research job in the UI: logs reference **Monad** listing counts and screened tickers.
3. **Publish** the basket. Holdings use Monad xStock symbols (typically `…x` suffix in data; UI strips for display where configured).
4. **Paper invest** on the view. Fills hit the pocket ledger against live marks.
5. Open **Wallet**: unlock with passkey, confirm Monad EVM address and **AUSD** balance.
6. Optional live leg: enable live flags, fund AUSD, submit a buy; confirm hash on Monadvision from the fill record.

---

## Research pipeline (high level)

```
belief text
  -> interpreter     mechanism, horizon, falsifiers
  -> screen          Monad xStocks catalog + web names mapped to listings
  -> diligence       per-name notes and scores
  -> portfolio       5-12 names, role caps, weights
  -> critic          second model can force a revision
  -> publish         social object + holdings + live stream
```

Models and env knobs are in `.env.example`. The worker runs construction asynchronously; the API exposes job status and streams quotes over SSE.

---

## Monorepo layout

| Path | Role |
| --- | --- |
| `apps/web` | Compose, feed, view page, wallet, portfolio |
| `apps/api` | Auth, research jobs, catalog routes, SSE quotes |
| `apps/worker` | xStocks sync, marks, research worker |
| `packages/ai` | Committee agents + venue hints |
| `packages/markets` | xStocks client, Kuru Flow, Aurora, rate limits |
| `packages/core` | Baskets, ledger, token cards, vs-SPY series |
| `packages/db` | Prisma schema, migrations, seed |
| `packages/shared` | Worlds, chain ids, catalog search helpers |

---

## Run locally

```bash
pnpm install
cp .env.example .env
docker compose up -d
pnpm db:generate && pnpm db:migrate && pnpm db:seed
pnpm dev
```

| Service | URL |
| --- | --- |
| Web | http://localhost:3000 |
| API | http://localhost:4000 (`GET /health`) |

**Required for a real research run:** `OPENAI_API_KEY`

**Optional (live / funding / extra quotes):** Privy, 0x, Kuru, Aurora, Nansen keys as listed in `.env.example`. Development can use demo auth; do not enable demo user in production.

```bash
pnpm test
```
