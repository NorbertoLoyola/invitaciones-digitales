# Late — invitaciones digitales (producto de venta)

Proyecto propio (no relacionado con A. D. Barbieri / TOTVS Protheus): convertir la invitación digital hecha para el cumpleaños de Aitana en un producto de invitaciones para vender, con varios templates por tipo de evento, usando un motor de plantillas (config por cliente) en vez de copiar el HTML entero cada vez.

## Marca y negocio (decidido 2026-08-11, ver `PLAN-COMERCIAL.md` para el detalle completo)

- **Nombre**: Late. Handle de Instagram sugerido: `@late.invitaciones`.
- **Quién lo ofrece**: Norberto y su mujer (corrige una mención anterior en este documento que decía "la prima" — esa fue una conversación previa, el dato actualizado es este).
- **Precio inicial (beta)**: $20.000–$25.000, por debajo de la referencia de mercado hasta tener casos reales.
- **Estrategia de lanzamiento**: validar el flujo operativo gratis con algún cumpleaños infantil de un conocido primero; lanzamiento comercial real con Bodas o 15 años después.

## Modelo de referencia

Competidor observado: `didi.tarjetas`, cobra $30.000–$40.000 por invitación de boda digital.

## Estructura de categorías

Categorías madre por tipo de evento, cada una con su(s) propio(s) subestilo(s) visual(es):

- **Bodas** — dos estilos: `rustico/` (verde eucalipto/terracota/pergamino) y `botanico/` (vino/salvia/crema, tendencia 2026). Ver `templates/bodas/README.md`.
- **15 años** — un estilo, glam/fiesta (`templates/quince/index.html`).
- **Cumpleaños infantil** — subestilos por rango de edad, los cuatro ya generados (ver `templates/cumple-infantil/README.md`): 0–2 años (pastel), 3–5 años (selva/animalitos genéricos), 6–9 años (espacial/astronautas genéricos), 10–12 años (gamer/pixel-art/neón).
- **Baby shower** — segmentado por género (no por edad): `neutro` ya generado (boho: salvia/mostaza/crema), `nino`/`nina` pendientes como recoloreos. Ver `templates/baby-shower/README.md`.
- **Bautismo** — segmentado por género (no por edad): `neutro` ya generado (elegante blanco/dorado/celeste), `nino`/`nina` pendientes como recoloreos. Ver `templates/bautismo/README.md`.
- **Primera comunión** — categoría nueva (distinta de bautismo), segmentada por género: `neutro` ya generado (crema/dorado/azul polvo, motivo de trigo), `nino`/`nina` pendientes. Ver `templates/primera-comunion/README.md`.
- **Cumpleaños de adultos** — categoría nueva. `sobre/` generado: animación protagonista = sobre que se abre con un tap al entrar. Ver `templates/cumpleanos-adultos/README.md`.
- **Despedida de soltera** — categoría nueva. `confetti/` generado: animación protagonista = explosión de confetti (canvas) al tocar "Confirmar asistencia". Ver `templates/despedida-soltera/README.md`.
- **Aniversario** — categoría nueva. `carrusel/` generado: animación protagonista = carrusel de fotos "Nuestra historia" con swipe nativo. Ver `templates/aniversario/README.md`.
- **Baby shower / gender reveal** — sumado `reveal/` a la categoría existente: animación protagonista = tarjeta circular con flip 3D que revela "¿niño o niña?".

## Arquitectura técnica

Cada template usa un objeto `CONFIG` centralizado (nombre, fecha, lugar, colores, textos, WhatsApp, mapa) para separar contenido de diseño. El motor de plantillas, cuando exista, solo necesita rellenar ese objeto por cliente en vez de tocar HTML/CSS de cada invitación.

**Novedad de motor (sumada en `cumple-infantil/0-2-anos/dulce/`)**: `CONFIG.rsvpContactos` — un array de `{etiqueta, numero, mensaje}` en vez de un único `whatsappNumero`/`whatsappMensaje`. Permite RSVP con más de un botón (ej: "Confirmar con mamá" / "Confirmar con papá"), pero también funciona con un solo contacto en el array. El resto de los templates todavía usan el patrón viejo (`whatsappNumero` + `whatsappMensaje` únicos) — migrarlos a `rsvpContactos` queda como tarea pendiente del motor si se quiere unificar.

