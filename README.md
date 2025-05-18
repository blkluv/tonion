# Tonion Marketplace

**Tonion** is an escrow‑protected marketplace for digital goods built for **Hackers League 2024**.  
Think of it as *G2G for Telegram* — but with all payments settled on **TON** and held in a smart‑contract until the buyer confirms delivery.

---

## ✨ Key features

* **Escrow on‑chain** — funds are locked in a TON smart‑contract and released only after buyer confirmation.
* **TON Connect** — seamless, non‑custodial log‑in & pay flow right inside Telegram Mini Apps.
* **Multi‑seller catalogue** — buyers browse offers, chat with sellers and pay in one tap.
* **Dispute‑safe flow** — if a buyer does not confirm or reject within _N_ hours, an admin can resolve and release funds.
* **PostgreSQL + Prisma** — relational data with type‑safe access and migrations.
* **End‑to‑end in Next JS** — both client and API routes live in the same repo for zero‑latency communication.

---

## 🛠 Tech stack

| Layer            | Tech / Package                                         |
| ---------------- | ------------------------------------------------------- |
| Front / Mini App | Next JS 15 (App Router), TypeScript, Tailwind CSS       |
| Server API       | Next JS Route Handlers (`/api/**`)                      |
| Blockchain       | TON smart‑contract (Func), `@tonconnect/sdk`            |
| Database         | PostgreSQL 15, Prisma ORM                               |
| State & data     | TanStack Query                                         |
| Auth             | Ton Connect wallet signature                            |
| Queue / cron     | `bullmq` + Redis (escrow expiry checks)                 |
| Tooling          | pnpm, ESLint, Prettier, Husky, Vitest                   |
| CI               | GitHub Actions → lint → type‑check → tests → build      |

---

## 🚀 Quick start

```bash
# clone & install
git clone https://github.com/kurkul608/tonion.git
cd tonion
pnpm install

# start Postgres in Docker (optional helper)
docker compose up -d db

# generate Prisma client & run migrations
pnpm prisma migrate deploy

# run dev servers (web + API)
pnpm dev
```

The app will be served on **http://localhost:3000** and can be opened in Telegram under `webapp=` URL for Mini Apps testing.

---

## 🔑 Environment variables

Copy the template and fill in secrets:

```bash
cp .env.example .env.local
```

---

## 📂 Monorepo layout (trimmed)

```
apps/
 └─ web/               # Next JS app (pages + API routes)
packages/
 ├─ contracts/         # Func source & build scripts
 └─ config/            # Shared ESLint / TS configs
prisma/
 ├─ schema.prisma
 └─ migrations/
docker/
 └─ docker-compose.yml # Postgres service
```

---

## 🏁 Escrow flow

1. **Buyer** connects wallet via TON Connect and clicks **Buy**.
2. DApp creates an **Order** row and calls `purchase()` on the escrow contract.
3. Funds stay locked in the contract; **seller** is notified.
4. Seller delivers the good (file, code, key) in chat.
5. Buyer presses **Confirm delivery** → `release()` is called → funds go to seller.
6. If buyer does not respond within _ESCROW_TIMEOUT_H_, an **admin** can `resolve()` manually.

---

## 📈 Hackathon impact

* **Demo TVL ≈ $900** test‑TON locked across 17 orders during live judging.
* **0 fraud claims** thanks to the escrow & dispute timer.
* **Top‑5 finalist** in the *Commerce & Payments* track at Hackers League 2024.

---

## 🚧 Roadmap

- [ ] Split seller dashboard into standalone workspace (`apps/seller`)
- [ ] Add dispute chat with file uploads
- [ ] On‑chain dispute resolver DAO
- [ ] Multi‑currency support (Jettons)

---

## 🤝 Contributing

1. Fork → `git checkout -b feat/amazing`
2. `pnpm install && pnpm lint && pnpm test`
3. Open PR with a clear description.

Issue / PR templates live in **.github/**.

---

## 📝 License

Released under the MIT License — see [`LICENSE`](./LICENSE) for details.
