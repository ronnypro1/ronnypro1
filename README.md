<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6c5ce7,100:26A5E4&height=180&section=header&text=Ronny&fontSize=60&fontColor=ffffff&animation=fadeIn" />

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&center=true&width=500&lines=Backend+developer;Telegram+bots+en+producci%C3%B3n;Node.js+%26+Python" />

<img src="https://skillicons.dev/icons?i=python,nodejs,mongodb,postgres,linux,js,html,css" />

**🌐 Idioma:** Español · [English](README.en.md)

</div>

Backend developer autodidacta en Guayaquil, Ecuador. Programo, despliego y mantengo bots de Telegram en producción: desde el código hasta el servidor.

## Stack
- **Python:** Pyrogram, Kurigram, Telethon, PyTgCalls
- **Node.js:** Telegraf
- **Bases de datos:** MongoDB, PostgreSQL
- **IA:** varios proveedores de modelos de lenguaje, voz (TTS) y transcripción (Whisper)
- **Infraestructura:** VPS Linux, despliegue, respaldos y monitoreo
- **Cloudflare:** Workers, KV, D1 y Cron Triggers
- **Frontend:** HTML, CSS, JavaScript

## Darkangel Bots
Red de bots de Telegram con moderación, música, juegos e interacción social. Mantengo yo solo los bots, los servidores y el sitio web.