## Base técnica reutilizable (de la invitación de Aitana)

- Scroll continuo (no pantallas completas por sección) con `IntersectionObserver` para fade-in progresivo.
- Título con efecto de tipeo letra por letra (JS).
- Cuenta regresiva en vivo (JS, `setInterval`).
- Botón "Ver ubicación" → link directo a Google Maps armado con la dirección.
- Botón "Confirmar asistencia" → `wa.me` con mensaje precargado.
- Elementos decorativos temáticos flotando de fondo (en la de Aitana: moños chiquitos, con `@keyframes` de traslado+opacidad, tamaño/duración/delay aleatorios por elemento).
- `<meta name="color-scheme" content="light only">` + `color-scheme: light` en CSS — imprescindible para que no se vea oscura en Samsung Internet/Chrome con modo oscuro del sistema.
- Publicación vía GitHub Pages (repo `aitumiprimeranito`, archivo debe llamarse `index.html`, activar Settings → Pages).

## Progreso de templates

- **Bodas**: dos estilos generados, `rustico/` (con sección de "mensaje del cliente", igual a la de la invitación de Aitana) y `botanico/` — ver `templates/bodas/README.md`.
- **15 años (glam/fiesta)**: generado, con sistema de `CONFIG`.
- **Cumpleaños infantil**: los cuatro subestilos por rango de edad generados — ver `templates/cumple-infantil/README.md`.
- **Baby shower**: variante `neutro` generada — ver `templates/baby-shower/README.md`.
- **Bautismo**: variante `neutro` generada — ver `templates/bautismo/README.md`.
- **Primera comunión**: variante `neutro` generada — ver `templates/primera-comunion/README.md`.

## Investigación de mercado (2026-08-10)

Búsqueda de tendencias/competidores para decidir qué sumar. Hallazgos que llevaron a las decisiones de arriba:
- El estilo **botánico** (ramas/hojas/flores silvestres, tonos tierra profundos) es la tendencia #1 en bodas 2026, tanto en papel como digital.
- **Primera comunión** aparece como categoría separada de bautismo en casi todas las plataformas competidoras (mercado propio, no un subestilo).
- Otras categorías candidatas que quedaron afuera de esta ronda por menor prioridad: cumpleaños de adultos, despedida de soltera/o, aniversario, jubilación/graduación.
- Otros subestilos candidatos que quedaron afuera: 15 años boho y pastel princess, dinosaurios para 3–5 años, baby shower safari y gender reveal.

## Punto pendiente importante — copyright

Cuidado con temas de personajes con copyright (Dora la Exploradora, Plim Plin, Minnie Mouse, etc.): no se pueden reproducir esos personajes tal cual para vender — es infracción de marca.

Alternativas:
- Diseños inspirados en la temática (selva/exploradora, circo/payaso) con personajes genéricos propios.
- Clip art con licencia comercial que el cliente ya tenga comprado.

**Caso real (2026-08-10)**: se encontró una invitación de referencia en Instagram (@invita.digitalc) con tema Minnie Mouse. Se descartó el personaje, pero se rescató el **formato/UX** (número grande detrás del nombre, tarjetas "blob", RSVP doble mamá/papá) para el nuevo template `cumple-infantil/0-2-anos/dulce/`. Buen precedente: cuando aparezca una referencia con copyright, separar "estilo/interacción" (reutilizable) de "personaje" (no reutilizable).

**Nota (2026-08-10)**: el usuario pidió explícitamente ignorar el copyright ("no será algo masivo, es para ofrecer por la zona"). Se mantuvo la regla igual — el riesgo de infracción de marca no depende de la escala del negocio, y Disney en particular persigue activamente a vendedores chicos/locales. En vez de usar personajes con marca, la respuesta fue profundizar en el eje que sí escala sin riesgo: más estilos con **animaciones distintas** (sobre que se abre, confetti, carrusel, flip card), todos con temática genérica.

## Rediseño de la home/catálogo (2026-08-11)

Pasó de ser un "catálogo de demostración" interno a la vidriera real que va a ver el cliente para elegir estilo (pensado para promocionar por Instagram). Cambios:

