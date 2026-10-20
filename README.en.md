<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=180&section=header&text=Ronny&fontSize=60" />

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&center=true&width=500&lines=Backend+developer;Telegram+bots+in+production;Node.js+%26+Python" />

<img src="https://skillicons.dev/icons?i=python,nodejs,mongodb,postgres,linux,js,html,css" />

**🌐 Language:** [Español](README.md) · English

<br><br>

</div>

Self-taught backend developer based in Guayaquil, Ecuador. I write, deploy and maintain Telegram bots in production: from the code all the way to the server.

## Stack
- **Python:** Pyrogram, Kurigram, Telethon, PyTgCalls
- **Node.js:** Telegraf
- **Databases:** MongoDB, PostgreSQL
- **AI:** integration of several language model providers, voice (TTS) and transcription (Whisper)
- **Infrastructure:** Linux VPS, bot deployment and monitoring
- **Cloudflare:** Workers, KV, D1 and Cron Triggers (monitoring and website)
- **Frontend:** HTML, CSS, JavaScript

## Darkangel Bots
A network of Telegram bots for moderation, music, games and social interaction. I maintain the bots, the servers and the website by myself.

| Bot | What it does | What I did |
|---|---|---|
| [Akami](https://t.me/AkamiXbot) | Group administration and moderation | Based on [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (MIT license), with a large part rewritten to an asynchronous architecture. Full moderation, per-group roles, antiflood, federations, night mode, AI chatbot and more (see below) |
| [Bocchi Music](https://t.me/BocchiXmusicbot) | Music and video in group voice chats | Based on [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (MIT license), with a large part rewritten: audio and video streaming with up to 5 assistant accounts and multiple languages, resource limits for a shared VPS, a more robust lifecycle, backups and monitoring (see below) |
| [Sayomi Whisper](https://t.me/SayomiXwhisperbot) | Whispered messages and privacy | Plugin-based architecture (inline mode, privacy, statistics), Spanish and English, and status reporting to my website |
| [Diosa Moon](https://t.me/Diosamoonbot) | Community with games, progression and birthdays | Lineage system with quiz, points and rankings; timezone-aware birthdays; games with asynchronous timers; multilanguage and task resumption after restarts |
| [Moon Confesiones](https://t.me/MoonConfesiones_bot) | Public-facing anonymous confessions with moderation | Approval flow in an admin group, text and media publishing to a channel with quote support, user blocking, Telegram Stars donations and daily backups |
| [Mephisto Ryu](https://t.me/MephistoRyu_Bot) | Role-playing game with economy and rituals | Economy with coins, familiars and rewards for watching ads; migration from MongoDB Atlas to a self-hosted server |
| [Vesta](https://t.me/Orusvesta_bot) | Daily horoscope and natal chart | 4-language Telegram miniapp with daily, weekly and monthly horoscopes, compatibility, astral profile and natal chart with custom astronomical calculation; AI astrologer with server-validated daily quotas; anonymous confessions and zodiac sign commands in the bot |
| [Puchu](https://t.me/Puchuflusbot) | Games and economy (inherited code; I maintain and develop it) | Turn-based multiplayer games in groups, economy with shop, gifts and badges, rankings with medals, World Cup 2026 betting with a knockout bracket, theme system, mass broadcasts, backups and a status command with server metrics (see below) |

> In several bots the idea and design come from other members of the team; the programming and the server are mine. The exception is Puchu, which I received already built and for which I handle maintenance and improvements. You can try them directly from the links.

## Bot monitoring and status
The bots report to a custom monitoring system running on Cloudflare (Workers, KV, D1 and Cron Triggers), designed to fit in the free plan. The bots only make outbound requests, so the VPS needs no open ports or certificates for this.

- **Heartbeat:** every bot sends a heartbeat every 10 to 20 minutes to the [darkangelbots.dev](https://darkangelbots.dev/) Worker, authenticated with a secret header. A bot is "alive" if its last heartbeat is less than 45 minutes old, "dead" if not, or "maintenance" if I set it manually
- **Restart loop detection:** every startup sends a `start` event that is stored in D1 with no rate limit. A healthy bot sends one a day, while one that keeps restarting sends one every few minutes, even if its heartbeat still says "alive"
- **Pinned message in a Telegram channel:** a cron every 2 minutes always edits the message with the status and the time of the last check, and only re-pins it (to notify subscribers) when a bot changes state. Telegram's service notice deletes itself
- **Separate alert** in Mephisto Ryu's official group when its status changes
- **Availability history in D1:** every status change is recorded with its timestamp
- **Maintenance mode:** `/mantenimiento <bot> on|off` from Telegram, admins only and with the webhook verified by a secret token, or through the authenticated API
- **Public dashboard:** [darkangelbots.dev/estado](https://darkangelbots.dev/estado) queries `/status.json` every minute and shows how many bots are active, in several languages

## What Akami includes
- **Moderation:** ban, temporary ban, ban with message deletion, kick, mute, temporary mute, warnings with configurable limit and penalty, filters that warn automatically, and logging of the actions admins take manually from Telegram
- **Administration:** tiered admin promotion and demotion, titles and tags, message pinning, invite links, and management of the group's photo, title and description
- **Protection:** antiflood with a time window and five penalty modes, antiraid, anti linked channel, blacklist, group federations, cleanup of deleted accounts, and a night mode that closes and opens the group based on schedule and timezone
- **Per-group roles (own design):** King, Queen, Rooks, Bishops, Knights and Pawns, with commands and limits per role, synced with Telegram's native tags
- **Community:** welcome and goodbye messages with media, welcome mute, join requests, AFK and sticker quotes (with per-group packs, ratings and own caches, on top of a rendering service from [QuotLy](https://github.com/LyoSU/quote-bot), MIT license)
- **AI chatbot:** conversation with per-user memory, automatic fallback across five model providers with key rotation, voice note replies and transcription of received voice notes, with a status panel
- **Utilities:** zip and unzip with AES-256 encryption, handwritten-style text on images, and bulk message deletion
- **Interactive button panels** to configure almost everything without typing commands

## Akami's architecture
- **Three Telegram clients in a single process** (PTB via webhook, Pyrogram and Telethon), with tuned connections and concurrency so none of them blocks the others
- **Group federations** with a global request limit and flood-wait handling, to sanction across hundreds of chats without getting blocked
- **Periodic cleanup tasks** for caches and abandoned flows, preventing memory leaks
- **Graceful shutdown** on system signals and **automatic backups** of Postgres and Mongo with a daily cloud copy

> Akami is based on [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (AnonymousX1025, MIT license). The per-group roles, the antiflood, the AI chatbot, the federation rewrite and the asynchronous architecture are my own work.

## What Bocchi Music includes
- **Playback:** `/play` and `/vplay` for audio and video in the voice chat, `/cplay` and `/cvplay` to stream to a channel, and `force` variants to put a track ahead of the queue
- **Sources:** Telegram audio and video files (with size and duration caps), live streams with m3u8 links, and compatible links
- **Queue and controls:** queue with position, pause, resume, skip, loop and stop, with buttons on every playback and a self-updating progress bar
- **Vote to skip:** in groups that enable it, a minimum number of votes decides the skip
- **Per-group permissions:** `/playmode` and `/settings` define whether only admins or everyone can control playback, the playback mode, authorized users and language
- **Channel playback:** `/channelplay` links a channel, and to link an external one the bot verifies that the requester is its owner
- **Thumbnails** with circular cover art, title, channel and progress bar, with disk cache
- **Up to 5 assistant accounts** (userbots) and multiple languages
- **Bounded resources on a shared VPS:** limited simultaneous downloads, per-user cooldown, duration and size caps, and cleanup of temporary folders on every startup
- **Operations:** scheduled daily backup, heartbeat to my website and a startup event to detect restart loops

## Bocchi Music's architecture
- **Pyrogram (Kurigram) + PyTgCalls** with the bot and assistants in the same process and a single shared event loop
- **Configurable CPU affinity:** `BOCCHI_CPUS` defines the cores for the process and its children (ffmpeg included); by default only core 0
- **Graceful shutdown:** the teardown lives in a `finally`, so on an error or forced exit the scheduler, bot, userbot and HTTP sessions are closed, and a failure in one step doesn't prevent the following ones
- **Shared HTTP sessions** (Deezer and thumbnails) created once and closed on shutdown
- **Asynchronous scheduler (APScheduler)** with the Guayaquil timezone for the daily backup and the heartbeat every 10 minutes

> Bocchi Music is based on [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (AnonymousX1025, MIT license). Resource management, lifecycle and shutdown, backups, monitoring and the rewrite of a large part of the code are my own work.

## What Diosa Moon includes
- **Lineages:** four factions with a poll-based quiz, activity-based points, rankings and group abilities with a 24-hour cooldown
- **Games:** Stop with rounds and timers, hangman, scheduled questions, lottery and a stars game with ticket redemption
- **Birthdays** with per-user timezone, group and private notifications without duplicates
- **Community:** welcomes with join request approval, AFK, quotes with a per-group panel and a lunar calendar
- **Operations:** automatic backups and periodic heartbeat to detect restart loops

## What Vesta includes
- **Horoscope miniapp:** today, week and month for all 12 signs in Spanish, English, Italian and French, with an animated background and a lightweight mode that activates automatically on low-end devices
- **Cached horoscope:** scraping of horoscope.com stored in MongoDB with per-period expiration, cached translations and nightly pre-warming of all signs, so nobody pays for a live scrape
- **AI astrologer:** chat with the day's horoscope as context, with a per-user daily quota verified on the server
- **Compatibility, astral profile and natal chart:** prompts are built on the server with validated data, JSON responses are validated and shared through cache between users
- **Natal chart calculated on the client:** positions of the Sun, Moon and planets using Kepler's equation, ascendant from the city and time, and a zodiac wheel in SVG
- **Rewarded ads (Monetag):** extra queries for watching an ad, with the reward decided by the server and not the client
- **Bot:** `/vesta` opens the miniapp, 12 per-sign quote commands with a no-repeat pool, `/topsignos` with a weekly ranking (group or global) and anonymous confessions with a card generated with Pillow, in free mode or with approval

## Vesta's architecture
- **Bot and API in the same process:** Pyrogram (Kurigram) and an aiohttp server share the event loop
- **Miniapp authentication:** HMAC verification of Telegram's signed `initData`, with expiration
- **AI provider chain:** Groq → Gemini → OpenRouter, with key rotation, cooldown on rate limits and fast fallthrough to the next provider
- **Operations:** automatic backups every 30 minutes with a daily copy, heartbeat to my website and a startup event to detect restart loops

## What Puchu includes
- **Multiplayer games in groups:** Bandido (detectives vs. bandits, with rooms, voting and items), Veloz, Tornado, Memoria, Ruleta, Dado, Bomba, Globo, Lobo and Tragapuchus (slot machine with a progressive jackpot). Every game has a waiting room, a minimum player count, multipliers for admins and bonuses in special games
- **Waiting list:** `/puchunext` notifies you privately when a new game starts in the group
- **Economy:** puchupoints, shop with a single catalog, gifts between users, transfers with a numeric keypad and dynamic fee, custom titles and purchasable badges with a per-user cap
- **Rankings and medals:** global, monthly and per-game top, with quarterly medals awarded automatically
- **Puchutime:** event that hands out points to whoever writes first, staggered across affiliated groups
- **World Cup 2026 betting:** multipliers from x1 to x1000, prize pool shared among winners, refund if nobody wins, withdrawal in suspended matches, schedule with times per country, `/topmundial` and a knockout bracket generated as an image. Now finished: toggled on and off with a single flag
- **Themes ("masivas"):** each user picks a character per group that replaces their name in games, with limited and paid changes
- **Community commands:** `/puchushipp`, `/puchuconsejo`, `/puchununca`, truth or dare in several languages, personality questions and action GIFs (`!kiss`, `!hug`…) filtered by gender
- **Administration:** broadcast to groups and users with a progress bar, chat blacklist, reset of stuck games and update via `git pull` plus restart from the chat itself
- **Multilanguage** per group and per user

## Puchu's architecture
- **Node.js with Telegraf and MongoDB (Mongoose)**, with game state in temporary files that allow timers to resume after a restart
- **Centralized error handling:** harmless Telegram errors are logged compactly and the rest are reported with their trace to a logs group
- **`getChatMember` cache** with expiration and periodic cleanup to avoid saturating the API
- **Backups:** rotating `mongodump` every 3 hours and daily delivery of the latest copy to the creators, split into parts if large
- **Monitoring:** heartbeat to my website and a status command with latency, CPU, RAM, disk, process memory and MongoDB status

## Technical problems I solved
- **Race conditions in bets (Puchu):** per-user locks to prevent duplicate or double-charged bets from double taps or Telegram retries
- **Bets that could charge too much or too little (Puchu):** the selector always started at x1 even if the user already had a bet, the balance validation compared against the total instead of the difference when modifying, and switching outcomes lost the previous bet without a refund; it now starts at the real amount, validates the real cost and requires withdrawing before betting on another outcome
- **Knockout bracket that wouldn't advance (Puchu):** the mapping from round of 16 to quarterfinals, semifinals and final, and from the losers to third place, was missing; winners didn't move on and the bracket stayed on "?"
- **Unreliable results API (Puchu):** I protected manually loaded status and scorers, prevented the score from going backwards, discarded impossible states (second half before halftime) and added a fallback with a second data source
- **Goals announced twice (Puchu):** when the manual notice and the API update coincided, the same goal was announced twice; now every goal has an announcement key and the score is compared before notifying
- **Match times in different timezones (Puchu):** each stadium has its own timezone and the schedule shows the time in several countries
- **Prizes with no winners (Puchu):** if nobody wins, everyone is refunded and the match is flagged so the history displays it properly
- **Two users with the same character (Puchu):** a unique index per group and theme prevents it, and if the change fails at the last moment the cost is refunded
- **Players who "came back to life" in the roulette (Puchu):** an `await` before saving let a late callback overwrite the eliminated state; the state is now computed and saved synchronously
- **Empty keyboards in Tornado (Puchu):** cells with no text made Telegram fail with "Can't find field text"; they are now skipped and an empty keyboard is never sent
- **Games stuck by errors (Puchu):** Tragapuchus only released the chat on the happy path; it now does so on any error too, and there is a command to free stuck games and their players' locks
- **Badge migration without duplicates (Puchu):** when splitting purchasable badges into new identifiers, I wrote an idempotent migration that credits what users already had while respecting the per-user cap
- **"Change welcome" mode that got stuck (Puchu):** I added a 2-minute expiration so an admin who abandons the panel doesn't leave others unable to edit
- **Puchutime without flood (Puchu):** I rewrote it with an execution lock, a single notice per group without enough players, and the interval spread according to the number of groups
- **Group to supergroup migration (Puchu):** on migration, the configuration and player records move to the new identifier
- **Telegram limits (Puchu):** sends with retry based on `retry_after` and spaced-out edits on live messages to avoid 429s
- **VPS load (Puchu):** I lowered the rotating backup from every 30 minutes to every 3 hours and stored it outside the folder watched by nodemon
- **Startup that didn't confirm the connection (Puchu):** Telegraf's `bot.launch()` doesn't resolve until the bot stops; I confirm the connection with `getMe()` and only then start the heartbeat
- **Shop centralization (Puchu):** a single catalog to eliminate sync errors between menus
- **Repeated updates after a restart (Akami):** the bot reprocessed old messages; I solved it with a maximum-priority handler and deduplication using in-memory structures
- **Accidental permanent bans (Akami):** in the antiflood with temporary penalty, if the message was already deleted the bot didn't read the duration and banned forever; it now uses the stored value and has a safe fallback
- **Corrupted flood counter (Akami):** queued messages from an already muted user wiped other users' progress; it now ignores them with a member cache that is invalidated on sanction
- **Bulk deletion without reading history (Akami):** bots can't read old messages, so I delete by ID range; if a batch fails because of service messages, I split it in half until the problematic one is isolated, and resume after flood waits
- **Night mode that self-corrects (Akami):** I compare the schedule by range, so after a restart the group recovers its correct state in under a minute
- **AI provider outages (Akami):** the chatbot automatically switches to the next provider and rotates keys and models when one fails
- **Log spam when configuring (Akami):** every click in the panel generated a notice; I added a 5-second debounce to send a single log with the final value
- **Logs that disappeared (Mephisto Ryu):** a misconfigured logger swallowed all informational messages; I found and fixed it
- **Inconsistent data in MongoDB (Mephisto Ryu):** I detected that some users were stored with a different identifier type and became invisible to the rest of the bot
- **Telegram button limit (Mephisto Ryu):** a whole dictionary was serialized into the button data and exceeded the 64-byte limit
- **Quotes lost on publishing (Moon Confesiones):** copying a message that replied to another lost the quote; I rebuilt the content with a quote block respecting Telegram's character limits, with a fallback if it didn't fit
- **Event loop error on startup (Moon Confesiones):** the client binds its event loop at creation and a second loop broke startup; I reused the same loop instead of creating a new one
- **Resource limits on a shared VPS (Bocchi):** I pinned the process to one core, limited simultaneous downloads, added per-user cooldown, duration and size caps, and automatic cleanup of temporary files
- **Shutdown that left things open (Bocchi):** if something failed or a `SystemExit` arrived, the bot, userbot, HTTP sessions and scheduler weren't closed; I moved the teardown to a `finally` with independent steps
- **Fixed core that didn't adapt (Bocchi):** the CPU limit was hardcoded; it is now configured with `BOCCHI_CPUS`, with core 0 as the default
- **Quiz that survives restarts (Diosa Moon):** I store progress in MongoDB with an expiration date, so nobody loses their progress when the bot restarts
- **"Farmable" points (Diosa Moon):** I added a minimum gap between messages, a temporary block on bursts and a minimum group size so points can't be accumulated through spam
- **Cache that expired at 19:00 (Vesta):** the server is in UTC and the "today" cache expired at UTC midnight; I moved the calculation to Guayaquil time
- **Quotas with simultaneous requests (Vesta):** message consumption uses an atomic conditional update in MongoDB, so it can't be overspent with parallel requests
- **Messages charged when the AI failed (Vesta):** if no provider responds or the JSON is invalid, the usage or the query is returned to the user
- **Easily faked ads (Vesta):** the reward requires a start registered on the server, a minimum and maximum time watching the ad, and a single claim per day per section; the Monetag postback is idempotent
- **Secret exposed in logs (Vesta):** the access log printed the postback token; I wrote a custom logger that masks it
- **Blocked translations (Vesta):** Google Translate blocked the VPS IP, so AI became the primary route, with Google as a fallback and cached results
- **English saved as another language (Vesta):** if translation failed, the client cached the English text as Spanish or French; the server now reports the real language and the client only caches if it matches
- **Duplicate scrapes (Vesta):** a per-key lock makes only one request scrape while the others wait for the cache
- **Malformed AI responses (Vesta):** I extract the JSON even when it comes wrapped in markdown or surrounding text and validate types, ranges and lengths before saving
- **Double `/start` with confessions (Vesta):** the confessions deep link stops the message from propagating so the `/start` with GIF doesn't arrive too
- **Stable weekly ranking (Vesta):** `/topsignos` is generated with a seed derived from the scope and the week, and saved with a conditional insert to avoid races
- **Workers KV write limit (Monitoring):** I consolidated the keys into a single JSON object and limited writes to one every 30 minutes per bot, regardless of heartbeat frequency, to avoid exhausting the free daily cap
- **Notification loops (Monitoring):** the channel message is only re-pinned when some bot's status changes, and the new status is saved before touching the pin to avoid a pin/unpin cycle if KV stops accepting writes
- **"Alive" bots that were actually restarting (Monitoring):** the heartbeat alone doesn't reveal a restart loop, so I added a startup event with no rate limit to see it
- **False "dead" flags (Monitoring):** the 45-minute threshold equals two missed heartbeats of the longest interval plus a margin, so one lost heartbeat doesn't mark a bot as down

## Other projects
- **[Vorath](https://t.me/Vorath_bot):** retro game library inside Telegram. Pyrogram (Kurigram) bot whose `/arcade` command opens, with a button, a Telegram miniapp with the game library; per-user and per-group language, lightweight mode for low-end devices, automatic backups and restart with update from the chat itself

> My production code lives in private repos. If you're a recruiter or evaluating my work, write to me and I'll give you read access.

## Currently learning
JavaScript certification on freeCodeCamp.

## Contact
<div align="center">

<a href="https://darkangelbots.dev/"><img src="https://img.shields.io/badge/Web-darkangelbots.dev-6c5ce7?style=for-the-badge&logo=googlechrome&logoColor=white" /></a>
<a href="https://t.me/darkangeldev"><img src="https://img.shields.io/badge/Telegram-@darkangeldev-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" /></a>
<a href="mailto:contacto@darkangelbots.dev"><img src="https://img.shields.io/badge/Email-contacto@darkangelbots.dev-ea4335?style=for-the-badge&logo=gmail&logoColor=white" /></a>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=100&section=footer" />

</div>
