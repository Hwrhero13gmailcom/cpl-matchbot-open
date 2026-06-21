# CPL Match Bot

> A full-featured Discord bot built for the **Competitive Penguin League** — a 300+ member competitive gaming server. Handles ranked matchmaking, live economy, referee assignment, and automated match management with zero manual staff intervention required.

---

## What it does

Running a competitive Discord server means staff are constantly managing match codes, logging scores, updating leaderboards, and handing out roles manually. This bot automates all of that with a single slash command.

A mod runs `/startmatch` or players fill a queue — the bot handles the rest. Private threads, lobby codes, referee assignment, score logging, MMR updates, rank role assignment, and thread cleanup all happen automatically.

---

## Features

**Ranked Queue System**
Players press a button to join the queue. The embed updates live showing each player's rank and MMR (e.g. `Bronze • 56/100 MMR`). At 6 players the bot automatically splits teams, pings an available referee, and opens a private match thread.

**Referee Flow**
The referee gets pinged inside the match thread with an Accept button — only members with the Referee role can press it. Once accepted, the lobby code is revealed. If no ref responds within 5 minutes, an admin can skip the process.

**MMR & Rank System**
Individual MMR tracking with 7 rank tiers: Bronze → Silver → Gold → Emerald → Diamond → Netherite → Champions. Rank roles are automatically assigned and updated after every match. New players complete 3 placement matches before receiving a rank.

**Signup Flow**
Players run `/signup`, select their experience level (Newbie / Novice / Pro), and receive the Unranked role — gating them into the queue system.

**Penguin Coins (PC) Economy**
Players earn PC alongside MMR from winning matches. Admins configure a shop with role rewards or custom items. Players spend PC via `/shop buy` — role rewards are auto-assigned on purchase.

**Scrim & Session System**
`/scrim` posts a react-to-join embed for casual 6-player scrims with automatic team splits and referee selection. `/code` does the same for quick 4-player sessions with a random lobby code in a private thread.

**Live Status Channel**
A voice channel updates automatically every 5 minutes to show bot uptime status, powered by UptimeRobot.

**Auto Leaderboard**
A designated channel updates with team standings after every match — no manual updates needed.

---

## Commands

| Command | Access | Description |
|---|---|---|
| `/signup` | Everyone | Register for ranked, pick experience level |
| `/stats [@player]` | Everyone | View MMR, rank, PC, W/L, and rank progress |
| `/leaderboard` | Everyone | Team season standings |
| `/matchhistory` | Everyone | Recent match results |
| `/shop view / buy` | Everyone | Browse and purchase from the PC shop |
| `/scrim` | Authorized | Post a 6-player scrim signup |
| `/code` | Authorized | Post a 4-player session signup |
| `/startmatch @t1 @t2` | Mods | Start a manual team match |
| `/endmatch [score]` | Mods | Log result, award MMR, auto-delete thread |
| `/addmmr /removemmr /addpc` | Admin | Manually adjust player stats |
| `/setupqueue` | Admin | Post the persistent queue embed |

---

## Tech Stack

- **Runtime** — Node.js
- **Library** — discord.js v14
- **Database** — SQLite via sql.js
- **Hosting** — Render (background worker) + UptimeRobot keepalive

---

## Architecture Notes

Commands and events load dynamically from their folders — adding a new command is dropping a file. Multiple handlers can listen to the same event, so the command dispatcher and queue button handler both live under `interactionCreate` without interfering.

The database runs schema migrations on every startup so deploying updates never wipes live data.

The queue uses polling instead of reaction event listeners — Discord's reaction events are unreliable on uncached messages. Every 5 seconds the bot fetches the live reaction list directly from the API and deduplicates it, keeping the count accurate regardless of spam.

---

## Live Deployment

Bot is live in the CPL server with 300+ members.

- Server: https://discord.gg/b5XdUFCmBB
- Uptime: https://stats.uptimerobot.com/YFT3xq38i7

---

*Built by Zylo, description by Claude*