- **Paleta/tipografía nueva**: terracota + ciruela + dorado, con Fraunces + Parisienne (script) + Jost — las mismas familias que ya usan varios templates individuales, para que la vidriera se sienta parte de la misma marca.
- **Sección "Cómo funciona"** (3 pasos: elegís estilo → contás tus datos → recibís tu invitación) — construye confianza como página de producto real, no solo demo.
- **Catálogo reescrito data-driven** (array `CATALOGO` en JS) en vez de HTML repetido a mano — más fácil de mantener a medida que se sumen estilos.
- **Cada card tiene dos CTA**: "Ver ejemplo" (como antes) + **"Elegir este estilo"** (nuevo). Este último es el punto de enganche para venta: hoy abre WhatsApp con un mensaje pre-cargado nombrando el estilo elegido; el día que se sume Mercado Pago (o cualquier otro cobro), alcanza con completar el campo `mpLink` de ese item en el array — no hace falta tocar el resto del HTML/JS. Ningún precio se inventó ni se mostró — no había una lista de precios definida por estilo, así que no se puso ninguna cifra falsa.

### Segunda vuelta del rediseño (mismo día): hero con celular + beneficios + testimonios

- **Se sacó la animación de íconos flotando del hero** — pedido explícito de no repetir el mismo recurso visual que ya usan los templates individuales.
- **Se agregó un mockup de celular en el hero** con `<iframe>` de las invitaciones reales rotando cada ~4s (crossfade entre dos iframes superpuestos), destacando 5 estilos variados (bodas botánico, 15 glam, cumple dulce, despedida confetti, aniversario carrusel). Es contenido real (las invitaciones en vivo), no capturas estáticas — se actualiza solo si cambia algún template.
- **Sección "Por qué una invitación digital"**: 4 beneficios reales del producto (sin instalar nada, ubicación al instante, confirmación por WhatsApp, cero papel) — pensada para sumar sustancia a la página sin inventar nada.
- **Sección "Lo que van a decir tus invitados"**: pedida como "contenido de éxito"/testimonios de relleno. Se armó pero **marcada explícitamente como ejemplo** (badge "Ejemplo" en cada tarjeta + aclaración en el texto) en vez de simular clientes reales con nombre y foto — publicar testimonios inventados como si fueran genuinos en una web pública es publicidad engañosa, incluso como relleno temporal. Reemplazar por comentarios reales apenas haya clientes.

### Tercera vuelta (mismo día): scroll simulado en el banner del celular — debug real con browser automation

El pedido: que cada template del banner, mientras se muestra, simule que alguien lo está scrolleando (para apreciar el template completo, no solo el hero), lento, uno por vez. Se probó y falló dos veces antes de andar — el debug se hizo con control real de navegador (`claude-in-chrome`), no a ciegas:

1. **Primer intento** (transform + iframe gigante escalado): tenía un bug real de reset (la vuelta a 0 se animaba en vez de ser instantánea, por transición siempre activa). Se corrigió, pero...
2. **Segundo intento**: se midieron las alturas reales de los templates destacados con el navegador (bodas/botánico 3477px, quince 3567px, cumple-dulce 3249px, despedida-confetti 3002px, aniversario-carrusel 3226px — quedaron hardcodeadas un momento y después se sacaron). Al probarlo con captura de pantalla real, la pantalla del celular quedaba **en blanco** — un iframe nativo de +3000px de alto escalado con `transform:scale()` tiene un bug de renderizado conocido en Chrome (el DOM carga bien, `readyState` da `complete`, pero no pinta el contenido).
3. **Solución final**: se abandonó la idea de un iframe gigante escalado. Ahora cada iframe tiene tamaño de celular real (390×812) y el scroll se hace de verdad adentro con `contentWindow.scrollTo()` (mismo origen, así que es accesible) — el `transform:scale()` solo se usa para reducir visualmente el iframe ya renderizado, no para fingir el scroll. Esto también reveló dos bugs más, encontrados con el navegador real en vez de a ciegas: (a) el CSS de los templates trae `scroll-behavior:smooth`, que competía con la animación manual — se fuerza `behavior:'instant'` en cada llamada; (b) la pestaña de prueba estaba en segundo plano (`document.hidden`), y `requestAnimationFrame` se pausa por completo ahí — se cambió a `setInterval` (50ms), que sigue funcionando (aunque con throttling) en pestañas no visibles, y corre sin problema en una pestaña real en foco.
4. Timing final: pausa de 700ms arriba → scroll de 9.2s → resto de pausa hasta completar ~11.4s por template → crossfade de 300ms al siguiente.

