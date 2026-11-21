# MagicBuyer-UT — sniper para la web app de EA FC 27

Script de Tampermonkey que añade un sniper / autobuyer a la web app de **EA SPORTS FC 27 Ultimate Team**:
búsqueda en bucle en el mercado de transferibles, compra inmediata de cartas por debajo de tu precio máximo (fijo o
a un % del precio de FUTBIN seguido en directo), puesta en venta automática al precio de FUTBIN, precio de FUTBIN en cada
carta, importación de soluciones de FUTBIN en los DCP con compra de los jugadores faltantes, pausas y detenciones configurables.
También añade herramientas (atajos de teclado, contador de peticiones a EA, gangas, precio mín. de EA, club, sobres)
y un planificador para la **Galería** de FC 27 (colecciones, niveles, bonificaciones, compra de los jugadores faltantes).

> ⚠️ **Leer antes de usar.** La automatización de la web app es contraria a los términos de servicio de EA.
> EA puede restringir el acceso al mercado de transferibles (soft ban), pedir captchas o incluso suspender la cuenta.
> Usa este script bajo tu propia responsabilidad; los autores no se hacen responsables de ninguna sanción en tu cuenta.

## Instalación

1. Instala [Tampermonkey](https://www.tampermonkey.net/) en Chrome, Edge o Brave.
   En versiones recientes de Chrome, activa **«Permitir scripts de usuario»** en los detalles de la extensión Tampermonkey.
2. Abre `fut-auto-buyer.user.js` desde la [última release](https://github.com/AMINE1921/MagicBuyer-UT/releases/latest) y haz clic en **Instalar**.
3. Abre la [web app de FC 27](https://www.ea.com/ea-sports-fc/ultimate-team/web-app/) e inicia sesión.

El mercado de transferibles debe estar desbloqueado en tu cuenta.

## Inicio rápido

1. Haz clic en la pestaña **MagicBuyer** (barra de navegación de EA) o en el botón flotante **MB** abajo a la derecha.
2. Pestaña **Objetivo**: escribe el nombre del jugador y selecciónalo de la lista.
3. Introduce el **Precio máximo de compra** (compra ya) y, si quieres revender, el **Precio de reventa**
   (el panel muestra el neto tras el impuesto del 5 % de EA y el beneficio por carta).
4. Haz clic en **Probar búsqueda (sin comprar)** para ver qué devuelve el mercado (en verde: lo que el bot compraría).
5. Haz clic en **▶ Iniciar**. Puedes cerrar el panel: el botón flotante muestra el estado, las búsquedas y las compras.

Otro método: configura tu búsqueda en **Traspasos → Mercado de transferibles** de EA (rareza, posición, estilo de juego…)
y haz clic en **⚡ Snipear esta búsqueda**: se creará un filtro con exactamente esos criterios.

## Precios de FUTBIN

Todas las funciones de precios utilizan **FUTBIN** (páginas leídas como en tu navegador, plataforma de consola o PC
según tu cuenta). Pestaña **FUTBIN**: botón **Probar FUTBIN** para verificar el acceso.

- **Compra al % del precio de FUTBIN** (pestaña Objetivo → Precio de compra → Modo): por ejemplo, 90 %; el precio máximo de compra
  se recalcula en cada actualización del precio. Un precio fijo puede servir como tope absoluto. Funciona para una versión
  específica, para todas las versiones de un jugador o para todos los jugadores de un filtro (lista de FUTBIN): cada carta
  tiene su propio precio máximo.
  Sin un precio de FUTBIN reciente (menos de 5 min), el filtro espera: nunca compra con un precio desactualizado.
- **Reventa al % del precio de FUTBIN** (pestaña Venta, o por filtro): el precio de la versión comprada se vuelve a leer justo
  después de la compra. Sin precio de FUTBIN, la carta va a la lista de transferibles sin ponerse en venta.
- **Relistar al precio de FUTBIN** y botón **Listar al precio de FUTBIN** (pestaña Traspasos) para las cartas disponibles y no vendidas.
- **Nunca con pérdidas** (pestaña Venta, activado por defecto): si el precio de FUTBIN baja justo después de la compra, la carta se pone
  en venta en el umbral de rentabilidad (precio pagado + impuesto del 5 % de EA + beneficio mínimo), nunca por debajo.
- **Antipérdidas antes de la compra** (pestaña Compra, activado por defecto): el bot no compra (ni puja) si la reventa
  prevista no deja el beneficio mínimo tras el impuesto de EA. Reventa prevista = precio fijo, o precio de FUTBIN × parte baja del rango
  de venta, ajustada justo por debajo de un anuncio más barato de la misma carta visto en la búsqueda. Una configuración que pierde
  seguro (ej. compra al 95 % y venta al 99 % del precio de FUTBIN) se avisa al iniciar.
- **Compra ya comparado con FUTBIN**: debajo del botón «Comprar ya» de un anuncio, precio de FUTBIN, % pagado y
  beneficio (verde) o pérdida (rojo) de una reventa al precio de FUTBIN tras impuestos. La tecla de compra inmediata (B) se bloquea
  cuando la compra haría perder monedas (el clic con el ratón sigue siendo posible).
- **Etiqueta de precio** en la parte superior de cada carta de jugador (club, mercado, traspasos, plantillas, DCP); clic = página de FUTBIN.
- **Panel «Poner en mercado» de EA**: precio de FUTBIN de la carta y botón **Rellenar** (tú confirmas con el botón de EA).
  Botón **Precio mín. EA**: unas pocas búsquedas exactas encuentran el anuncio más barato del momento y luego **Rellenar**
  propone un escalón por debajo.
- **Puntuación de artículo** (puntos de galería, insignia de gema) junto al precio de FUTBIN en cada carta, leído en FUTBIN junto con el precio.
- **Gangas**: en el mercado, un anuncio por debajo del X % del precio de FUTBIN (90 % por defecto) se resalta en verde
  con la diferencia y el beneficio estimado tras impuestos. Solo en tus cartas (club, traspasos): **precio pagado** y
  beneficio estimado.

Actualización inteligente: los objetivos del bot y las compras de DCP se vuelven a leer cada 60 a 120 s, las cartas
mostradas cada ~2 min (con menos frecuencia si el precio no se mueve), una sola petición a FUTBIN a la vez, espaciadas,
ralentización automática si FUTBIN bloquea, y un salto de precio anormal se verifica una segunda vez antes de usarse.
FUTBIN no envía sus precios automáticamente: se vuelven a leer regularmente mientras la carta esté en seguimiento.

Si FUTBIN devuelve una verificación de Cloudflare, abre futbin.com en una pestaña y pasa la verificación;
el respaldo «página de FUTBIN oculta» (iframe invisible) toma el relevo cuando la petición directa es rechazada.

## DCP: soluciones de FUTBIN

1. Abre la plantilla de un desafío (pantalla con el campo) y haz clic en **⚡ Solución FUTBIN** en la parte superior de la pantalla.
2. Pega el enlace de FUTBIN de la solución (página de la plantilla) y pulsa **Cargar**: formación, 11 jugadores, los de tu club
   (y del almacén de DCP, cesiones excluidas, intransferibles con prioridad) y los que **faltan por comprar** con su precio de FUTBIN.
3. **Colocar en la plantilla**: se aplica la formación y los jugadores se colocan en una posición en la que puedan jugar.
4. Revisa o modifica el **precio máximo** de cada jugador faltante (precio de FUTBIN + margen ajustable) y pulsa **Comprar faltantes**:
   búsqueda exacta de la versión, la más barata primero, compra, envío al club y colocación en el desafío.

El desafío nunca se envía automáticamente: eres tú quien hace clic en «Enviar». Captcha, sesión expirada o
limitación de EA detienen la compra inmediatamente; el botón **Detener** la para en cualquier momento.

- **DCP repetibles**: la solución utilizada se guarda por desafío. Al volver a abrir el desafío, **⚡ Solución FUTBIN** la vuelve a leer
  (cartas del club detectadas); **Colocar en la plantilla**, **Comprar faltantes** y luego enviar.
- **Valor de FUTBIN de la plantilla** del desafío mostrado debajo del botón (suma de los precios de los jugadores colocados).
- **Colecciones ocultas**: botón × en una colección de la lista de DCP; se puede volver a mostrar desde la pestaña Herramientas.
- **Después del envío**: botón **Ordenar sin asignar** (duplicados intransferibles → almacén de DCP, el resto al club).
- **Alerta** cuando EA rechaza los DCP (demasiadas peticiones, verificación, bloqueo temporal): la tarea en curso se detiene.

## Galería (FC 27)

Mosaico **Galería** en el inicio de la web app, junto a «FC Hub» y con el mismo formato: abre pantallas de la web app
(título y botón de volver de EA). Datos y precios: **FUTBIN**.

1. **Categorías** (logos de las ligas, fichas de galería por ganar) y luego **colecciones**: barra de niveles D a S
   (puntos necesarios, recompensas al pasar el ratón), tus cartas y tu puntuación en las colecciones ya abiertas.
   Los jugadores se muestran con las **cartas reales de la web app de EA** (pestañas Lista lista / Faltantes / Tuyos).
2. Al abrir una colección, todo se hace solo: jugadores elegibles leídos en FUTBIN, tus cartas detectadas (club,
   almacén de DCP, cartas ya coleccionadas) y luego **lista preparada**: los jugadores más baratos al precio de FUTBIN para
   alcanzar el siguiente nivel, tus cartas primero. La puntuación sigue el cálculo de FUTBIN (puntos + bonificaciones: ataque,
   mismo club, naciones diferentes, primer propietario…).
3. **Comprar N jugadores** (tras confirmación): cada jugador faltante se compra al precio de FUTBIN + margen de DCP y se envía al club.
   **Comprar + revender al mismo precio**: tras todas las compras, cada carta se vuelve a poner en venta a su precio de compra
   (venta masiva; impuesto del 5 % de EA en la venta). Botón **+ Comprar** en cada jugador, **+ / −** para modificar la lista,
   elección del nivel objetivo.

La web app no muestra la galería: una carta cuenta como «coleccionada» cuando se ve en tu club / almacén
o cuando la compra esta herramienta (historial local), y pasa a ser gratuita en los planes siguientes.

## Herramientas

Pestaña **Herramientas**:

- **Contador de peticiones a EA**: búsquedas del bot y manuales en la última hora y las últimas 24 h,
  compras / pujas. Alerta al 90 % y 100 % de los límites (900/h y 4 500/24 h por defecto); **pausa automática** opcional:
  el bot espera a volver a estar por debajo del límite, las compras de DCP / galería y el precio mín. de EA se detienen.
- **Atajos de teclado** (teclas modificables, mostradas en los botones de EA): Intro = buscar, B = compra inmediata
  (luego Intro confirma esa compra; una confirmación abierta con el ratón, como una venta rápida, nunca se confirma
  con el teclado), N = pujar, L = poner en mercado, Retroceso = volver, ↑ ↓ = carta anterior / siguiente,
  ← → = página, + / − y Re Pág / Av Pág = precio mín. / máx., Esc = cancelar, M = abrir MagicBuyer.
  Inactivos mientras se escribe en un campo de texto.
- **Club**: exportación CSV (media, posición, transferible, primer propietario, precio pagado, precio de FUTBIN, puntuación de artículo, venta rápida;
  precios de FUTBIN faltantes leídos opcionalmente), envío masivo a la lista de transferibles por media / precio de FUTBIN, almacén de DCP → club
  (cartas que ya no son duplicados), venta rápida de intransferibles en un rango de medias. Vista previa (con los nombres)
  y confirmación antes de cada acción; si no se puede leer la plantilla activa, no se hace nada: sus jugadores nunca se tocan.
- **Sobres**: apertura de los sobres que ya tienes («Mis sobres», sin compras) con ordenación automática tras cada sobre:
  duplicados intransferibles → almacén de DCP, duplicados transferibles y transferibles que valgan al menos X en FUTBIN → lista
  de transferibles, venta rápida opcional de intransferibles hasta una media, el resto al club (nunca un duplicado).
  Si queda alguna carta en sin asignar, la apertura se detiene. Valor de FUTBIN de cada sobre en el registro.

Estas herramientas nunca se ejecutan al mismo tiempo que el bot ni que otra tarea (una sola secuencia de peticiones a EA a la vez).

## Cómo evita el sniper perder una ganga

- **Resultados frescos en cada búsqueda**: la caché de la web app se vacía y cada petición es diferente
  (anticaché «automático»: la puja máx. varía *por encima* de tu precio máximo de compra, por lo que no se excluye ningún anuncio comprable).
- **Compra inmediata** en cuanto llega la respuesta de EA, sin esperar a FUTBIN ni a ninguna otra petición.
- **La más barata primero**, luego la más reciente a igualdad de precio; tus propios anuncios se ignoran.
- **Reventa después de la compra**, nunca durante: la siguiente búsqueda no se retrasa.
- **Pestaña activa en segundo plano**: Chrome ralentiza las pestañas ocultas; la opción «Mantener la pestaña activa» lo impide
  (aparece un icono de altavoz en la pestaña, no se emite ningún sonido).

## Ajustes

| Pestaña | Ajustes principales |
| --- | --- |
| **Objetivo** | Filtros guardados, rotación entre filtros, jugador, calidad, posición, media mín./máx., precio de compra (fijo o % FUTBIN en directo, tope), reventa (pestaña Venta, fijo o % FUTBIN), puja máx., IDs avanzados (versión exacta, rareza, nación, liga, club, estilo). |
| **Compra** | Compras máx. por búsqueda, detener tras N compras, reserva de monedas, umbral de resultados, ignorar porteros, pujas (ventana final, sobrepuja, pujas activas máx.). |
| **Venta** | Puesta en venta automática / envío a lista de transferibles / nada, precio fijo o % del precio de FUTBIN, duración, beneficio mínimo. |
| **Timing** | Perfiles Prudente / Normal / Rápido, tiempo entre búsquedas, búsquedas máx. por minuto, pausa cada N búsquedas, duración de pausa, detención automática, retraso tras compra, anticaché, páginas recorridas, pausa de seguridad por limitación de EA. |
| **Traspasos** | Estado de la lista, relistar no vendidos (mismo precio o precio de FUTBIN), puesta en venta masiva al precio de FUTBIN, limpiar vendidos, detener si la lista está llena. |
| **FUTBIN** | Prueba de acceso, plataforma de precios, frecuencia de actualización, protección contra saltos de precio, etiquetas en las cartas, ajustes de compras de DCP. |
| **Herramientas** | Contador de peticiones a EA y pausa auto, gangas, precio pagado, puntuación de artículo, precio mín. EA, atajos de teclado, club, sobres, DCP (valor, alerta, colecciones ocultas). |
| **Alertas** | Idioma de la interfaz, sonidos, notificaciones del navegador, webhook de Discord, bot de Telegram, elección de eventos notificados. |

Formatos aceptados para las duraciones: `5-9` (segundos), `4.5-7`, `40-80S`, `5M`, `1-2H`, `1D`.
Todos los valores se guardan automáticamente (almacenamiento de Tampermonkey).

### Perfiles de timing

| Perfil | Entre dos búsquedas | Máx. / minuto | Pausa | Detención auto |
| --- | --- | --- | --- | --- |
| Prudente | 8 a 14 s | 6 | 60–120 s cada 12–18 búsquedas | 1–2 h |
| Normal (por defecto) | 5 a 9 s | 10 | 40–80 s cada 15–25 búsquedas | 2–3 h |
| Rápido | 3 a 5 s | 15 | 30–60 s cada 20–30 búsquedas | 1–1,5 h |

Cuanto más rápido es el ritmo, antes activa EA captchas y limitaciones.

### Seguridad

- **Captcha (458)**, **sesión expirada (401)**, **mercado bloqueado (494)**, **cuenta bloqueada**: detención inmediata + alerta.
- **Demasiadas peticiones (429) / bloqueo temporal (512, 521)**: pausa de seguridad (4 a 8 min por defecto), detención si se repite.
- **3 búsquedas fallidas seguidas**: detención.
- Códigos de error personalizados: pestaña Timing → Errores de EA.

El script nunca resuelve captchas: resuélvelo tú mismo en la web app y luego reinicia.

## Compilar desde las fuentes

```bash
npm install
npm run build:prod
```

El script se genera en `dist/fut-auto-buyer.user.js` (cabecera de Tampermonkey en `tampermonkey-header.js`).

## Novedades 5.4.6

- Galería (y otras páginas de futbin.com): FUTBIN rechaza sus páginas a las peticiones del script (403, verificación de
  Cloudflare) y el iframe oculto ya no funciona (Chrome no envía las cookies del sitio allí). El script abre entonces
  una pestaña de futbin.com en segundo plano y realiza allí la petición como tu navegador, recuperando la respuesta mediante el
  almacenamiento de Tampermonkey: galería, colecciones, búsqueda de jugadores de una colección, páginas de jugador o listas de
  respaldo. Una petición a la vez, la pestaña se cierra sola tras 5 min sin uso. Ajuste: pestaña FUTBIN →
  «Respaldo: pestaña FUTBIN». Tampermonkey pedirá permiso para abrir pestañas al actualizar.

## Novedades 5.4.5

- Búsqueda de jugador (pestaña Objetivo) mediante la API de la app de FUTBIN: todas las versiones en ~0,2 s, en lugar de esperar
  a la búsqueda de futbin.com que FUTBIN rechaza al script (hasta 20 s bloqueado). Versiones especiales nombradas por EA,
  cartas holo señaladas. Los filtros «jugador, todas las versiones» encuentran sus versiones de la misma manera.
- Pestaña Objetivo: Tipo (todas las rarezas del juego), Holo (solo holo / sin holo), Estilo de química, Nación,
  Liga y Club se eligen en listas con los nombres del juego, junto a Calidad y Posición: ya no hace falta escribir ningún
  identificador. Los clubes siguen a la liga elegida; dos clubes con el mismo nombre (equipos masculino y
  femenino) llevan su liga entre paréntesis. La lista de filtros muestra estos nombres (ej. «estilo Sombra»).

## Novedades 5.4.4

- Solucionador de DCP: las cartas por comprar (jugadores que faltan en el club, ej. «Italia: 2 mín.») se leen mediante la API
  de la app de FUTBIN (listas filtradas por nación, liga, club, medias y calidad, ordenadas por precio, con los
  identificadores de EA y las posiciones). Las listas de futbin.com, que FUTBIN rechaza a las peticiones del script (403), solo sirven
  como respaldo: el solucionador ya no se detiene con «FUTBIN bloquea la lectura de precios».

## Novedades 5.4.3

- Tabla «Prima holo» más compacta: la columna «Compra máx.» cabe en el panel (promo eliminada de las filas,
  la promo escaneada es la del menú).

## Novedades 5.4.2

- Pestaña FUTBIN, sección «Prima holo»: cada carta promo existe en versión normal y en versión holo, más
  rara y más cara (TOTW 1 a 4 al 09/10/2026: holo 1,3 a 3 veces la normal; TOTW 80 ~10 500 frente a ~14 000,
  Lewandowski 85 13 750 frente a ~44 000). El escaneo pide a EA todas las cartas de la promo elegida (búsqueda
  «concepto» por rareza: una petición para los 184 TOTW, ninguna búsqueda en el mercado de transferibles); la holo
  lleva el aspecto holo de EA. Luego lee el precio de FUTBIN de la holo y de su versión normal mediante la API de la app de
  FUTBIN (una búsqueda por jugador). Las listas de futbin.com no se usan: FUTBIN las rechaza a las peticiones del script.
  La tabla muestra, por cada holo del rango de precios: precio normal, precio holo, ganancia (holo pagada al precio normal,
  revendida al precio holo, tras impuestos) y compra máx. (el precio más alto que deja el beneficio mínimo en la compra, 1 000
  por defecto).
- «Crear los filtros marcados»: un filtro «precio mercado EA» por holo (versión exacta, compra al 90 % del precio releído,
  tope = compra máx.), desactivado para hacer funcionar 3 a la vez. Una versión ya seleccionada como objetivo no se vuelve a crear.

## Novedades 5.4.1

- Primera versión del escáner de prima holo, mediante las listas de futbin.com: FUTBIN las rechaza a las peticiones del script
  (403), de ahí la 5.4.2.
- Corrección: la marca holo de la API de FUTBIN venía del campo «signature», que solo marca las holo de los jugadores
  destacados (15 TOTW de 92). Ahora viene de la animación holo de FUTBIN.

## Novedades 5.4.0

- Precios de FUTBIN mediante la API de la app de FUTBIN (futbin.org): una petición JSON ligera por jugador devuelve todas
  sus versiones (precios en consola y PC, precio anterior, rango de precios de EA, tendencia, tipo de carta, marca holo),
  sin página de verificación de Cloudflare. La carta se encuentra por su identificador exacto de EA; las demás versiones
  del mismo jugador reciben su precio de paso, y una versión leída hace menos de 60 s se reutiliza sin hacer petición.
- Las páginas de futbin.com siguen siendo el respaldo automático: API rechazada (pausa de 3 min), sin respuesta o carta no encontrada
  por su nombre. Las listas (barridos, DCP, galería) siguen pasando por las páginas.
- Tipos de carta aprendidos automáticamente de las imágenes de FUTBIN (3 = Team of the Week, 22 = Destined for Glory…): ninguna
  lista que actualizar en cada nueva promo. La marca holo viene de FUTBIN (más fiable que el identificador).
- Pestaña FUTBIN: «Fuente de precios FUTBIN» (API + páginas de respaldo, o solo páginas), peticiones de API en el estado,
  y el botón Probar intenta primero la API. Tampermonkey pedirá permiso para futbin.org al actualizar.

## Novedades 5.3.17

- Pestaña Compra: «Beneficio mínimo en la compra». El antipérdidas antes de la compra exige este beneficio (ej. 1 000), mientras
  que el precio suelo de puesta en venta mantiene el de la pestaña Venta (ej. 100): una carta comprada con +1 000
  previsto puede salir más barata si su mercado baja, en lugar de quedarse bloqueada a precio pagado + 1 000.

## Novedades 5.3.16

- Sin cambios de comportamiento. Prueba abandonada: el mercado de transferibles de la web app de FC 27 no envía la
  media (ovrMin / ovrMax) a EA, una búsqueda de «jugadores oro media 88» sigue siendo imposible; la media de los filtros sigue
  siendo aplicada por el bot sobre los resultados.

## Novedades 5.3.15

- DCP: el botón × que oculta una colección ya no tapa el botón de favorito de EA (esquina superior derecha de los
  mosaicos). Se coloca justo debajo. Tras ocultar, «Deshacer» permanece visible 6 s; después, la colección se
  vuelve a mostrar desde la pestaña Herramientas («Colecciones DCP ocultas»).

## Novedades 5.3.14

- Precio mercado EA: un anuncio solo se descarta como «malvendido» tras verificación. EA ordena los anuncios por tiempo restante:
  la página 1 muestra los que están por terminar (a menudo no vendidos, demasiado caros), no los últimos publicados. Cuando el sondeo
  no ha visto todos los anuncios hasta un 11 % por encima del precio bajo, una búsqueda limitada a ese nivel los muestra
  todos, incluidos los recientes. Varios anuncios al mismo precio forman el precio de mercado, nunca una ganga.
- Filtro de precio de mercado: 3 gangas o más de golpe por debajo de la compra máx. = precio de mercado falso o desactualizado. Sin compras,
  precio releído antes de la siguiente búsqueda (como una página llena).

## Novedades 5.3.13

- Puesta en venta al precio de mercado: una carta con un estilo de química (Sombra, Cazador…) se compara con los
  anuncios de la misma variante, y un anuncio malvendido aislado ya no sirve de referencia. Cuando el sondeo ve menos
  de 3 anuncios, una búsqueda adicional (hasta 1,6 × el precio más bajo) muestra los siguientes.

## Novedades 5.3.12

- Precio mercado EA: uno o dos anuncios malvendidos (más de un 10 % por debajo del 3.º más barato) ya no marcan el precio de
  referencia; se notifican en el registro y siguen siendo gangas que el bot compra.

## Novedades 5.3.11

- **Precio mercado EA** (3.er modo de precio de un filtro, versión exacta): el bot comprueba por sí mismo el precio de «compra
  inmediata» más bajo de la carta en el mercado de EA, estilo de química incluido (ej. un jugador con Sombra, que
  FUTBIN no cotiza). Compra hasta el X % de este precio, reventa un escalón por debajo. Precio releído al iniciar, cada
  N minutos (pestaña Compra, 10 por defecto) y tras cada compra; hasta 4 búsquedas por sondeo, contadas
  en el contador de peticiones.
- Página llena de anuncios por debajo de la compra máx. de un filtro de precio de mercado: el precio registrado está desactualizado, sin compras, precio releído.

## Novedades 5.3.10

- **Eliminar inactivos** (pestaña Objetivo): elimina de golpe todos los filtros desactivados, con una sola confirmación.
- **Beneficio mín.** (pestaña Venta): cantidad libre en monedas (10, 100…), ya no redondeada a un precio de EA (mínimo 150 antes).
- FUTBIN: un rechazo en la búsqueda o en las listas (403) ya no pausa las páginas de jugador, que siguen
  leyéndose; una página de verificación de Cloudflare sigue pausando todo FUTBIN.

## Novedades 5.3.0

- Beneficio real: cada venta vista durante la sesión se contabiliza (precio de venta tras impuestos − precio pagado),
  en el registro, el historial, el resumen al detenerse y debajo del KPI «Beneficio estimado».
- Notificaciones: inicio, carta vendida (con el beneficio), puesta en venta fallida y resumen cada N minutos
  (búsquedas, compras, ventas, beneficio real, saldo) además de los eventos existentes.
- Límites de seguridad siempre activos: 2 s como mínimo entre dos búsquedas, pausa como máximo cada 50 búsquedas,
  detención como máximo tras 12 h, 1,5 a 2,5 s entre dos compras o pujas de una misma búsqueda; inicio rechazado
  si EA bloquea el mercado de transferibles (cuenta, solo consola, mantenimiento).
- Filtros: exportación / importación JSON (copiar y pegar), resumen de sesión por filtro (búsquedas, compras, gastado, beneficio)
  en la lista, preajuste **Medias DCP** (oro 85 / 86 / 87 en modo FUTBIN, rotación).
- Modo «% del precio de FUTBIN» con un rango de medias (ej. 85–85): se sigue la lista de FUTBIN de las cartas más baratas de esas
  medias, sin jugador ni nación. **Puja máx. en % del precio de FUTBIN** de cada carta (la puja fija sirve de tope).
- Herramientas → Club: **Analizar el club** (valor en FUTBIN de los transferibles, monedas invertidas, plusvalía latente tras impuestos,
  mayores ganancias / pérdidas, reparto por media, cartas revendibles con beneficio para enviar a la lista de transferibles).
- Sobres: saltar animación de apertura (opción); elección de jugadores: la mejor carta aparece recuadrada (precio de FUTBIN o media).
- Atajos: 1 a 6 = inicio, plantilla, traspasos, club, DCP, evoluciones; alerta cuando una tecla sirve para dos acciones.
- Correcciones: el menú ⋮ de las listas de EA se muestra por encima de las cartas; la pestaña MagicBuyer del menú de EA ya no salta
  de sitio al cambiar de pantalla.

## Novedades 5.2.0

- Búsqueda de jugador por FUTBIN: aparece cada versión (carta base, cartas especiales, otro club…),
  con su versión y su club cuando FUTBIN los facilita. Un clic elige la versión exacta: ya no hace falta copiar el ID.
  «Todas las versiones» conserva todas las cartas del jugador, cada una comprada al X % de su propio precio de FUTBIN.
- Modo «% del precio de FUTBIN» para los filtros sin jugador (nación, liga, club, posición): el bot lee la lista de FUTBIN
  correspondiente (o el enlace de lista pegado en el filtro), la vuelve a leer cada 3 min y compra cada carta al X % de SU precio.
- Búsqueda inteligente: nunca hay búsquedas amplias sin precio máximo. Las cartas en seguimiento se reparten en tramos de precios
  ajustados (un tramo de una sola carta = búsqueda de esa versión exacta): pocos resultados, así que los anuncios
  más recientes siempre están en la primera página. Si un tramo devuelve una página llena, los tramos se estrechan.
- Idioma: la interfaz sigue el idioma de la cuenta de FC (francés o inglés; inglés para los demás idiomas), configurable en Alertas.
- Galería FC 27 en el inicio de la web app: colecciones de FUTBIN, lista preparada desde la apertura (siguiente nivel, tus cartas
  primero), compra de faltantes o compra + reventa masiva al mismo precio.
- Pestaña Herramientas: contador de peticiones a EA (pausa auto), atajos de teclado, herramientas del club (CSV, envío masivo,
  almacén de DCP → club, venta rápida), apertura de los sobres en posesión con ordenación automática.
- Cartas: puntuación de artículo (gema), gangas resaltadas en el mercado, precio pagado y beneficio en tus cartas;
  botón «Precio mín. EA» en el panel de puesta en venta.
- DCP: solución recargada para los desafíos repetibles, valor en FUTBIN de la plantilla, colecciones ocultables,
  ordenación de recompensas tras el envío, alerta cuando EA rechaza los DCP.
- Antipérdidas: ninguna compra ni puja que no dé beneficio tras el impuesto de EA (reventa prevista limitada por debajo del mercado
  visible), reventa nunca por debajo del umbral de rentabilidad, tecla de compra inmediata bloqueada en una compra con pérdidas, y
  compra inmediata comparada con el precio de FUTBIN debajo del botón de EA.
- Panel anclado en pantallas anchas: la web app se desplaza en lugar de quedar cubierta (Alertas → Interfaz).
- Botón MagicBuyer siempre justo después de Inicio en el menú de EA; teclas de los atajos más discretas.
- Listas de FUTBIN leídas en las filas reales de la tabla (nombre, versión, club, puntuación de artículo); el script de Cloudflare
  presente en todas las páginas de FUTBIN ya no se confunde con un bloqueo (listas de filtros y galería).
- Pruebas automáticas: `npm test`.

## Novedades 5.1.1

- DCP: lectura de soluciones de FUTBIN corregida (la plantilla se lee en los datos de la página, ya no en la vista):
  formación, posición exacta de cada jugador como en FUTBIN, posición bloqueada del desafío respetada, precio de la página tomado al instante.

## Novedades 5.1.0

- Compra al % del precio de FUTBIN por filtro, con precio seguido en directo durante el bot (tope opcional).
- Reventa y relistado al % del precio de FUTBIN, puesta en venta masiva de la lista de transferibles al precio de FUTBIN.
- Etiqueta de precio de FUTBIN en todas las cartas de jugador y botón «Rellenar» en el panel de puesta en venta de EA.
- DCP: importación de una solución de FUTBIN (formación + jugadores del club), compra de los faltantes al precio de FUTBIN modificable.
- Nueva pestaña FUTBIN (prueba de acceso, actualización inteligente, protección contra saltos de precio).
- FUTWIZ eliminado: FUTBIN sirve como fuente única. Los antiguos ajustes de «precio de referencia» se convierten automáticamente.

## Novedades 5.0.0

Reescritura completa del motor y de la interfaz para FC 27:

- Motor de snipe reescrito sobre las API reales de la web app de FC 27 (`UTSearchCriteriaDTO`, `services.Item`), sin reescritura de URL ni hooks frágiles.
- Corrige el bloqueo del bot tras la primera compra, la falta de detención ante un captcha, el precio de FUTBIN que reemplazaba el precio máximo,
  el anticaché desactivado en los jugadores objetivo, los ajustes que nunca se guardaban y el bucle de renderizado que cargaba el procesador.
- Nuevo panel lateral (la web app sigue siendo utilizable), botón flotante, registro filtrable, exportación CSV, prueba de búsqueda sin compra,
  importación de una búsqueda de EA, rotación entre varios filtros, perfiles de timing, pujas, notificaciones de Discord/Telegram.
