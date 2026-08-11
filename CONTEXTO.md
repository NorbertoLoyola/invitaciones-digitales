# Invitaciones digitales — producto de venta

Proyecto propio (no relacionado con A. D. Barbieri / TOTVS Protheus): convertir la invitación digital hecha para el cumpleaños de Aitana en un producto de invitaciones para vender, con varios templates por tipo de evento, usando un motor de plantillas (config por cliente) en vez de copiar el HTML entero cada vez.

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

## Publicación (catálogo de demostración)

`index.html` en la raíz de esta carpeta es el home/catálogo para mostrarle los templates a la prima (quien va a ofrecer el producto): un menú por categoría, cada card abre su template en pestaña nueva (`target="_blank"`). Pensado para publicarse tal cual con GitHub Pages, igual que `aitumiprimeranito` — al estar `index.html` en la raíz del repo, alcanza con subir esta carpeta como repo y activar Settings → Pages. Las rutas de las cards son relativas (`templates/bodas/index.html`, etc.), así que funcionan sin cambios apenas se suba la carpeta completa.

## Próximo paso

Las seis categorías madre tienen al menos un template generado. Pendiente:
- Variantes `nino`/`nina` de baby shower, bautismo y primera comunión (recoloreo de la variante `neutro`, misma estructura).
- Subestilos/categorías candidatos de la investigación de mercado que quedaron afuera de esta ronda (ver arriba).
- Evaluar el motor de plantillas real (`motor/`) que rellene `CONFIG` por cliente en vez de tener que copiar y editar el HTML a mano.

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
- Baby shower y bautismo: sin empezar todavía.
