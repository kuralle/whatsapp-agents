# Build your next WhatsApp bot with Kuralle

Three **deep, runnable** WhatsApp agent examples built on [Kuralle](https://github.com/kuralle/kuralle-agents) — showing free-form extraction, template-based (closed-window) sends, interactive messages routed by stable id, consent/handoff, and idempotent broadcasts. Every bot runs **multi-turn across WhatsApp / web / Instagram from one definition**, with outbound that is **window-safe by construction** (a closed-window free-form message can't leak — it converts to a template, tags, or defers).

These run **offline** against a built-in simulator (deterministic tests, no Meta) and **live** against a real model (one API key). To deploy a bot on a real WhatsApp Cloud API number, scaffold the starter: `npm create kuralle-agents@latest -- --template whatsapp-bot`.

## The bots

| Bot | Demonstrates |
|-----|--------------|
| [`booking/`](./booking) | Reservations — **free-form extraction** (date/time/party/name) → availability → **interactive slot choices (routed by stable id, not label)** → confirm → book; **closed-window hold-reminder template**. |
| [`pharmacy/`](./pharmacy) | Prescription orders — identity/insurance/address **extraction**, id-routed Rx + pickup/delivery, **approval-gated copay** (`needsApproval`), **consent/STOP**, **escalate → human** ownership, **closed-window refill-reminder template** + idempotent broadcast. |
| [`clothing/`](./clothing) | Store — **interactive** product/size/color (id-routed; size renders a WhatsApp list / Instagram carousel), **cart grow/shrink across turns**, checkout address **extraction**, payment, **opt-in-only promo broadcast template**. |

Each bot is three files: `bot.ts` (the flow agent + `engagement({ policies })` wiring), `run.ts` (a live-model multi-turn demo driven by the simulator — no live Meta), and `<bot>.test.ts` (deterministic, offline — `MockLanguageModelV3` + a mock template selector + recording clients).

## Quickstart

```bash
bun install
cp .env.example .env          # add OPENAI_API_KEY

bun test                      # deterministic, offline — no keys, no Meta
bun run booking               # live multi-turn demo (needs a model key)
```

`bun run pharmacy` and `bun run clothing` run the other two. Set `KURALLE_EXAMPLE_PROVIDER=openai|google|xai` to force a provider.

## What Kuralle gives you here

- **Window-safe outbound** — a non-removable pipeline converts/﻿defers anything that would violate the 24h window; closed-window free-form can't leak.
- **Durable, resumable flows** — structured `reply`/`collect`/`decide`/`action` nodes; SOP lives in flows, not prompts.
- **Exactly-once tools** — effect-log replay; a retried turn never double-charges or double-books.
- **Channel-agnostic** — one bot across WhatsApp / web / Instagram; add a channel = add a policy.
- **Stable-id interactive routing** — buttons/lists route by id, never by the (translatable) label shown.

## Built with

[`@kuralle-agents/core`](https://www.npmjs.com/package/@kuralle-agents/core) · [`@kuralle-agents/messaging`](https://www.npmjs.com/package/@kuralle-agents/messaging) · [`@kuralle-agents/messaging-meta`](https://www.npmjs.com/package/@kuralle-agents/messaging-meta) · [`@kuralle-agents/engagement`](https://www.npmjs.com/package/@kuralle-agents/engagement) — all `^0.3.0`.

Framework: [github.com/kuralle/kuralle-agents](https://github.com/kuralle/kuralle-agents)
