# Mephisto Ryu

Bot de Telegram de juego de rol con economía, colección de personajes y una red de mini-apps web. Hecho con Pyrogram (Kurigram) y MongoDB, con una API propia en FastAPI que sirve las mini-apps.

> **Estado de este documento:** versión parcial, escrita a partir de una parte del código (arranque, minijuegos, colección y la API unificada). Se irá ampliando.

## Qué incluye

### Economía
- **Monedas y fichas:** dos monedas distintas. Las monedas se ganan jugando y las fichas se obtienen con anuncios, eventos y cambios en la tienda
- **Anuncios recompensados:** Bingo, UNO, Sopa de palabras y Conóceme dan monedas y fichas por ver un anuncio, con un reclamo cada 8 horas por juego
- **Multiplicador x2 de XP:** se activa 3 horas y queda en espera 8 horas antes de poder repetirse

### Husbandos (colección de personajes)
- **Mini-app de colección:** catálogo de personajes agrupados por serie, con fotos servidas desde un proxy propio
- **Sobres y tienda de fichas:** compra de sobres, sobres cósmicos y cambio de monedas por fichas
- **Mercado entre usuarios:** vender, comprar y retirar publicaciones
- **Intercambios y regalos:** propuestas de intercambio con aceptar, responder y cancelar, y regalos directos entre usuarios
- **Equipar y preferencias:** personaje equipado por usuario y foto preferida por personaje
- **Compartir personajes** desde la mini-app hacia los chats
- **Familiares:** 20 familiares organizados en 5 niveles de ataque (de 7 a 20), con un bono de +5 ataques por ver un anuncio cada 6 horas

### Colección Rompecabezas
- **Comando `/coleccion`:** álbum paginado por serie, con navegación continua de personaje en personaje
- **Packs por rareza:** rara, épica, mítico, legendaria y divino, con precios de 150 a 1000 monedas y de 8 a 3 piezas por pack
- **Tablero armado con Pillow:** cada foto es un rompecabezas; las piezas bloqueadas se dibujan con un candado
- **Cupo compartido:** 10 packs por ventana móvil de 24 horas, sin importar la rareza
- **Reembolso por pieza repetida:** de 15 a 220 monedas según la rareza
- **Varias fotos por personaje**, cada una con su propio progreso, y personaje equipable al completar una
- **Datos separados de la lógica:** sumar un personaje es copiar un bloque en un archivo de configuración

### Minijuegos
- **Dados de Telegram (5 monedas por intento):** dardo, fútbol, tragamonedas, bowling y básquet, con 10 intentos por ventana de 24 horas y premios de 20 a 50 monedas
- **`/mephisto_given`:** adivina el número de un dado en el privado; premio de 20 a 35 monedas y 15 % de probabilidad de fichas extra. Límite diario de 100 tiradas para miembros del grupo oficial y 50 para el resto
- **`/mephisto_wager`:** apuesta de 10, 20 o 30 monedas a uno de 9 planetas; si aciertas ganas el décuplo
- **`/signo`:** elección del signo zodiacal con emojis premium, guardado en el idioma del usuario
- **Tests de porcentaje por rol:** psique, LGBTQ+ y BDSM, con resultados por niveles
- **Comandos lúdicos desactivables por grupo**

### Juegos multijugador
- **En chat:** cartas, juicio, cacería, trivia BL, barco, banquete, bingo, sopa de palabras, campamento, blackjack, UNO y dinámica
- **Inline con salas:** ajedrez, damas, tres en raya, conecta 4, othello, póker y bingo
- **Con mini-app:** Bingo BL (cartón y espectador), UNO (cartas con la foto del personaje), Sopa de palabras y Wall Rush

### Mini-apps web
- **Confesiones:** confesiones anónimas con alias, sesión de 3 horas, límite de 1000 caracteres, cooldown de envío, desbloqueo por anuncio de 12 horas y compartir a un grupo
- **Shadow Chat:** chat anónimo uno a uno con cola de espera, sesión de 1 hora, cierre por inactividad, límite de saltos de pareja por hora, reportes con baneo al tercer aviso, reacciones, indicador de escritura, stickers guardados y archivos de hasta 150 MB
- **Chibi:** mascota virtual con alimentar, bañar, dormir, curar y jugar, cada acción con su propio enfriamiento
- **Conóceme:** tests de hasta 20 preguntas que se comparten por enlace