**Nota de compatibilidad (confirmada, no solo sospechada)**: el mecanismo de `contentWindow.scrollTo()` requiere mismo origen entre la página y los templates. Abriendo `index.html` con doble clic (protocolo `file://`) Chrome bloquea ese acceso — el crossfade entre templates se ve, pero el scroll simulado no corre (el JS hace early-return silencioso al no poder acceder a `contentWindow`), dando la sensación de que "cambia rápido sin mostrar nada". Se confirmó sirviendo el sitio con un server local (`http://localhost:8934`) y midiendo `scrollY` en vivo con `claude-in-chrome`: el scroll progresa perfecto (0 → 2664px en ~10s, hasta el fondo real del template). Va a andar bien apenas esté publicado en GitHub Pages (mismo origen real). Para probarlo antes de publicar, hace falta servirlo con algún servidor local (no doble clic al archivo) — **desde acá en adelante, todos los ajustes de esta página se prueban así**, sirviendo con un server local y verificando con `claude-in-chrome` en vez de abrir el archivo directo, porque es más representativo de cómo se va a ver en producción.

Timing final ajustado a pedido: crossfade 0.4s, pausa antes de scrollear 250ms, scroll 13.5s (se subió de 9.2s por pedido explícito de que se sienta menos apurado), ciclo total ≈ 14.65s por template.

### Cuarta vuelta (mismo día): nav fijo (Inicio / Beneficios / Templates)

Se agregó `<nav class="navbar">` sticky arriba de todo, con tres links de scroll suave (usa el `scroll-behavior:smooth` que ya tenía el `html`, sin JS extra): Inicio (`#inicio`, la sección `.hero`), Beneficios (`#beneficios`), Templates (`#catalogo`, ya existía ese id en el `<main>`). Se agregó `scroll-margin-top` a esas tres secciones para que el nav fijo no tape el título al saltar. Verificado con `claude-in-chrome`: clickear "Templates" salta limpio a la sección de Bodas sin recortar el título.

## Publicación (catálogo de demostración)

`index.html` en la raíz de esta carpeta es el home/catálogo para mostrarle los templates a la prima (quien va a ofrecer el producto): un menú por categoría, cada card abre su template en pestaña nueva (`target="_blank"`). Pensado para publicarse tal cual con GitHub Pages, igual que `aitumiprimeranito` — al estar `index.html` en la raíz del repo, alcanza con subir esta carpeta como repo y activar Settings → Pages. Las rutas de las cards son relativas (`templates/bodas/index.html`, etc.), así que funcionan sin cambios apenas se suba la carpeta completa.

## Egresados — dos categorías nuevas (2026-08-15)

A pedido de un tercero que le recomendó al usuario sumar egresados al catálogo. Se investigó primero en internet (tipografía cinética/interactividad como tendencia 2026 en general; birrete+borla dorada y diplomas/certificados como motivo recurrente específico de egresados) y se confirmó con el usuario que hacían falta **dos categorías separadas**, no una — en Argentina "egresados" es un producto totalmente distinto según la edad:

- **Egresados de secundaria/universidad** (`templates/egresados-secundaria/birrete/`): paleta de gala (azul noche + dorado), tipografía Cormorant Garamond + Parisienne. Animación protagonista: **lanzamiento de birretes** (canvas, dibujados con primitivas — no emoji ni imagen) al tocar "Confirmar asistencia". `color-scheme: dark`.
- **Egresados de jardín/primaria** (`templates/egresados-primaria/diploma/`): paleta infantil (celeste/amarillo/coral), tipografía Baloo 2 + Caveat. Animación protagonista: **sello "¡Lo logré!"** que se estampa con un keyframe de escala+rotación (CSS puro, sin canvas) al confirmar. `color-scheme: light`.

