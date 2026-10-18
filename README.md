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
| [Bocchi Music](https://t.me/BocchiXmusicbot) | Música y video en llamadas de voz de grupos | Transmisión de audio y video con hasta 5 cuentas asistentes y varios idiomas; límites de recursos en un VPS compartido (un solo núcleo, descargas simultáneas acotadas y espera por usuario), limpieza automática de archivos y respaldos |
| [Sayomi Whisper](https://t.me/SayomiXwhisperbot) | Mensajes susurrados y privacidad | Arquitectura por plugins (modo inline, privacidad, estadísticas), español e inglés, y reporte de estado a mi página |
| [Diosa Moon](https://t.me/Diosamoonbot) | Comunidad con juegos, progreso y cumpleaños | Sistema de linajes con quiz, puntos y rankings; cumpleaños por zona horaria; juegos con temporizadores asíncronos; multilenguaje y reanudación de tareas tras reinicios |
| [Moon Confesiones](https://t.me/MoonConfesiones_bot) | Confesiones anónimas de cara al público, con moderación | Flujo de aprobación en un grupo de admins, publicación de texto y multimedia en un canal con soporte de citas, bloqueo de usuarios, donaciones con Telegram Stars y respaldos diarios |
| [Mephisto Ryu](https://t.me/MephistoRyu_Bot) | Juego de rol con economía y rituales | Economía con monedas, familiares y recompensas por anuncios; migración de MongoDB Atlas a un servidor propio |
| [Vesta](https://t.me/Orusvesta_bot) | Horóscopo diario y carta natal | Miniapp de Telegram en 4 idiomas con horóscopo diario, semanal y mensual, compatibilidad, perfil astral y carta natal con cálculo astronómico propio; astrólogo con IA con cuotas diarias validadas en el servidor; confesiones anónimas y comandos de signos en el bot |
| [Puchu](https://t.me/Puchuflusbot) | Juegos y economía (código recibido; lo mantengo y desarrollo) | Apuestas del Mundial 2026, tienda centralizada y comando de estado con métricas del servidor |

> En varios bots la idea y el diseño son de otras personas del equipo; la programación y el servidor son míos. La excepción es Puchu, que recibí ya creado y del que me encargo del mantenimiento y las mejoras. Los puedes probar directamente desde los enlaces.

## Qué incluye Akami
- **Moderación:** ban, ban temporal, ban con borrado de mensajes, kick, mute, mute temporal, advertencias con límite y sanción configurables, filtros que advierten automáticamente y registro de las acciones que los admins hacen a mano desde Telegram
- **Administración:** promoción y degradación de admins por niveles, títulos y etiquetas, anclado de mensajes, enlaces de invitación y gestión de foto, título y descripción del grupo
- **Protección:** antiflood con ventana de tiempo y cinco modos de sanción, antiraid, anti canal vinculado, lista negra, federaciones de grupos, limpieza de cuentas eliminadas y modo nocturno que cierra y abre el grupo según horario y zona horaria
- **Roles por grupo (diseño propio):** Rey, Reina, Torres, Alfiles, Caballos y Peones, con comandos y límites por rol, sincronizados con las etiquetas nativas de Telegram
- **Comunidad:** bienvenidas y despedidas con multimedia, mute de bienvenida, solicitudes de ingreso, AFK y frases en sticker (con packs por grupo, valoraciones y caché propios, sobre un servicio de renderizado de [QuotLy](https://github.com/LyoSU/quote-bot), licencia MIT)
- **Chatbot con IA:** conversación con memoria por usuario, respaldo automático entre cinco proveedores de modelos con rotación de claves, respuestas en nota de voz y transcripción de notas de voz recibidas, con panel de estado
- **Utilidades:** zip y unzip con cifrado AES-256, texto a mano alzada en imagen y borrado masivo de mensajes
- **Paneles interactivos con botones** para configurar casi todo sin escribir comandos

## Arquitectura de Akami
- **Tres clientes de Telegram en un mismo proceso** (PTB por webhook, Pyrogram y Telethon), con ajuste de conexiones y concurrencia para que ninguno bloquee a los otros
- **Federaciones de grupos** con límite global de peticiones y manejo de esperas por flood, para sancionar en cientos de chats sin ser bloqueado
- **Tareas de limpieza periódicas** para cachés y flujos abandonados, que evitan fugas de memoria
- **Cierre ordenado** ante señales del sistema y **respaldos automáticos** de Postgres y Mongo con copia diaria en la nube

> Akami parte de [FallenRobot](https://github.com/AnonymousX1025/FallenRobot) (AnonymousX1025, licencia MIT). Los roles por grupo, el antiflood, el chatbot con IA, la reescritura de federaciones y la arquitectura asíncrona son trabajo propio.

## Qué incluye Diosa Moon
- **Linajes:** cuatro bandos con un quiz por encuestas, puntos según la actividad, rankings y habilidades de grupo con enfriamiento de 24 horas
- **Juegos:** Stop con rondas y temporizadores, ahorcado, preguntas programadas, lotería y un juego de estrellas con canje de tickets
- **Cumpleaños** con zona horaria por usuario, avisos al grupo y en privado sin duplicados
- **Comunidad:** bienvenidas con aprobación de solicitudes, AFK, frases con panel por grupo y calendario lunar
- **Operación:** respaldos automáticos y latido periódico para detectar reinicios en bucle

## Qué incluye Vesta
- **Miniapp de horóscopo:** hoy, semana y mes para los 12 signos en español, inglés, italiano y francés, con fondo animado y modo ligero que se activa solo en dispositivos de gama baja
- **Horóscopo en caché:** scrapeo de horoscope.com guardado en MongoDB con expiración por período, traducciones cacheadas y precalentado nocturno de todos los signos, para que nadie pague un scrapeo en vivo
- **Astrólogo con IA:** chat con el horóscopo del día como contexto, con cuota diaria por usuario verificada en el servidor
- **Compatibilidad, perfil astral y carta natal:** los prompts se arman en el servidor con datos validados, las respuestas JSON se validan y se comparten por caché entre usuarios
- **Carta natal calculada en el cliente:** posiciones de Sol, Luna y planetas con ecuación de Kepler, ascendente a partir de la ciudad y la hora, y rueda zodiacal en SVG
- **Anuncios recompensados (Monetag):** consultas extra por ver un anuncio, con el premio decidido por el servidor y no por el cliente
- **Bot:** `/vesta` abre la miniapp, 12 comandos de frases por signo con pool sin repetición, `/topsignos` con ranking semanal (grupo o global) y confesiones anónimas con tarjeta generada con Pillow, en modo libre o con aprobación

## Arquitectura de Vesta
- **Bot y API en un mismo proceso:** Pyrogram (Kurigram) y un servidor aiohttp comparten el event loop
- **Autenticación de la miniapp:** verificación HMAC del `initData` firmado de Telegram, con caducidad
- **Cadena de proveedores de IA:** Groq → Gemini → OpenRouter, con rotación de claves, enfriamiento ante límites de uso y salto rápido al siguiente proveedor
- **Operación:** respaldos automáticos cada 30 minutos con copia diaria, heartbeat hacia mi página y evento de arranque para detectar bucles de reinicio

## Problemas técnicos que resolví
- **Condiciones de carrera en apuestas (Puchu):** bloqueos por usuario para evitar apuestas duplicadas y corrección de la lógica de llaves del Mundial
- **Centralización de la tienda (Puchu):** un único catálogo para eliminar errores de sincronización entre menús
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
- **Citas que se perdían al publicar (Moon Confesiones):** al copiar un mensaje que respondía a otro se perdía la cita; reconstruí el contenido con un bloque de cita respetando los límites de caracteres de Telegram, con un respaldo si no cabía
- **Error de bucle de eventos al arrancar (Moon Confesiones):** el cliente fija su event loop al crearse y un segundo loop rompía el inicio; reutilicé el mismo loop en vez de crear uno nuevo
- **Límites de recursos en un VPS compartido (Bocchi):** fijé el proceso a un núcleo, limité las descargas simultáneas, añadí espera por usuario, topes de duración y tamaño, y limpieza automática de archivos temporales
- **Quiz que sobrevive a los reinicios (Diosa Moon):** guardo el progreso en MongoDB con fecha de expiración, así nadie pierde el avance cuando el bot se reinicia
- **Puntos "farmeables" (Diosa Moon):** puse separación mínima entre mensajes, bloqueo temporal ante ráfagas y un tamaño mínimo de grupo para que no se puedan acumular puntos con spam
- **Caché que vencía a las 19:00 (Vesta):** el servidor está en UTC y la caché de "hoy" expiraba a medianoche UTC; pasé el cálculo a hora de Guayaquil
- **Cuotas con peticiones simultáneas (Vesta):** el consumo de mensajes usa una actualización atómica en MongoDB con condición, así que no se puede gastar de más con peticiones en paralelo
- **Mensajes cobrados cuando la IA fallaba (Vesta):** si ningún proveedor responde o el JSON es inválido, se devuelve el uso o la consulta al usuario
- **Anuncios fáciles de falsear (Vesta):** el premio exige un inicio registrado en el servidor, un tiempo mínimo y máximo viendo el anuncio, y un solo reclamo al día por sección; el postback de Monetag es idempotente
- **Secreto expuesto en los logs (Vesta):** el access log imprimía el token del postback; escribí un logger propio que lo enmascara
- **Traducciones bloqueadas (Vesta):** Google Translate bloqueaba la IP del VPS, así que la IA pasó a ser la vía principal, con Google como respaldo y resultado cacheado
- **Inglés guardado como otro idioma (Vesta):** si la traducción fallaba, el cliente cacheaba el texto en inglés como español o francés; ahora el servidor informa el idioma real y el cliente solo cachea si coincide
- **Scrapeos duplicados (Vesta):** un lock por clave hace que solo una petición scrapee y las demás esperen a la caché
- **Respuestas de IA malformadas (Vesta):** extraigo el JSON aunque venga con markdown o texto alrededor y valido tipos, rangos y largos antes de guardarlo
- **`/start` doble con confesiones (Vesta):** el deep-link de confesiones corta la propagación del mensaje para que no llegue también el `/start` con GIF
- **Ranking semanal estable (Vesta):** `/topsignos` se genera con una semilla derivada del ámbito y la semana, y se guarda con inserción condicional para evitar carreras

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