### Operación
- **Respaldos:** dump rotativo cada 30 minutos, envío diario por Telegram y copia diaria en la nube
- **Heartbeat** hacia darkangelbots.dev cada 10 minutos y evento de arranque para detectar bucles de reinicio
- **Difusiones reanudables:** `/mephisto_difusion` y las difusiones a usuarios y grupos continúan donde quedaron tras un reinicio
- **Poda diaria de logs** de cooldowns que solo sirven para el día en curso
- **Multilenguaje** con textos externos por idioma

## Arquitectura

- **Dos servidores:** el bot corre en uno y la API pública en otro, dentro de una misma red privada
- **Bot en polling:** Pyrogram (Kurigram 2.2.25), sin webhook, así que el servidor del bot no necesita estar expuesto
- **API unificada:** un solo proceso FastAPI + uvicorn sirve Husbandos, Confesiones, Shadow Chat, Chibi, Bingo, UNO, Wall Rush, Sopa de palabras y Conóceme. Reemplazó a procesos separados que estaban duplicados
- **Envíos delegados al bot:** cuando una mini-app necesita mandar un mensaje, la API le pide al bot por una ruta interna que lo envíe con su sesión activa
- **MongoDB con Motor** como base de datos única, compartida por el bot y la API
- **APScheduler** con zona horaria de Guayaquil para respaldos, poda de logs y heartbeat
- **Autenticación de las mini-apps:** verificación HMAC-SHA256 del `initData` firmado de Telegram, con caducidad
- **Proxy de imágenes:** las fotos se sirven por la API, así que el token del bot nunca llega al navegador
- **Capas de decoradores** en cada comando: idioma, lista negra de usuarios, bloqueo durante partidas, captura de errores y envíos seguros que absorben errores de Telegram

## Problemas técnicos que resolví

- **Cobros de más en los dados:** en dardo, fútbol, tragamonedas y bowling el cobro se hace después de confirmar que Telegram envió el dado; si el envío falla, no se descuenta nada
- **Doble toque en la apuesta de planetas:** un conjunto de callbacks activos, una marca de apuesta en curso en Mongo y un tiempo máximo de 5 minutos evitan resultados duplicados
- **Apuestas colgadas tras un reinicio:** al arrancar se reinician todas las apuestas en curso
- **Anuncios cobrados dos veces:** el reclamo es una actualización atómica en Mongo que solo prospera si pasó el enfriamiento, así que un doble toque o dos pestañas no acreditan la misma vista dos veces
- **Compras que gastan de más:** los descuentos de monedas usan una operación que solo descuenta si alcanza el saldo
- **`chat_id` falsificable desde el cliente:** el chat de origen se deduce de los campos firmados del `initData`, no de lo que el navegador mande en el cuerpo de la petición
- **500 por `initData` mal formado:** un `auth_date` no numérico ahora responde 401 en lugar de romper con un error interno
- **Mensajes duplicados en Shadow Chat:** un nonce por mensaje con vida de 30 segundos descarta los reintentos, y un candado por destinatario ordena la entrega
- **Entrega inmediata en Shadow Chat:** los mensajes pendientes pasan por un buffer en RAM sin esperar a Mongo, con expiración automática a las 24 horas
- **Cierre del navegador en UNO ("Aw, Snap!"):** el proxy reescala las fotos al ancho que pide la página y las guarda en caché, primero en memoria y luego en disco con tope de 1,5 GB
- **Catálogo demasiado pesado:** la página recibe una copia liviana del catálogo, sin las listas de fotos, que baja de unos 10 MB a unos 1,6 MB
- **Fotos de Telegram desde otro origen:** las URL de imágenes se arman absolutas porque la página vive en otro dominio que la API
- **Partidas perdidas al reiniciar:** el arranque recarga desde Mongo las partidas pendientes de más de veinte juegos y salas inline
- **Eventos de grupo que bloqueaban un cupo:** al iniciar se limpian los eventos de husbandos ya vencidos
- **Difusiones a medias:** quedan registradas en Mongo y se reanudan en segundo plano sin bloquear el arranque
- **Cooldowns que crecían sin fin:** una poda diaria borra las claves de fecha viejas sin tocar ningún contador de ranking
- **Pérdida de eventos del scheduler:** el scheduler se inicia antes de restaurar los eventos guardados

## Stack

Python · Pyrogram (Kurigram) · FastAPI · uvicorn · MongoDB (Motor) · APScheduler · Pillow · HTML/JS para las mini-apps · PM2
