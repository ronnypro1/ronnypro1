<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=180&section=header&text=Ronny&fontSize=60" />

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&center=true&width=500&lines=Backend+developer;Telegram+bots+en+producci%C3%B3n;Node.js+%26+Python" />

<img src="https://skillicons.dev/icons?i=python,nodejs,mongodb,postgres,linux,js,html,css" />

<br><br>

</div>

Backend developer autodidacta en Guayaquil, Ecuador. Programo, despliego y mantengo bots de Telegram en producción: desde el código hasta el servidor.

## Stack
- **Python:** Pyrogram, Kurigram, Telethon, PyTgCalls
- **Node.js:** Telegraf
- **Bases de datos:** MongoDB, PostgreSQL
- **IA:** integración de varios proveedores de modelos de lenguaje, voz (TTS) y transcripción (Whisper)
- **Infraestructura:** VPS Linux, despliegue y monitoreo de bots
- **Frontend:** HTML, CSS, JavaScript

## Darkangel Bots
Red de bots de Telegram con moderación, música, juegos e interacción social. Mantengo yo solo los bots, los servidores y el sitio web.

| Bot | Qué hace | Lo que hice |
|---|---|---|
| [Akami](https://t.me/AkamiXbot) | Administración y moderación de grupos | Parto de [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (licencia MIT) y reescribí gran parte a arquitectura asíncrona. Moderación completa, roles por grupo, antiflood, federaciones, modo nocturno, chatbot con IA y más (ver abajo) |
| [Penny](https://t.me/PennyPirate_bot) | Administración de grupos con temática pirata | (completar) |
| [Akami Music](https://t.me/AkamiXmusicbot) | Música y video en grupos y canales | (completar) |
| [Bocchi Music](https://t.me/BocchiXmusicbot) | Música y videostream | Pyrogram/Kurigram con PyTgCalls, yt-dlp y ffmpeg, hasta 5 cuentas asistentes y varios idiomas; optimización de CPU en un VPS de 1 núcleo |
| [Sayomi Whisper](https://t.me/SayomiXwhisperbot) | Mensajes susurrados y privacidad | Arquitectura por plugins (modo inline, privacidad, estadísticas), español e inglés, y reporte de estado a mi página |
| [Diosa Moon](https://t.me/Diosamoonbot) | Cumpleaños, bienvenidas, juegos, clima y calendario lunar | (completar) |
| [Moon Confesiones](https://t.me/MoonConfesiones_bot) | Confesiones anónimas y registro de cumpleaños | (completar) |
| [Mephisto Ryu](https://t.me/MephistoRyu_Bot) | Juego de rol con economía y rituales | Economía con monedas, familiares y recompensas por anuncios; migración de MongoDB Atlas a un servidor propio |
| [Vesta](https://t.me/Orusvesta_bot) | Horóscopo diario y carta natal | (completar) |
| [Puchu](https://t.me/Puchuflusbot) | Juegos y economía (código recibido; lo mantengo y desarrollo) | Apuestas del Mundial 2026, tienda centralizada y comando de estado con métricas del servidor |

> En varios bots la idea y el diseño son de otras personas del equipo; la programación y el servidor son míos. La excepción es Puchu, que recibí ya creado y del que me encargo del mantenimiento y las mejoras. Los puedes probar directamente desde los enlaces.

## Qué incluye Akami
- **Moderación:** ban, ban temporal, ban con borrado de mensajes, kick, mute, mute temporal, advertencias con límite y sanción configurables, filtros que advierten automáticamente y registro de las acciones que los admins hacen a mano desde Telegram
- **Administración:** promoción y degradación de admins por niveles, títulos y etiquetas, anclado de mensajes, enlaces de invitación y gestión de foto, título y descripción del grupo
- **Protección:** antiflood con ventana de tiempo y cinco modos de sanción, antiraid, anti canal vinculado, lista negra, federaciones de grupos, limpieza de cuentas eliminadas y modo nocturno que cierra y abre el grupo según horario y zona horaria
- **Roles por grupo (diseño propio):** Rey, Reina, Torres, Alfiles, Caballos y Peones, con comandos y límites por rol, sincronizados con las etiquetas nativas de Telegram
- **Comunidad:** bienvenidas y despedidas con multimedia, mute de bienvenida, solicitudes de ingreso, AFK y frases en sticker
- **Chatbot con IA:** conversación con memoria por usuario, respaldo automático entre cinco proveedores de modelos con rotación de claves, respuestas en nota de voz y transcripción de notas de voz recibidas, con panel de estado
- **Utilidades:** zip y unzip con cifrado AES-256, texto a mano alzada en imagen y borrado masivo de mensajes
- **Paneles interactivos con botones** para configurar casi todo sin escribir comandos

## Arquitectura de Akami
- **Tres clientes de Telegram en un mismo proceso** (PTB por webhook, Pyrogram y Telethon), con ajuste de conexiones y concurrencia para que ninguno bloquee a los otros
- **Federaciones de grupos** con límite global de peticiones y manejo de esperas por flood, para sancionar en cientos de chats sin ser bloqueado
- **Tareas de limpieza periódicas** para cachés y flujos abandonados, que evitan fugas de memoria
- **Cierre ordenado** ante señales del sistema y **respaldos automáticos** de Postgres y Mongo con copia diaria en la nube

> Akami parte de [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (AnonymousX1025, licencia MIT). Los roles por grupo, el antiflood, el chatbot con IA, la reescritura de federaciones y la arquitectura asíncrona son trabajo propio.

## Problemas técnicos que resolví
- **Condiciones de carrera en apuestas (Puchu):** bloqueos por usuario para evitar apuestas duplicadas y corrección de la lógica de llaves del Mundial
- **Actualizaciones repetidas tras un reinicio (Akami):** el bot reprocesaba mensajes viejos; lo resolví con un handler de prioridad máxima y deduplicación con estructuras de memoria
- **Bans permanentes por error (Akami):** en el antiflood con sanción temporal, si el mensaje ya estaba borrado el bot no leía la duración y baneaba para siempre; ahora usa el valor guardado y tiene un respaldo seguro
- **Contador de flood corrompido (Akami):** los mensajes en cola de un usuario ya silenciado borraban el progreso de otros; ahora los ignora con una caché de miembros que se invalida al sancionar
- **Borrado masivo sin leer el historial (Akami):** los bots no pueden leer mensajes antiguos, así que borro por rango de identificadores; si un lote falla por mensajes de servicio, lo divido a la mitad hasta aislar el problemático, y retomo tras las esperas por flood
- **Modo nocturno que se corrige solo (Akami):** comparo el horario por rango, así que tras un reinicio el grupo recupera su estado correcto en menos de un minuto
- **Caída de proveedores de IA (Akami):** el chatbot pasa automáticamente al siguiente proveedor y rota claves y modelos cuando uno falla
- **Spam de logs al configurar (Akami):** cada clic en el panel generaba un aviso; agregué una espera de 5 segundos para enviar un solo registro con el valor final
- **Logs que desaparecían (Mephisto Ryu):** un logger mal configurado tragaba todos los mensajes informativos; lo encontré y arreglé
- **Datos inconsistentes en MongoDB (Mephisto Ryu):** detecté que algunos usuarios se guardaban con otro tipo de identificador y quedaban invisibles para el resto del bot
- **Límite de Telegram en botones (Mephisto Ryu):** un diccionario completo se serializaba dentro de los datos del botón y superaba el límite de 64 bytes
- **Centralización de la tienda (Puchu):** un único catálogo para eliminar errores de sincronización entre menús

## Otros proyectos
- **Arcade retro:** arcade en el navegador con emuladores (EmulatorJS)

> Mi código de producción está en repos privados. Con gusto doy acceso de lectura para revisión si lo necesitas.

## Aprendiendo ahora
Certificación de JavaScript en freeCodeCamp.

## Contacto
<div align="center">

<a href="https://darkangelbots.dev/"><img src="https://img.shields.io/badge/Web-darkangelbots.dev-6c5ce7?style=for-the-badge&logo=googlechrome&logoColor=white" /></a>
<a href="https://t.me/darkangeldev"><img src="https://img.shields.io/badge/Telegram-@darkangeldev-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" /></a>
<a href="mailto:contacto@darkangelbots.dev"><img src="https://img.shields.io/badge/Email-contacto@darkangelbots.dev-ea4335?style=for-the-badge&logo=gmail&logoColor=white" /></a>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=100&section=footer" />

</div>