Ambas siguen el mismo motor `CONFIG` y la misma estructura de secciones que el resto de los templates (hero, countdown, itinerario, nota, galería, mensaje, RSVP).

**Fotos de stock**: 4 por template, buscadas en Unsplash con `claude-in-chrome` (navegador real, no scraping a ciegas) y descargadas vía el botón "Descargar gratis" de cada foto — mismo criterio de curación que en la Fase 3 del catálogo (sin personajes con copyright, sin nombres/fechas reales, ocasión y edad correctas). Aprendizaje nuevo de esta ronda: Unsplash mezcla resultados **Unsplash+ (de pago)** entre los gratuitos en la grilla de búsqueda — hay que abrir cada foto y confirmar que el título de la pestaña diga "Foto gratuita en Unsplash" (no "Foto en Unsplash+") antes de descargar, el ícono superpuesto en la miniatura no siempre es visible/confiable. Las fotos bajan a resolución original (algunas de 10+ MB) — se redimensionaron a 1000px de ancho / calidad JPEG 80 antes de sumarlas al repo, para no inflar el catálogo con imágenes pesadas.

**Bug real encontrado y corregido** probando en vivo con servidor local + `claude-in-chrome`: en `egresados-primaria`, el sello quedaba invisible para siempre después del primer "Confirmar asistencia" — el `setTimeout` que lo hace desaparecer le seteaba `style.opacity = '0'` inline, que tiene más especificidad que la animación CSS y bloqueaba cualquier reintento posterior. Se corrigió limpiando ese inline (`sello.style.opacity = ''`) al inicio de cada clic, antes de re-agregar la clase `.slam`.

**Home actualizado**: dos secciones nuevas sumadas al array `CATALOGO` en `index.html` (mismo patrón que las demás — swatch, desc, mensaje de WhatsApp precargado), verificadas visualmente en el catálogo real antes de cerrar la tarea.

## Análisis de competencia — invitio.events (2026-08-16)

Se recorrió `invitio.events/es/crear` (flujo completo: elegir evento → elegir plantilla → preview) buscando funcionalidades para adoptar como **patrón/interacción**, no contenido (mismo criterio que ya se usa con el copyright de fotos). Hallazgos, de más a menos fuertes:

1. **Playlist de Spotify embebida** ("Playlist de la Party") — widget oficial de Spotify, reproducible/guardable desde la invitación. No implementado todavía — requiere elegir/crear la playlist real por evento, es más una decisión de contenido que de código.
2. **Mapa embebido interactivo** — en vez de solo un botón "Cómo llegar" que abre afuera, un iframe real de Google Maps dentro del scroll. **Implementado** (ver abajo).
3. **Botón "Añadir a mi calendario"** — agrega el evento al Google Calendar del invitado con un tap. **Implementado** (ver abajo).
4. **Personalización instantánea en el selector de plantillas**: antes de mostrar la galería, pregunta el nombre del evento y lo aplica en vivo a las miniaturas. **Implementado** (ver abajo) — con una variante más suave: no bloquea el acceso al catálogo, es un campo opcional arriba de los chips de filtro.
5. **CTA flotante fijo** durante todo el scroll de la plantilla. **Implementado** (ver abajo) — se aplicó al botón de RSVP de la invitación individual (no al catálogo, que ya tiene su propio patrón de card con dos CTA; son casos distintos).
6. **Galería en pila estilo Polaroid con swipe** en vez de grilla estática. No implementado — cambio visual más grande, evaluar a futuro.

Confirma también que "Graduación" es una categoría de mercado real y separada — valida la decisión de sumar egresados.

### Patrón nuevo: mapa embebido + "Añadir a calendario" (sin API key, sin costo)

Implementado primero como prueba en `templates/egresados-secundaria/birrete/index.html`, y luego **propagado a los 17 templates del catálogo completo** (confirmado con el usuario antes de replicarlo). El campo `text` del link de calendario se generalizó a `document.title` (en vez del `CONFIG.tituloEvento+promocion` específico de egresados) porque cada template usa nombres de campo distintos para el título del evento — así el snippet queda idéntico y copiable en cualquier template nuevo. Verificado en vivo en 3 paletas bien distintas (quince dark/glam, cumple-infantil 10-12 neón, bodas botánico claro) — se adapta bien en las tres.

