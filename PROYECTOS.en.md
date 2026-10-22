# Technical details of my projects

**🌐 Language:** [Español](PROYECTOS.md) · English

[← Back to profile](README.en.md)

What each bot includes, how it is built and which real problems I solved. My production code lives in private repositories; if you want to review it, message me at [@darkangeldev](https://t.me/darkangeldev) and I will give you read access.

**Index:** [Akami](#akami) · [Bocchi Music](#bocchi-music) · [Diosa Moon](#diosa-moon) · [Mephisto Ryu](#mephisto-ryu) · [Vesta](#vesta) · [Puchu](#puchu) · [Moon Confesiones](#moon-confesiones) · [Monitoring](#monitoring-and-bot-status) · [Vorath](#vorath)

---

## Akami
Group administration and moderation. Based on [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (AnonymousX1025, MIT license). The per-group roles, the antiflood, the AI chatbot, the federations rewrite and the async architecture are my own work. [Documentation](https://darkangelbots.dev/akami/akami)

### What it includes
- **Moderation:** ban, temporary ban, ban with message deletion, kick, mute, temporary mute, warnings with a configurable limit and penalty, filters that warn automatically, and a log of the actions admins perform manually from Telegram
- **Administration:** admin promotion and demotion by levels, titles and tags, message pinning, invite links, and management of the group photo, title and description
- **Protection:** antiflood with a time window and five penalty modes, antiraid, anti linked channel, blacklist, group federations, cleanup of deleted accounts, and a night mode based on schedule and timezone
- **Per-group roles (own design):** King, Queen, Rooks, Bishops, Knights and Pawns, with commands and limits per role, synced with Telegram's native tags
- **Community:** welcome and goodbye messages with media, welcome mute, join requests, AFK and sticker quotes (with per-group packs, ratings and its own cache, on top of a rendering service from [QuotLy](https://github.com/LyoSU/quote-bot), MIT license)
- **AI chatbot:** conversation with per-user memory, automatic fallback across five providers with key rotation, voice note replies and transcription of received voice notes
- **Utilities:** zip and unzip with AES-256 encryption, handwritten text rendered as an image, and bulk message deletion
- **Interactive button panels** to configure almost everything without typing commands

### Architecture
- **Three Telegram clients in a single process** (PTB via webhook, Pyrogram and Telethon), with connection and concurrency tuning so none of them blocks the others
- **Group federations** with a global request limit and flood-wait handling, to sanction across hundreds of chats without getting blocked
- **Periodic cleanup tasks** for caches and abandoned flows, preventing memory leaks
- **Graceful shutdown** on system signals and **automatic backups** of Postgres and Mongo with a daily cloud copy

### Problems I solved
- **Repeated updates after a restart:** the bot reprocessed old messages; I fixed it with a top-priority handler and deduplication using in-memory structures
- **Permanent bans by mistake:** in the antiflood with a temporary penalty, if the message was already deleted the bot didn't read the duration and banned forever; it now uses the stored value and has a safe fallback
- **Corrupted flood counter:** queued messages from an already muted user wiped other users' progress; they are now ignored using a member cache that is invalidated on sanction
- **Bulk deletion without reading history:** bots can't read old messages, so I delete by ID range; if a batch fails because of service messages, I split it in half until the problematic one is isolated, and resume after flood waits
- **Night mode that self-corrects:** I compare the schedule as a range, so after a restart the group recovers its correct state in under a minute
- **AI provider outages:** the chatbot switches to the next provider and rotates keys and models when one fails
- **Log spam while configuring:** every click in the panel produced a notice; I added a 5-second wait so only one log with the final value is sent

---

## Bocchi Music
Music and video in group voice chats. Based on [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (AnonymousX1025, MIT license). Resource management, lifecycle and shutdown, backups, monitoring and the rewrite of a large part of the code are my own work. [Documentation](https://darkangelbots.dev/bocchi/bocchi)

### What it includes
- **Playback:** `/play` and `/vplay` for audio and video in the voice chat, `/cplay` and `/cvplay` to stream to a channel, and `force` variants to move a track ahead of the queue
- **Sources:** Telegram audio and video files (with size and duration limits), live streams via m3u8 links, and supported links
- **Queue and controls:** position, pause, resume, skip, loop and stop, with buttons and a progress bar that updates itself
- **Skip voting:** in groups that enable it, a minimum number of votes decides the skip
- **Per-group permissions:** `/playmode` and `/settings` define whether only admins or everyone can control playback, the playback mode, authorized users and language
- **Channel playback:** `/channelplay` links a channel, and to link an external one it verifies that the requester is its owner
- **Thumbnails** with a circular cover, title, channel and progress bar, cached on disk
- **Up to 5 assistant accounts** (userbots) and multiple languages
- **Operations:** scheduled daily backup, heartbeat to my site and a start event to detect restart loops

### Architecture
- **Pyrogram (Kurigram) + PyTgCalls** with the bot and assistants in the same process and a single shared event loop
- **Configurable CPU affinity:** `BOCCHI_CPUS` sets the cores for the process and its children (ffmpeg included); by default only core 0
- **Graceful shutdown:** shutdown lives in a `finally`, so on an error or forced exit the scheduler, bot, userbot and HTTP sessions are all closed, and a failure in one step doesn't prevent the following ones
- **Shared HTTP sessions** (Deezer and thumbnails) created once and closed on shutdown
- **Async scheduler (APScheduler)** with the Guayaquil timezone for the daily backup and the heartbeat every 10 minutes

### Problems I solved
- **Resource limits on a shared VPS:** I pinned the process to one core, limited simultaneous downloads, added per-user cooldowns, duration and size caps, and automatic cleanup of temporary files on every start
- **Shutdown that left things open:** if something failed or a `SystemExit` arrived, the bot, userbot, HTTP sessions and scheduler weren't closed; I moved shutdown into a `finally` with independent steps
- **Hardcoded CPU core:** the CPU limit was written in the code; it is now configurable via `BOCCHI_CPUS`

---

## Diosa Moon
Community with games, progress and birthdays. [Documentation](https://darkangelbots.dev/diosamoon/diosamoon)

### What it includes
- **Lineages:** four factions with a poll-based quiz, points based on activity, rankings and group abilities with a 24-hour cooldown
- **Games:** Stop with rounds and timers, hangman, scheduled questions, a lottery and a stars game with ticket redemption
- **Birthdays** with a per-user timezone, announcements in the group and in private without duplicates
- **Community:** welcome messages with join request approval, AFK, quotes with a per-group panel and a lunar calendar
- **Operations:** automatic backups and a periodic heartbeat to detect restart loops

### Problems I solved
- **Quiz that survives restarts:** I store progress in MongoDB with an expiration date, so nobody loses their progress when the bot restarts
- **"Farmable" points:** I added a minimum gap between messages, a temporary lock on bursts and a minimum group size so points can't be accumulated with spam

---

## Mephisto Ryu
Role-play game with an economy and rituals. [Documentation](https://darkangelbots.dev/mephisto/mephisto)

### What it includes
- **Husbandos (character collection):** a character appears in groups and whoever guesses it first wins it. It has 6 rarities, collector level, packs, a user marketplace with commission, a cosmic shop of numbered editions and a mini app to manage everything
- **Economy:** coins and chips, a 7-tier bank with taxes, loans between users and fixed-term deposits, and rewarded ads with atomic claiming
- **20+ multiplayer games:** bingo, UNO, blackjack, minesweeper royale, hunting, battleship, camp, BL trivia, word search, chess and checkers, among others. They have a lobby, pot betting and several use a mini app
- **Couples and family:** marriage and divorce through phrases instead of commands, adoption, and certificates that accumulate coins and are distributed every month, all inside transactions
- **AI chatbot:** a character with 18 emotional-state traits per user and per chat, four providers with key rotation, voice, and understanding of photos, audio, stickers and videos
- **Web mini apps:** Husbandos, Confessions, Shadow Chat (one-to-one anonymous chat), Chibi (virtual pet), Bingo, UNO, Word Search and Wall Rush
- **BL profile:** an animated card drawn with Pillow, as MP4 or GIF, with Omegaverse and Guideverse ranks
- **Interaction:** 70+ commands grouped into an arsenal, sign compatibility, couple of the day, category rankings and a multi-page profile
- **Content editable from Telegram:** manhwas, characters and photos live in Mongo and are edited with `/data_bl`, with hot reload and their own backup
- **Operations:** per-language texts, backups every 30 minutes, heartbeat with a start event and broadcasts that resume after a restart

### Architecture
- **Two servers on the same private network:** the bot runs in polling mode on one, not exposed, and the public API on the other. When a mini app needs to send a message, the API asks the bot through an internal route
- **Unified API with FastAPI and uvicorn:** a single process serves all the mini apps, with HMAC verification of Telegram's signed `initData` and an image proxy so the bot token never reaches the browser
- **MongoDB (Motor) shared** by the bot and the API, with transactions on changes that affect two people at once
- **Game data outside the code:** the economy lives in a single hot-reloadable file and, if Mongo fails on startup, the game uses the last snapshot saved to disk
- **Central match coordinator:** one game per chat and one player in only one match, without touching each plugin
- **Matches that survive a restart:** most games' state is saved in Mongo and resumed on startup

### Problems I solved
- **Logs that disappeared:** a misconfigured logger swallowed all informational messages; I found and fixed it
- **Inconsistent data in MongoDB:** I detected that some users were stored with a different ID type and became invisible to the rest of the bot
- **Telegram button limit:** a whole dictionary was being serialized into the button data and exceeded the 64-byte limit
- **Ads charged twice:** the claim is an atomic update in Mongo that only succeeds if the cooldown has passed, so a double tap or two tabs can't credit the same view twice
- **Origin chat spoofable from the mini app:** the chat is derived from the signed `initData` fields and not from what the browser sends in the request body
- **Half-finished couples:** marrying and divorcing were two separate updates, so a restart between them left one person married and the other not; they now run in a transaction, and the certificate payout is part of the same one
- **Duplicate messages in anonymous chat:** a per-message nonce with a 30-second lifetime discards retries, a per-recipient lock orders delivery and an in-RAM buffer avoids waiting on Mongo
- **Browser crashing in UNO:** the proxy rescales photos to the width the page asks for and caches them in memory and on disk; the catalog the page receives dropped from about 10 MB to about 1.6 MB
- **Game data tied to the code:** I moved manhwas, characters and photos from a file to Mongo with a migration that never deletes or overwrites, can be repeated without duplicating, and compares today's game against what it would build from Mongo alone before switching the source
- **Two games stepping on each other in the same chat:** a central guard blocks any start if a match already exists, and I resolved a circular import with blackjack by loading it at runtime
- **Refunds when leaving the lobby:** each game charges at a different moment, so the refund uses a "paid" flag that is cleared on refund to avoid refunding twice
- **Nicknames paid with Stars lost on restart:** the pending order is also stored in Mongo and the flow type travels inside the payment itself; if it isn't found, the payment is forwarded to staff instead of being silently lost
- **Mini app on low-end devices:** Bingo detects the device tier and network quality, using the classification Telegram sends in the User-Agent, and reacts to a network change on the fly

---

## Vesta
Daily horoscope and birth chart in a Telegram mini app. [Documentation](https://darkangelbots.dev/vesta/vesta)

### What it includes
- **Horoscope mini app:** today, week and month for the 12 signs in Spanish, English, Italian and French, with an animated background and a lightweight mode that activates automatically on low-end devices
- **Cached horoscope:** scraping of horoscope.com stored in MongoDB with per-period expiration, cached translations and a nightly warm-up of all signs, so nobody pays for a live scrape
- **AI astrologer:** chat with the day's horoscope as context, with a per-user daily quota verified on the server
- **Compatibility, astral profile and birth chart:** prompts are built on the server with validated data, JSON responses are validated and shared through a cache across users
- **Birth chart calculated on the client:** positions of the Sun, Moon and planets using Kepler's equation, ascendant from the city and time, and a zodiac wheel in SVG
- **Rewarded ads (Monetag):** extra queries for watching an ad, with the reward decided by the server and not the client
- **Bot:** `/vesta` opens the mini app, 12 per-sign quote commands with a non-repeating pool, `/topsignos` with a weekly ranking (group or global) and anonymous confessions with a card generated with Pillow, in free mode or with approval

### Architecture
- **Bot and API in the same process:** Pyrogram (Kurigram) and an aiohttp server share the event loop
- **Mini app authentication:** HMAC verification of Telegram's signed `initData`, with expiration
- **AI provider chain:** Groq → Gemini → OpenRouter, with key rotation, cooldown on usage limits and fast failover to the next provider
- **Operations:** automatic backups every 30 minutes with a daily copy, heartbeat to my site and a start event to detect restart loops

### Problems I solved
- **Cache that expired at 7 PM:** the server is in UTC and the "today" cache expired at midnight UTC; I moved the calculation to Guayaquil time
- **Quotas with simultaneous requests:** message consumption uses a conditional atomic update in MongoDB, so it can't be overspent with parallel requests
- **Messages charged when the AI failed:** if no provider responds or the JSON is invalid, the usage or the query is returned to the user
- **Easy-to-fake ads:** the reward requires a start registered on the server, a minimum and maximum time watching the ad, and a single claim per day per section; the Monetag postback is idempotent
- **Secret exposed in logs:** the access log printed the postback token; I wrote a custom logger that masks it
- **Blocked translations:** Google Translate was blocking the VPS IP, so AI became the main route, with Google as a fallback and cached results
- **English stored as another language:** if the translation failed, the client cached the English text as Spanish or French; now the server reports the actual language and the client only caches if it matches
- **Duplicate scrapes:** a per-key lock makes only one request scrape while the others wait for the cache
- **Malformed AI responses:** I extract the JSON even when it comes wrapped in markdown or surrounding text and validate types, ranges and lengths before storing it
- **Double `/start` with confessions:** the confessions deep link stops message propagation so the `/start` with GIF doesn't also arrive
- **Stable weekly ranking:** `/topsignos` is generated with a seed derived from the scope and the week, and stored with a conditional insert to avoid races

---

## Puchu
Games and economy. I received the code already built; I maintain and develop it. [Documentation](https://darkangelbots.dev/puchu/puchu)

### What it includes
- **Multiplayer games in groups:** Bandido (detectives vs. bandits, with rooms, voting and items), Veloz, Tornado, Memoria, Ruleta, Dado, Bomba, Globo, Lobo and Tragapuchus (slot machine with an accumulating jackpot). Each match has a waiting room, a minimum number of players, multipliers for admins and bonuses in special matches
- **Waiting list:** `/puchunext` notifies you in private when a new match starts in the group
- **Economy:** puchupoints, a shop with a single catalog, gifts between users, transfers with a numeric keypad and dynamic commission, custom titles and purchasable badges with a per-user cap
- **Rankings and medals:** global, monthly and per-game top lists, with quarterly medals awarded automatically
- **Puchutime:** an event that gives points to whoever writes first, staggered across affiliated groups
- **World Cup 2026 betting:** multipliers from x1 to x1000, a pot shared among winners, a refund if nobody wins, withdrawal in suspended matches, a calendar with times per country, `/topmundial` and a knockout bracket generated as an image. Already finished: it is toggled on and off with a single flag
- **Themes ("masivas"):** each user picks a character per group that replaces their name in games, with limited, paid changes
- **Community commands:** `/puchushipp`, `/puchuconsejo`, `/puchununca`, truth or dare in several languages, personality questions and action GIFs (`!kiss`, `!hug`…) filtered by gender
- **Administration:** broadcast to groups and users with a progress bar, chat blacklist, restart of stuck matches and update with `git pull` plus restart from the chat itself
- **Multilanguage** per group and per user

### Architecture
- **Node.js with Telegraf and MongoDB (Mongoose)**, with match state in temporary files that allow timers to resume after a restart
- **Centralized error handling:** harmless Telegram errors are logged compactly and the rest are reported with their trace to a logs group
- **`getChatMember` cache** with expiration and periodic cleanup to avoid saturating the API
- **Backups:** rotating `mongodump` every 3 hours and a daily delivery of the latest copy to the creators, split into parts if large
- **Monitoring:** heartbeat to my site and a status command with latency, CPU, RAM, disk, process memory and MongoDB status

### Problems I solved
**World Cup betting**
- **Race conditions:** per-user locks to prevent duplicate bets or bets charged twice due to double taps or Telegram retries
- **Bets that could charge too much or too little:** the selector always started at x1 even if the user already had a bet, the balance validation compared against the total instead of the difference when modifying, and switching outcome lost the previous bet without a refund; it now starts at the real amount, validates the real cost and requires withdrawing before betting on another outcome
- **Knockout bracket that didn't advance:** the mapping from round of 16 to quarterfinals, semifinals and final, and from losers to third place, was missing; winners now move on to the next round
- **Unreliable results API:** I protected manually loaded status and scorers, prevented the score from going backwards, discarded impossible states (second half before halftime) and added a fallback with a second data source
- **Goals announced twice:** each goal now has an announcement key and the score is compared before announcing
- **Match times in different timezones:** each stadium has its own timezone and the calendar shows the time in several countries
- **Prizes without winners:** if nobody wins, everyone is refunded and the match is flagged so the history shows it correctly

**Games and economy**
- **Two users with the same character:** a unique index per group and theme prevents it, and if the change fails at the last moment the cost is refunded
- **Players who "resurrected" in roulette:** an `await` before saving let a late callback overwrite the eliminated state; the state is now computed and saved synchronously
- **Empty keyboards in Tornado:** cells without text made Telegram fail with "Can't find field text"; they are now omitted
- **Matches stuck by errors:** Tragapuchus only released the chat on the happy path; it now does so on any error, and there is a command to free stuck matches and their players' locks
- **Badge migration without duplicates:** I wrote an idempotent migration that credits what users already had while respecting the per-user cap
- **Shop centralization:** a single catalog to eliminate sync errors between menus

**Operations**
- **"Change welcome" mode that stayed locked:** I added a 2-minute expiration so an admin who abandons the panel doesn't leave others unable to edit
- **Puchutime without flood:** I rewrote it with an execution lock, a single notice per group without enough players and the interval spread according to the number of groups
- **Group to supergroup migration:** settings and player records move to the new ID
- **Telegram limits:** sends with retry based on `retry_after` and spaced edits on live messages to avoid 429s
- **VPS load:** I lowered the rotating backup from every 30 minutes to every 3 hours and stored it outside the folder watched by nodemon
- **Startup that didn't confirm the connection:** Telegraf's `bot.launch()` doesn't resolve until the bot stops; I confirm the connection with `getMe()` and only then start the heartbeat

---

## Moon Confesiones
Public-facing anonymous confessions, with moderation. [Documentation](https://darkangelbots.dev/moonconfesiones/moonconfesiones)

- Approval flow in an admin group, publishing of text and media to a channel with quote support, user blocking, donations with Telegram Stars and daily backups

### Problems I solved
- **Quotes lost when publishing:** when copying a message that replied to another, the quote was lost; I rebuilt the content with a quote block respecting Telegram's character limits, with a fallback if it didn't fit
- **Event loop error on startup:** the client binds its event loop when created and a second loop broke startup; I reused the same loop instead of creating a new one

---

## Monitoring and bot status
The bots report to my own monitoring system running on Cloudflare (Workers, KV, D1 and Cron Triggers), designed to fit within the free tier. The bots only make outbound requests, so the VPS doesn't need open ports or certificates for this.

### What it includes
- **Heartbeat:** each bot sends a heartbeat every 10 to 20 minutes to the [darkangelbots.dev](https://darkangelbots.dev/) Worker, authenticated with a secret header. A bot is listed as "alive" if its last heartbeat is less than 45 minutes old, "dead" if not, or "maintenance" if I mark it manually
- **Restart loop detection:** each startup sends a `start` event stored in D1 with no rate limit. A healthy bot sends one per day, and one that restarts on its own sends one every few minutes, even if its heartbeat still says "alive"
- **Pinned message in a Telegram channel:** a cron every 2 minutes edits the message with the status and the time of the last check, and only re-pins it (to notify subscribers) when some bot changes state
- **Separate alert** in Mephisto Ryu's official group when its status changes
- **Availability history in D1:** every state change is recorded with its time
- **Maintenance mode:** `/mantenimiento <bot> on|off` from Telegram, admins only and with the webhook verified by a secret token, or through the authenticated API
- **Public dashboard:** [darkangelbots.dev/estado](https://darkangelbots.dev/estado) queries `/status.json` every minute and shows how many bots are active, in several languages

### Problems I solved
- **Workers KV write limit:** I consolidated the keys into a single JSON object and limited writes to one every 30 minutes per bot, regardless of heartbeat frequency, so the free daily cap isn't exhausted
- **Looping notifications:** the channel message is only re-pinned when some bot changes state, and the new state is saved before touching the pin to avoid a pin/unpin cycle if KV stops accepting writes
- **"Alive" bots that were actually restarting:** the heartbeat alone doesn't reveal a restart loop, so I added a start event with no rate limit
- **False "dead" alerts:** the 45-minute threshold equals two missed heartbeats of the longest interval plus a margin, so one missed heartbeat doesn't mark a bot as down

---

## Vorath
Retro game library inside Telegram. [Try the bot](https://t.me/Vorath_bot)

- Pyrogram (Kurigram) bot whose `/arcade` command opens, with a button, a Telegram mini app with the game library
- Per-user and per-group language, lightweight mode for low-end devices, automatic backups and restart with an update from the chat itself
