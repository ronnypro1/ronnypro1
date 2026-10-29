<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6c5ce7,100:26A5E4&height=180&section=header&text=Ronny&fontSize=60&fontColor=ffffff&animation=fadeIn" />

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&center=true&width=500&lines=Backend+developer;Telegram+bots+in+production;Node.js+%26+Python" />

<img src="https://skillicons.dev/icons?i=python,nodejs,mongodb,postgres,linux,js,html,css" />

**🌐 Language:** [Español](README.md) · English

</div>

Self-taught backend developer based in Guayaquil, Ecuador. I build, deploy and maintain Telegram bots in production: from the code to the server.

> 💼 **Available for remote work** · Python / Node.js · [@darkangeldev](https://t.me/darkangeldev) · [contacto@darkangelbots.dev](mailto:contacto@darkangelbots.dev)

## Stack
- **Python:** Pyrogram, Kurigram, Telethon, PyTgCalls
- **Node.js:** Telegraf
- **Databases:** MongoDB, PostgreSQL
- **AI:** multiple language model providers, voice (TTS) and transcription (Whisper)
- **Infrastructure:** Linux VPS, deployment, backups and monitoring
- **Cloudflare:** Workers, KV, D1 and Cron Triggers
- **Frontend:** HTML, CSS, JavaScript

## Darkangel Bots
A network of Telegram bots covering moderation, music, games and social interaction. I maintain the bots, the servers and the website on my own.

| Bot | What it does | What I did |
|---|---|---|
| [Akami](https://t.me/AkamiXbot) · [Docs](https://darkangelbots.dev/akami/akami) | Group moderation and administration | Async rewrite of [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (MIT): per-group roles, antiflood, federations and an AI chatbot |
| [Bocchi Music](https://t.me/BocchiXmusicbot) · [Docs](https://darkangelbots.dev/bocchi/bocchi) | Music and video in group voice chats | Rewrite of [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (MIT): up to 5 assistant accounts, resource limits for a shared VPS, backups and monitoring |
| [Sayomi Whisper](https://t.me/SayomiXwhisperbot) · [Docs](https://darkangelbots.dev/sayomi/sayomi) | Whispered messages and privacy | Plugin-based architecture, inline mode, Spanish and English |
| [Diosa Moon](https://t.me/Diosamoonbot) · [Docs](https://darkangelbots.dev/diosamoon/diosamoon) | Community with games and birthdays | Quiz-based lineages with rankings, per-timezone birthdays, games that resume after restarts |
| [Moon Confesiones](https://t.me/MoonConfesiones_bot) · [Docs](https://darkangelbots.dev/moonconfesiones/moonconfesiones) | Anonymous confessions with moderation | Admin approval flow, media with quote support, Telegram Stars donations, daily backups |
| [Mephisto Ryu](https://t.me/MephistoRyu_Bot) · [Docs](https://darkangelbots.dev/mephisto/mephisto) | Role-play game with an economy | Economy, 20+ multiplayer games, mini apps, a custom API on a separate server, migration from MongoDB Atlas to a self-hosted server |
| [Vesta](https://t.me/Orusvesta_bot) · [Docs](https://darkangelbots.dev/vesta/vesta) | Daily horoscope and birth chart | 4-language mini app, birth chart with custom astronomical calculations, AI astrologer with server-validated quotas |
| [Puchu](https://t.me/Puchuflusbot) · [Docs](https://darkangelbots.dev/puchu/puchu) | Games and economy | Inherited codebase that I maintain and develop: turn-based games, shop, rankings and World Cup 2026 betting |
| [Vorath](https://t.me/Vorath_bot) | Retro game library | Telegram mini app opened with `/arcade`, lightweight mode for low-end devices, backups and in-chat updates |

> For several bots, the idea and design come from other team members; the programming and the server are mine. I received Puchu already built and I handle its maintenance and improvements.

## Highlights
- **Custom monitoring on Cloudflare** (Workers, KV, D1, Cron) within the free tier: heartbeats, restart-loop detection, availability history and a [public status page](https://darkangelbots.dev/estado)
- **Data consistency:** MongoDB transactions, atomic updates and per-user locks to prevent bets, ads and quotas from being charged twice
- **Mini app security:** HMAC verification of Telegram's `initData` and rewards always decided by the server
- **Resilience:** automatic backups, graceful shutdown and games that resume after a restart
- **AI with fallback:** provider chain with key rotation and automatic failover when one goes down
- **Migration from MongoDB Atlas to a self-hosted server** and a separate API on a private network (Mephisto Ryu)

📄 **[Full technical details and solved problems →](PROYECTOS.en.md)**

<div align="center">

<a href="https://darkangelbots.dev/estado"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fdarkangelbots.dev%2Fstatus.json&query=%24.activos&label=Active%20bots&color=2ecc71&style=for-the-badge&logo=statuspage&logoColor=white" /></a>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/ronnypro1/ronnypro1/output/snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/ronnypro1/ronnypro1/output/snake.svg" />
  <img alt="Animated contribution graph" src="https://raw.githubusercontent.com/ronnypro1/ronnypro1/output/snake.svg" />
</picture>

</div>

> My production code lives in private repositories. If you are a recruiter or evaluating my work, message me and I will give you read access.

## Certifications
- [JavaScript Certification](https://www.freecodecamp.org/certification/rn_ny/javascript-v9), freeCodeCamp (October 2026, approx. 300 hours)

## Contact
<div align="center">

<a href="https://darkangelbots.dev/"><img src="https://img.shields.io/badge/Web-darkangelbots.dev-6c5ce7?style=for-the-badge&logo=googlechrome&logoColor=white" /></a>
<a href="https://t.me/darkangeldev"><img src="https://img.shields.io/badge/Telegram-@darkangeldev-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" /></a>
<a href="mailto:contacto@darkangelbots.dev"><img src="https://img.shields.io/badge/Email-contacto@darkangelbots.dev-ea4335?style=for-the-badge&logo=gmail&logoColor=white" /></a>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=100&section=footer" />

</div>