- **Mapa**: `<iframe src="https://www.google.com/maps?q={CONFIG.lugarDireccion}&output=embed">` — el truco `output=embed` de Google Maps no requiere API key ni facturación (a diferencia del Maps Embed API oficial). Se agregó dentro de la sección de lugar/itinerario, con el botón "Cómo llegar" renombrado a "Abrir en Google Maps" como acción secundaria.
- **Calendario**: link a `https://calendar.google.com/calendar/render?action=TEMPLATE&...` (truco no oficial pero estable y muy usado, sin API key). La hora se convierte de local a UTC con `new Date(CONFIG.fechaCountdown).toISOString()` — funciona correctamente sin importar en qué huso horario esté el invitado. Duración por defecto: 3 horas (no hay un campo de hora de fin en `CONFIG` todavía).
- Ambos se probaron en vivo con servidor local + `claude-in-chrome`: el mapa carga la dirección real, el link de calendario arma bien texto/fecha/ubicación (verificado parseando la URL generada, no solo mirándola).

### Patrón nuevo: playlist de Spotify embebida (opcional, con fallback curado por categoría)

Scaffolding sumado a los **17 templates del catálogo completo**, anclado en el mismo bloque `#calendar-section`/`calendar-btn` del patrón anterior (verificado por Grep como idéntico en los 17 archivos antes de tocar nada — `#nota-section` no sirve de ancla porque quince y ambos templates de bodas no tienen sección de dress code).

- **Embed**: cualquier URL de `open.spotify.com/...` se vuelve reproducible insertando `/embed` después del dominio: `open.spotify.com/embed/playlist/...`. Truco oficial de Spotify, gratis, sin API key.
- La sección (`#playlist-section` + `#playlist-divider`) arranca oculta (`display:none`) y solo se muestra si `CONFIG.spotifyUrl` está definida — así ningún template rompe visualmente mientras no haya una URL real cargada.
- `CONFIG.spotifyUrl` **no se agregó como campo vacío** a ningún `CONFIG` todavía — se va a sumar con la URL real directamente cuando existan las playlists curadas, evitando una pasada de edición redundante.
- Pregunta correspondiente ya sumada al skill `entregar-invitacion`: se le pregunta al cliente si tiene playlist propia (opcional); si no manda nada, usar la curada por Late para esa categoría (ver más abajo).
- Verificado en vivo con servidor local + `claude-in-chrome` en 2 templates (quince y cumple-infantil 10-12 años): sin `spotifyUrl`, la sección queda oculta y no hay errores de consola; forzando un `spotifyUrl` de prueba, el iframe se arma con la URL `/embed/` correcta y el widget de Spotify se ve integrado con la paleta de cada template.

**Pendiente (pausado 2026-08-16 a pedido del usuario)**: curar las playlists reales por categoría (una por categoría del catálogo, para la demo y como fallback en entregas reales). Esto requiere buscar/armar playlists en una cuenta de Spotify real — Claude no puede loguearse ni manejar credenciales de terceros bajo ninguna circunstancia, así que esta parte necesita participación activa del usuario (loguearse él mismo y armar las playlists, o pedir una lista de temas sugeridos para armarlas por su cuenta). El usuario todavía no tiene cuenta de Spotify — retomar cuando la tenga y esté en la compu (venía trabajando desde el celular).

### Patrón nuevo: CTA flotante "Confirmar asistencia" (aparece al salir del hero, se oculta en el RSVP real)

Implementado primero como prueba en `templates/quince/index.html`, verificado en vivo con servidor local + `claude-in-chrome` (scroll real, no programático — el `IntersectionObserver` no reacciona de forma confiable a saltos de scroll programáticos como `scrollTo`/`scrollIntoView` dentro de este entorno de automatización; con un scroll real de mouse-wheel sí funciona siempre, que es el caso real de un usuario) y luego **propagado a los 17 templates del catálogo completo** (confirmado con el usuario antes de replicarlo).

