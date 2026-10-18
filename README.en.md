<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=180&section=header&text=Ronny&fontSize=60" />

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&center=true&width=500&lines=Backend+developer;Telegram+bots+in+production;Node.js+%26+Python" />

<img src="https://skillicons.dev/icons?i=python,nodejs,mongodb,postgres,linux,js,html,css" />

<br><br>

**🌐 Language:** [Español](README.md) · English

</div>

Self-taught backend developer based in Guayaquil, Ecuador. I build, deploy and maintain Telegram bots in production, from the code all the way to the server.

## Stack
- **Python:** Pyrogram, Kurigram, Telethon, PyTgCalls
- **Node.js:** Telegraf
- **Databases:** MongoDB, PostgreSQL
- **AI:** integration of multiple language model providers, text-to-speech (TTS) and transcription (Whisper)
- **Infrastructure:** Linux VPS, bot deployment and monitoring
- **Frontend:** HTML, CSS, JavaScript

## Darkangel Bots
A network of Telegram bots covering moderation, music, games and social interaction. I run the bots, the servers and the website on my own.

| Bot | What it does | What I did |
|---|---|---|
| [Akami](https://t.me/AkamiXbot) | Group administration and moderation | Based on [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (MIT license); I rewrote most of it into an async architecture. Full moderation, per-group roles, antiflood, federations, night mode, AI chatbot and more (see below) |
| [Bocchi Music](https://t.me/BocchiXmusicbot) | Music and video in group voice chats | Based on [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (MIT license); I rewrote a large part of it: audio and video streaming with up to 5 assistant accounts and multiple languages, resource limits for a shared VPS, a more robust lifecycle, backups and monitoring (see below) |
| [Sayomi Whisper](https://t.me/SayomiXwhisperbot) | Whispered messages and privacy | Plugin-based architecture (inline mode, privacy, statistics), Spanish and English, and status reporting to my website |
| [Diosa Moon](https://t.me/Diosamoonbot) | Community with games, progression and birthdays | Lineage system with quiz, points and rankings; timezone-aware birthdays; games with async timers; multilingual, with task recovery after restarts |
| [Moon Confesiones](https://t.me/MoonConfesiones_bot) | Public-facing anonymous confessions with moderation | Approval flow in an admin group, text and media publishing to a channel with quote support, user blocking, Telegram Stars donations and daily backups |
| [Mephisto Ryu](https://t.me/MephistoRyu_Bot) | Role-playing game with economy and rituals | Economy with coins, familiars and ad rewards; migration from MongoDB Atlas to a self-hosted server |
| [Vesta](https://t.me/Orusvesta_bot) | Daily horoscope and birth chart | 4-language Telegram mini app with daily, weekly and monthly horoscopes, compatibility, astral profile and a birth chart using my own astronomical calculations; AI astrologer with server-validated daily quotas; anonymous confessions and zodiac sign commands in the bot |
| [Puchu](https://t.me/Puchuflusbot) | Games and economy (inherited codebase; I maintain and develop it) | Turn-based multiplayer games in groups, economy with shop, gifts and badges, rankings with medals, World Cup 2026 betting with a knockout bracket, theme system, mass broadcasts, backups and a status command with server metrics (see below) |

> In several bots, the idea and design come from other people on the team; the programming and the server are mine. The exception is Puchu, which I received already built and now maintain and improve. You can try them directly from the links.

## What Akami includes
- **Moderation:** ban, temporary ban, ban with message deletion, kick, mute, temporary mute, warnings with configurable limit and punishment, filters that warn automatically, and a log of actions admins perform manually from Telegram
- **Administration:** admin promotion and demotion by level, titles and tags, message pinning, invite links, and management of group photo, title and description
- **Protection:** antiflood with a time window and five punishment modes, antiraid, anti linked channel, blacklist, group federations, cleanup of deleted accounts, and a night mode that locks and unlocks the group by schedule and timezone
- **Per-group roles (my own design):** King, Queen, Rooks, Bishops, Knights and Pawns, with commands and limits per role, synced with Telegram's native member tags
- **Community:** welcome and goodbye messages with media, welcome mute, join requests, AFK and sticker quotes (with per-group packs, ratings and its own cache, on top of a rendering service from [QuotLy](https://github.com/LyoSU/quote-bot), MIT license)
- **AI chatbot:** conversation with per-user memory, automatic fallback across five model providers with key rotation, voice note replies and transcription of incoming voice notes, with a status panel
- **Utilities:** zip and unzip with AES-256 encryption, handwritten-style text on images, and bulk message deletion
- **Interactive button panels** to configure almost everything without typing commands

## Akami architecture
- **Three Telegram clients in a single process** (PTB via webhook, Pyrogram and Telethon), with connection and concurrency tuning so none of them blocks the others
- **Group federations** with a global request limit and flood-wait handling, to apply sanctions across hundreds of chats without getting rate-limited
- **Periodic cleanup tasks** for caches and abandoned flows, preventing memory leaks
- **Graceful shutdown** on system signals and **automatic backups** of Postgres and Mongo with a daily cloud copy

> Akami is based on [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (AnonymousX1025, MIT license). The per-group roles, antiflood, AI chatbot, federation rewrite and async architecture are my own work.

## What Bocchi Music includes
- **Voice chat streaming:** audio and video in group voice chats, with up to 5 assistant accounts (userbots) and multiple languages
- **Bounded resources on a shared VPS:** the process is pinned to a single CPU core (configurable via environment variable), with limited concurrent downloads, per-user cooldowns and caps on duration and size
- **Automatic cleanup:** temporary folders (downloads, cache, playback files) are wiped on every startup
- **Operations:** scheduled daily backup, heartbeat to my website and a startup event to detect restart loops

## Bocchi Music architecture
- **Pyrogram (Kurigram) + PyTgCalls** with the bot and assistants in the same process and a single shared event loop
- **Configurable CPU affinity:** `BOCCHI_CPUS` sets the cores for the process and its children (ffmpeg included); defaults to core 0 only
- **Graceful shutdown:** teardown lives in a `finally` block, so on an error or a forced exit the scheduler, bot, userbot and HTTP sessions are all closed, and a failure in one step doesn't prevent the rest
- **Shared HTTP sessions** (Deezer and thumbnails), created once and closed on shutdown
- **Async scheduler (APScheduler)** with the Guayaquil timezone for the daily backup and the 10-minute heartbeat

> Bocchi Music is based on [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (AnonymousX1025, MIT license). The resource management, lifecycle and shutdown handling, backups, monitoring and the rewrite of a large part of the code are my own work.

## What Diosa Moon includes
- **Lineages:** four factions with a poll-based quiz, activity-based points, rankings and group abilities with a 24-hour cooldown
- **Games:** Stop with rounds and timers, hangman, scheduled questions, a lottery, and a stars game with ticket redemption
- **Birthdays** with per-user timezone, group and private notifications with no duplicates
- **Community:** welcome messages with join request approval, AFK, quotes with a per-group panel, and a lunar calendar
- **Operations:** automatic backups and a periodic heartbeat to detect restart loops

## What Vesta includes
- **Horoscope mini app:** today, week and month for all 12 signs in Spanish, English, Italian and French, with an animated background and a lightweight mode that turns on automatically on low-end devices
- **Cached horoscopes:** horoscope.com scraping stored in MongoDB with per-period expiry, cached translations and a nightly warm-up of all signs, so nobody pays for a live scrape
- **AI astrologer:** chat with the day's horoscope as context, with a per-user daily quota verified on the server
- **Compatibility, astral profile and birth chart:** prompts are built on the server from validated data, JSON responses are validated and shared through a cache across users
- **Client-side birth chart:** positions of the Sun, Moon and planets using Kepler's equation, ascendant computed from city and time, and an SVG zodiac wheel
- **Rewarded ads (Monetag):** extra queries for watching an ad, with the reward decided by the server rather than the client
- **Bot:** `/vesta` opens the mini app, 12 per-sign quote commands with a no-repeat pool, `/topsignos` with a weekly ranking (group or global), and anonymous confessions with a Pillow-generated card, in free mode or with approval

## Vesta architecture
- **Bot and API in one process:** Pyrogram (Kurigram) and an aiohttp server share the event loop
- **Mini app authentication:** HMAC verification of Telegram's signed `initData`, with expiry
- **AI provider chain:** Groq → Gemini → OpenRouter, with key rotation, cooldown on rate limits and fast failover to the next provider
- **Operations:** automatic backups every 30 minutes with a daily copy, heartbeat to my website and a startup event to detect restart loops

## What Puchu includes
- **Multiplayer group games:** Bandido (detectives vs. bandits, with rooms, voting and items), Veloz, Tornado, Memoria, Ruleta, Dado, Bomba, Globo, Lobo and Tragapuchus (slot machine with a growing jackpot). Every game has a waiting room, a minimum player count, admin multipliers and bonuses in special games
- **Waiting list:** `/puchunext` notifies you privately when a new game starts in the group
- **Economy:** puchupuntos, a shop with a single catalog, gifts between users, transfers with a numeric keypad and dynamic fee, custom titles and purchasable badges with a per-user cap
- **Rankings and medals:** global, monthly and per-game leaderboards, with quarterly medals awarded automatically
- **Puchutime:** an event that rewards the first people to write, staggered across affiliated groups
- **World Cup 2026 betting:** multipliers from x1 to x1000, a prize pool shared among winning bets, refunds if nobody wins, withdrawal on suspended matches, a calendar with times per country, `/topmundial` and a knockout bracket generated as an image. Already finished: toggled on and off with a single flag
- **Themes ("masivas"):** each user picks a character per group that replaces their name in games, with limited and paid changes
- **Community commands:** `/puchushipp`, `/puchuconsejo`, `/puchununca`, truth or dare in several languages, personality questions and action GIFs (`!kiss`, `!hug`…) filtered by gender
- **Administration:** broadcasts to groups and users with a progress bar, chat blacklist, reset of stuck games, and updating with `git pull` plus restart straight from the chat
- **Multilingual** per group and per user

## Puchu architecture
- **Node.js with Telegraf and MongoDB (Mongoose)**, with game state in temporary files so timers can resume after a restart
- **Centralized error handling:** harmless Telegram errors are logged compactly and the rest are reported with their stack trace to a log group
- **`getChatMember` cache** with expiry and periodic cleanup to avoid saturating the API
- **Backups:** rotating `mongodump` every 3 hours and a daily send of the latest copy to the creators, split into parts if large
- **Monitoring:** heartbeat to my website and a status command with latency, CPU, RAM, disk, process memory and MongoDB state

## Technical problems I solved
- **Race conditions in betting (Puchu):** per-user locks to prevent duplicate bets, and a fix for the World Cup bracket logic
- **Bets that could over- or undercharge (Puchu):** the selector always started at x1 even when the user already had a bet, balance validation compared against the total instead of the difference when modifying, and switching outcome lost the previous bet without a refund; now it starts at the real amount, validates the real cost and requires withdrawing before betting on a different outcome
- **Knockout bracket that wouldn't advance (Puchu):** the mapping from round of 16 to quarterfinals, semifinals and final, and from losers to third place, was missing; winners didn't move on and the bracket stayed on "?"
- **Unreliable results API (Puchu):** I protected manually entered status and scorers, stopped the scoreboard from going backwards, discarded impossible states (second half before halftime) and added a fallback with a second data source
- **Match times across timezones (Puchu):** every stadium has its own timezone and the calendar shows the time in several countries
- **Prizes with no winners (Puchu):** if nobody wins, everyone is refunded and the match is flagged so the history displays correctly
- **Players "coming back to life" in the roulette (Puchu):** an `await` before saving let a late callback overwrite the eliminated state; the state is now computed and saved synchronously
- **Empty keyboards in Tornado (Puchu):** cells with no text made Telegram fail with "Can't find field text"; they are now skipped and no empty keyboard is sent
- **Games stuck on errors (Puchu):** Tragapuchus only released the chat on the happy path; now it does so on any error too, and there's a command to free stuck games and their players' locks
- **Badge migration without duplicates (Puchu):** when splitting purchasable badges into new identifiers, I wrote an idempotent migration that credits what users already had while respecting the per-user cap
- **"Change welcome" mode getting stuck (Puchu):** I added a 2-minute expiry so an admin who abandons the panel doesn't lock everyone else out of editing
- **Puchutime flooding (Puchu):** I rewrote it with an execution lock, a single notice per group without enough players, and the interval spread according to the number of groups
- **Group to supergroup migration (Puchu):** on migration, settings and player records move to the new identifier
- **Telegram limits (Puchu):** sends with retry based on `retry_after` and spaced-out edits in live messages to avoid 429s
- **VPS load (Puchu):** I lowered the rotating backup from every 30 minutes to every 3 hours and moved it out of the folder nodemon watches
- **Startup that never confirmed the connection (Puchu):** Telegraf's `bot.launch()` doesn't resolve until the bot stops; I confirm the connection with `getMe()` and only then start the heartbeat
- **Shop centralization (Puchu):** a single catalog to eliminate sync errors between menus
- **Repeated updates after a restart (Akami):** the bot reprocessed old messages; I fixed it with a top-priority handler and in-memory deduplication
- **Accidental permanent bans (Akami):** in antiflood with a temporary punishment, if the message was already deleted the bot didn't read the duration and banned forever; it now uses the stored value and has a safe fallback
- **Corrupted flood counter (Akami):** queued messages from an already muted user wiped other users' progress; they are now ignored using a member cache that's invalidated on punishment
- **Bulk deletion without reading history (Akami):** bots can't read old messages, so I delete by ID range; if a batch fails because of service messages, I split it in half until the problematic one is isolated, and resume after flood waits
- **Night mode that self-corrects (Akami):** I compare the schedule by range, so after a restart the group recovers its correct state in under a minute
- **AI provider outages (Akami):** the chatbot automatically falls back to the next provider and rotates keys and models when one fails
- **Log spam when configuring (Akami):** every click in the panel generated a notice; I added a 5-second debounce to send a single log with the final value
- **Disappearing logs (Mephisto Ryu):** a misconfigured logger swallowed every info message; I found and fixed it
- **Inconsistent data in MongoDB (Mephisto Ryu):** I found that some users were stored with a different ID type and became invisible to the rest of the bot
- **Telegram button limit (Mephisto Ryu):** a whole dictionary was being serialized into the button data and exceeded the 64-byte limit
- **Quotes lost on publishing (Moon Confesiones):** copying a message that replied to another lost the quote; I rebuilt the content with a quote block respecting Telegram's character limits, with a fallback if it didn't fit
- **Event loop error on startup (Moon Confesiones):** the client binds its event loop when created and a second loop broke startup; I reused the same loop instead of creating a new one
- **Resource limits on a shared VPS (Bocchi):** I pinned the process to one core, limited concurrent downloads, added per-user cooldowns, duration and size caps, and automatic cleanup of temporary files
- **Shutdown that left things open (Bocchi):** if something failed or a `SystemExit` arrived, the bot, userbot, HTTP sessions and scheduler weren't closed; I moved teardown into a `finally` with independent steps
- **Hardcoded CPU core (Bocchi):** the CPU limit was written into the code; it is now configured with `BOCCHI_CPUS`, with core 0 as the default
- **Quiz that survives restarts (Diosa Moon):** I store progress in MongoDB with an expiry date, so nobody loses their progress when the bot restarts
- **"Farmable" points (Diosa Moon):** I added a minimum gap between messages, a temporary block on bursts and a minimum group size so points can't be accumulated through spam
- **Cache that expired at 7 PM (Vesta):** the server runs on UTC and the "today" cache expired at UTC midnight; I moved the calculation to Guayaquil time
- **Quotas under concurrent requests (Vesta):** message consumption uses an atomic conditional update in MongoDB, so it can't be overspent with parallel requests
- **Messages charged when the AI failed (Vesta):** if no provider responds or the JSON is invalid, the usage or query is returned to the user
- **Easy-to-fake ads (Vesta):** the reward requires a server-registered start, a minimum and maximum time watching the ad, and a single claim per day per section; the Monetag postback is idempotent
- **Secret exposed in logs (Vesta):** the access log was printing the postback token; I wrote a custom logger that masks it
- **Blocked translations (Vesta):** Google Translate was blocking the VPS IP, so AI became the primary route, with Google as a fallback and cached results
- **English stored as another language (Vesta):** if translation failed, the client cached the English text as Spanish or French; now the server reports the real language and the client only caches if it matches
- **Duplicate scrapes (Vesta):** a per-key lock ensures only one request scrapes and the others wait for the cache
- **Malformed AI responses (Vesta):** I extract the JSON even if it comes wrapped in markdown or surrounding text, and validate types, ranges and lengths before saving
- **Double `/start` with confessions (Vesta):** the confessions deep link stops message propagation so the `/start` with a GIF doesn't also fire
- **Stable weekly ranking (Vesta):** `/topsignos` is generated with a seed derived from the scope and the week, and saved with a conditional insert to avoid races

## Other projects
| [Vorath](https://t.me/Vorath_bot) | Retro game library inside Telegram | Pyrogram (Kurigram) bot whose `/arcade` command opens, via a button, a Telegram mini app with the game library; per-user and per-group language, a lightweight mode for low-end devices, automatic backups and restart-with-update from the chat itself |

> My production code lives in private repos. If you're a recruiter or evaluating my work, message me and I'll give you read access.

## Currently learning
JavaScript certification on freeCodeCamp.

## Contact
<div align="center">

<a href="https://darkangelbots.dev/"><img src="https://img.shields.io/badge/Web-darkangelbots.dev-6c5ce7?style=for-the-badge&logo=googlechrome&logoColor=white" /></a>
<a href="https://t.me/darkangeldev"><img src="https://img.shields.io/badge/Telegram-@darkangeldev-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" /></a>
<a href="mailto:contacto@darkangelbots.dev"><img src="https://img.shields.io/badge/Email-contacto@darkangelbots.dev-ea4335?style=for-the-badge&logo=gmail&logoColor=white" /></a>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=100&section=footer" />

</div>