| Bot | Qué hace | Lo que hice |
|---|---|---|
| [Akami](https://t.me/AkamiXbot) · [Docs](https://darkangelbots.dev/akami/akami) | Moderación y administración de grupos | Reescritura asíncrona de [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (MIT): roles por grupo, antiflood, federaciones y chatbot con IA |
| [Bocchi Music](https://t.me/BocchiXmusicbot) · [Docs](https://darkangelbots.dev/bocchi/bocchi) | Música y video en llamadas de voz | Reescritura de [AnonXMusic](https://github.com/AnonymousX1025/AnonXMusic) (MIT): hasta 5 asistentes, límites de recursos para un VPS compartido, respaldos y monitoreo |
| [Sayomi Whisper](https://t.me/SayomiXwhisperbot) · [Docs](https://darkangelbots.dev/sayomi/sayomi) | Mensajes susurrados y privacidad | Arquitectura por plugins, modo inline, español e inglés |
| [Diosa Moon](https://t.me/Diosamoonbot) · [Docs](https://darkangelbots.dev/diosamoon/diosamoon) | Comunidad con juegos y cumpleaños | Linajes con quiz y rankings, cumpleaños por zona horaria, juegos que se reanudan tras reinicios |
| [Moon Confesiones](https://t.me/MoonConfesiones_bot) · [Docs](https://darkangelbots.dev/moonconfesiones/moonconfesiones) | Confesiones anónimas con moderación | Aprobación por admins, multimedia con citas, donaciones con Telegram Stars, respaldos diarios |
| [Mephisto Ryu](https://t.me/MephistoRyu_Bot) · [Docs](https://darkangelbots.dev/mephisto/mephisto) | Juego de rol con economía | Economía, +20 juegos multijugador, miniapps, API propia en servidor aparte, migración de MongoDB Atlas a servidor propio |
| [Vesta](https://t.me/Orusvesta_bot) · [Docs](https://darkangelbots.dev/vesta/vesta) | Horóscopo diario y carta natal | Miniapp en 4 idiomas, carta natal con cálculo astronómico propio, astrólogo con IA con cuotas validadas en el servidor |
| [Puchu](https://t.me/Puchuflusbot) · [Docs](https://darkangelbots.dev/puchu/puchu) | Juegos y economía | Código recibido; lo mantengo y desarrollo: juegos por turnos, tienda, rankings y apuestas del Mundial 2026 |
| [Vorath](https://t.me/Vorath_bot) | Biblioteca de juegos retro | Miniapp de Telegram con `/arcade`, modo ligero para gama baja, respaldos y actualización desde el chat |

> En varios bots la idea y el diseño son de otras personas del equipo; la programación y el servidor son míos. Puchu lo recibí ya creado y me encargo de su mantenimiento y mejoras.

## Lo más destacado
- **Monitoreo propio en Cloudflare** (Workers, KV, D1, Cron) dentro del plan gratuito: heartbeat, detección de bucles de reinicio, historial de disponibilidad y [panel público](https://darkangelbots.dev/estado)
- **Datos consistentes:** transacciones en MongoDB, actualizaciones atómicas y bloqueos por usuario para evitar apuestas, anuncios y cuotas cobrados dos veces
- **Seguridad en miniapps:** verificación HMAC del `initData` de Telegram y premios decididos siempre por el servidor
- **Resiliencia:** respaldos automáticos, cierre ordenado y partidas que se retoman tras un reinicio
- **IA con respaldo:** cadena de proveedores con rotación de claves y salto automático cuando uno falla
- **Migración de MongoDB Atlas a un servidor propio** y API separada en una red privada (Mephisto Ryu)

📄 **[Ver detalle técnico completo y problemas resueltos →](PROYECTOS.md)**

<div align="center">

<a href="https://darkangelbots.dev/estado"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fdarkangelbots.dev%2Fstatus.json&query=%24.activos&label=Bots%20activos&color=2ecc71&style=for-the-badge&logo=statuspage&logoColor=white" /></a>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/ronnypro1/ronnypro1/output/snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/ronnypro1/ronnypro1/output/snake.svg" />
  <img alt="Gráfico de contribuciones animado" src="https://raw.githubusercontent.com/ronnypro1/ronnypro1/output/snake.svg" />
</picture>

</div>

> Mi código de producción está en repos privados. Si eres reclutador o estás evaluando mi trabajo, escríbeme y te doy acceso de lectura.

## Certificaciones
<p>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/responsive-web-design-v9">
    <img src="https://img.shields.io/badge/freeCodeCamp-Dise%C3%B1o%20Web%20Adaptable-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de Diseño web adaptable en freeCodeCamp" />
  </a>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/javascript-v9">
    <img src="https://img.shields.io/badge/freeCodeCamp-JavaScript-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de JavaScript en freeCodeCamp" />
  </a>
</p>
<p>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/relational-databases-v9">
    <img src="https://img.shields.io/badge/freeCodeCamp-Bases%20de%20Datos%20Relacionales-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de Bases de Datos Relacionales en freeCodeCamp" />
  </a>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/back-end-development-and-apis-v9">
    <img src="https://img.shields.io/badge/freeCodeCamp-Desarrollo%20Back--end%20y%20APIs-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de Desarrollo back-end y APIs en freeCodeCamp" />
  </a>
</p>
<p>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/python-v9">
    <img src="https://img.shields.io/badge/freeCodeCamp-Python-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de Python en freeCodeCamp" />
  </a>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/front-end-development-libraries-v9">
    <img src="https://img.shields.io/badge/freeCodeCamp-Bibliotecas%20Front--end-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de Bibliotecas de desarrollo front-end en freeCodeCamp" />
  </a>
</p>
<p>
  <a href="https://www.freecodecamp.org/espanol/certification/rn_ny/foundational-c-sharp-with-microsoft">
    <img src="https://img.shields.io/badge/freeCodeCamp-Fundamentos%20de%20C%23%20con%20Microsoft-0a0a23?style=for-the-badge&logo=freecodecamp&logoColor=white" alt="Certificación de Fundamentos de C# con Microsoft en freeCodeCamp" />
  </a>
</p>

### MongoDB
<p>
  <a href="https://www.credly.com/badges/0c81b444-4779-4476-ac05-e5d35d39dc4b/public_url">
    <img src="https://img.shields.io/badge/MongoDB-CRUD%20Operations-00684A?style=for-the-badge&logo=mongodb&logoColor=white" alt="Insignia CRUD Operations in MongoDB" />
  </a>
  <a href="https://www.credly.com/badges/7ac4082c-173a-44bb-a0f2-c031af288f77/public_url">
<img src="https://img.shields.io/badge/MongoDB-Relational%20to%20Document%20Model-00684A?style=for-the-badge&logo=mongodb&logoColor=white" alt="Insignia From Relational Model (SQL) to MongoDB's Document Model" />
<p>
  <a href="https://www.credly.com/badges/0222ca23-4923-4c25-ac6f-367ed5bdc5d3/public_url">
    <img src="https://img.shields.io/badge/MongoDB-Advanced%20Schema%20Design-00684A?style=for-the-badge&logo=mongodb&logoColor=white" alt="Insignia MongoDB Advanced Schema Design Patterns and Anti-patterns" />
  </a>
  <a href="https://www.credly.com/badges/8713736b-f092-4dfb-9ee3-a6cc08f4ab51/public_url">
    <img src="https://img.shields.io/badge/MongoDB-Aggregation%20Fundamentals-00684A?style=for-the-badge&logo=mongodb&logoColor=white" alt="Insignia MongoDB Aggregation Fundamentals" />
  </a>
</p>

## Contacto
<div align="center">

<a href="https://darkangelbots.dev/"><img src="https://img.shields.io/badge/Web-darkangelbots.dev-6c5ce7?style=for-the-badge&logo=googlechrome&logoColor=white" /></a>
<a href="https://t.me/darkangeldev"><img src="https://img.shields.io/badge/Telegram-@darkangeldev-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" /></a>
<a href="mailto:contacto@darkangelbots.dev"><img src="https://img.shields.io/badge/Email-contacto@darkangelbots.dev-ea4335?style=for-the-badge&logo=gmail&logoColor=white" /></a>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=100&section=footer" />

</div>