- Reutiliza la clase `.btn` ya existente de cada template (mismo color de acento que el resto de los botones), así no hace falta definir una paleta nueva por template — solo se agregan las reglas de `position:fixed` con un ID (`#floating-cta`) que gana por especificidad sobre `.btn`.
- Ancla en `#hero` y `#rsvp-section` (ambos ids universales en las 17 plantillas, confirmado por Grep antes de tocar nada): un solo `IntersectionObserver` observa los dos, y el CTA se muestra solo cuando ninguno de los dos está en pantalla (`!heroVisible && !rsvpVisible`) — así no aparece sobre el hero ni duplica el botón real de RSVP cuando el invitado ya llegó a esa sección.
- El link es un `<a href="#rsvp-section">` simple — aprovecha el `scroll-behavior:smooth` que ya tiene el `<html>` de cada template, no hace falta JS de scroll propio.
- Colocado como hijo directo de `<body>` (antes de `.wrap` u otros elementos), para evitar el problema de "containing block" de `position:fixed` — si un ancestro tuviera `transform` (como las secciones `.fade-up` lo tienen mientras animan), el fixed dejaría de anclarse al viewport.

### Patrón nuevo: personalización instantánea en el catálogo (`index.html`)

A diferencia de invitio.events (que bloquea la entrada a la galería con una pregunta previa), se implementó como un campo **opcional** arriba de la barra de chips: "¿Para quién es la fiesta?". No hay backend ni persistencia — es puro JS en el cliente, se resetea al recargar.

- Cada card del catálogo tiene un `.card-preview` oculto (`max-height:0; opacity:0`) entre el eyebrow de categoría y el título del estilo.
- Al tipear, un listener `input` recorre las ~17 cards y les asigna una frase corta generada por categoría (`fraseParaCategoria`, un `switch` con una frase por categoría del `CATALOGO` — ej. "Bodas" → "{Nombre} se casa 💍", "15 años" → "Los 15 de {Nombre} 🎉"), capitalizando la primera letra automáticamente. Si el campo queda vacío, todas las previews vuelven a ocultarse.
- Verificado en vivo con servidor local + `claude-in-chrome`: tipeando "valentina" aparece "Valentina se casa 💍" / "Los 15 de Valentina 🎉" / "¡Valentina cumple años! 🎂" en las cards correspondientes, en tiempo real y sin recargar; al vaciar el campo, vuelve limpio. Sin errores de consola.

## Próximo paso

Las categorías madre tienen al menos un template generado. Pendiente:
- Variantes `nino`/`nina` de baby shower, bautismo y primera comunión (recoloreo de la variante `neutro`, misma estructura).
- Subestilos/categorías candidatos de la investigación de mercado que quedaron afuera de esta ronda (ver arriba).
- Evaluar el motor de plantillas real (`motor/`) que rellene `CONFIG` por cliente en vez de tener que copiar y editar el HTML a mano.
- Definir seguimiento para cuando el usuario ofrezca las ~5 invitaciones gratis de lanzamiento (ver si vale usar la planilla de RSVP para medir enganche real).

## Estructura de carpetas creada

```
invitaciones-digitales/
  index.html      (home/catálogo — menú con todos los templates, cada uno abre en pestaña nueva)
  CONTEXTO.md
  base-aitana/
    index.html    (invitación original de Aitana, copiada de Downloads — es la publicada en GitHub Pages)
  templates/
    bodas/
      README.md              (subestilos)
      rustico/index.html     ("Nos casamos" — verde eucalipto/terracota/pergamino)
      botanico/index.html    (vino/salvia/crema)
    quince/index.html       (template glam — "Mis 15", con CONFIG)
    cumple-infantil/
      README.md               (subestilos por rango de edad)
      0-2-anos/
        pastel/index.html     (pastel, hereda de base-aitana)
        dulce/index.html      (blob/número gigante/RSVP doble mamá-papá)
      3-5-anos/index.html     (selva/animalitos genéricos)
      6-9-anos/index.html     (espacial/astronautas genéricos)
      10-12-anos/index.html   (gamer/pixel-art/neón)
    baby-shower/
      README.md              (variantes por género)
      neutro/index.html      (boho: salvia/mostaza/crema)
      nino/, nina/           (pendientes)
    bautismo/
      README.md              (variantes por género)
      neutro/index.html      (elegante blanco/dorado/celeste)
      nino/, nina/           (pendientes)
    primera-comunion/
      README.md              (variantes por género)
      neutro/index.html      (crema/dorado/azul polvo, motivo de trigo)
      nino/, nina/           (pendientes)
  motor/          (para el motor de plantillas / CONFIG cuando se defina)
```

