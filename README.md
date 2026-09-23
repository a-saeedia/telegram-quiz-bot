# A | Cap Quiz Bot

NestJS backend powering the A | Cap financial **risk-assessment quiz bot** on Telegram (and Bale). The bot walks users through a risk quiz, collects contact details behind a channel-join gate, and syncs results to Google Sheets.

## Features

- **Risk-assessment quiz** — question bank in `src/quiz/quiz-questions.ts`, scoring logic in `src/quiz/quiz-logic.ts`
- **Contact collection** — users must join the A | Cap channel before completing the flow
- **Dual platform** — Telegram first-class, with a Bale subclass that swaps the API base URL and token/channel env keys
- **Admin panel** — guarded admin endpoints (`src/admin/`) for managing the funnel
- **Google Sheets sync** — ambassador/leaderboard + contact spreadsheets updated via the Google Sheets API (`src/sheets/`)
- **Mini web app** (`src/webapp/`) — supporting web endpoints for the bot
- **Swagger API docs** at `/api-docs` (no-cache headers set for safe serving)
- **Prisma + Postgres** persistence — users, answers and sync flags (`prisma/schema.prisma`)

## Architecture

| Module | Responsibility |
|---|---|
| `telegram` | Telegram webhook handling, photo sending, channel gate |
| `bale` | Bale platform adapter (overrides API base URL + env keys) |
| `quiz` | Question bank, quiz logic & scoring |
| `admin` | Guarded admin API |
| `sheets` | Google Sheets sync (contacts, ambassador/leaderboard) |
| `webapp` | Mini App support |
| `prisma` | Database access layer |

## Environment

| Variable | Required | Purpose |
|---|---|---|
| `TELEGRAM_BOT_TOKEN` | yes | Telegram bot token |
| `TELEGRAM_CHANNEL_ID` | yes | Channel users must join |
| `DATABASE_URL` | yes | Postgres connection string for Prisma |
| `GOOGLE_CREDENTIALS_JSON` | no | Service-account JSON for Sheets sync (sync disabled when unset) |

## Run locally

```bash
npm install
# set the env vars above, then
npm run start:dev        # watch mode
npm run start:prod       # production build
```

API docs: http://localhost:3000/api-docs

## Deploy

Vercel-ready — `vercel.json` is included and the build script runs `prisma generate && nest build` (`vercel-build`). See the `vercel-build` script in `package.json`.

## Test

```bash
npm run test       # unit
npm run test:e2e   # e2e
```