## Estado de archivos locales (última verificación 2026-08-10)

- **Invitación de Aitana**: guardada en `base-aitana/index.html` (versión realmente publicada en GitHub Pages, coincide con `norbertoloyola.github.io/aitumiprimeranito`).
- **Template de bodas (rústico)**: guardado en `templates/bodas/rustico/index.html` (título "Nos casamos", con `const CONFIG`). Se le agregó `<meta name="color-scheme" content="light only">` (faltaba). Reubicado desde `templates/bodas/index.html` al sumar el estilo botánico.
- **Template de bodas (botánico)**: guardado en `templates/bodas/botanico/index.html`, generado directamente con `color-scheme: light only`.
- **Template de primera comunión (neutro)**: guardado en `templates/primera-comunion/neutro/index.html`, generado directamente con `color-scheme: light only`.
- **Template de 15 (glam)**: guardado en `templates/quince/index.html` (título "Mis 15", con `const CONFIG`). Se le agregó `<meta name="color-scheme" content="dark only">` (faltaba) — es dark only y no light only porque el diseño es oscuro por naturaleza (`color-scheme: dark` en el CSS).
- **Cumpleaños infantil**: cuatro subestilos generados en `templates/cumple-infantil/` (`0-2-anos`, `3-5-anos`, `6-9-anos`, `10-12-anos`), cada uno con su propio `color-scheme` (light/dark según corresponda al diseño) y el mismo motor `CONFIG`.

## Fase 3 — fotos de stock en el catálogo (2026-08-13)

Los 13 templates con sección de galería (todos menos `aniversario/carrusel`, que usa un carrusel de texto, y `baby-shower/reveal`, que no tiene galería) reemplazaron el placeholder `<div class="ph">Foto N</div>` por `<img>` reales, apuntando a `templates/<categoría>/img/foto-N.jpg` (4 fotos c/u, descargadas al repo, no enlazadas a Unsplash).

**Por qué existen estas fotos y qué NO son**: son fotos de stock con licencia Unsplash (uso comercial libre, sin atribución) elegidas para el **catálogo de demostración público** — para que un visitante vea el estilo real de cada template en vez de cajas grises con "Foto 1". No son fotos de ningún cliente. Cuando se arme la Fase 4 (repo de entregas separado), cada venta real lleva las fotos que mande el cliente, no estas.

**Filtro aplicado al elegir cada foto** (varias búsquedas de Unsplash devolvieron resultados que había que descartar a mano, no alcanzaba con la relevancia del buscador):
- Nada de personajes con marca/copyright reconocible visible en la foto (se descartaron: fondo con Tiana de Disney en una foto de quince, campera "PAW Patrol", castillo inflable con Mickey Mouse, gorro y caja "Krispy Kreme", botella "Maker's Mark" de fondo).
- Nada de nombres, fechas o iniciales de un bebé/persona real horneados o escritos en la foto (varias tortas de "cumple 1 año" en los resultados tenían el nombre real del bebé del cliente original — se descartaron aunque la ocasión encajara).
- Ocasión correcta, no solo "se ve parecido": se descartaron fotos de Año Nuevo (globos "Happy New Year"), Navidad (papel de regalo con muñecos de nieve) y "sweet 16" que aparecían mezcladas en búsquedas de cumpleaños/quince.
- Edad correcta para cumple-infantil: la búsqueda "tween birthday party" devolvía sobre todo adultos de 20-30 años: se descartaron y se buscó de nuevo con términos más específicos ("kids party games", regalos/torta sin personas) hasta lograr las 4 fotos del rango 10-12 años.
- Para despedida de soltera: se descartó una foto con un hombre en primer plano (evento típicamente solo de mujeres) y otra de Año Nuevo mal indexada como "bachelorette".

Este es el mismo criterio de cuidado con copyright que ya se había aplicado al caso "dulce" (Minnie Mouse) documentado más arriba — se mantiene como estándar para cualquier foto que se sume al catálogo público de ahora en más.
