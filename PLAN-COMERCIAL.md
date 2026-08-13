# Plan comercial — de demo a primera venta

Plan de acción sobre los 7 puntos de la revisión crítica del 2026-08-11. Organizado en fases: cada una depende de que la anterior esté resuelta, no de que esté perfecta. El objetivo de las fases 0-3 es una sola cosa: **poder vender la primera invitación real sin mentir en ninguna promesa del catálogo.**

---

## Fase 0 — Decisiones (esta semana, sin código)

Nada de esto se programa — son decisiones de negocio que todo lo demás depende de que estén tomadas.

**Decisiones tomadas (2026-08-11):**

- [x] **Estrategia de categoría, en dos pasos.** No hay contacto cercano todavía para una boda/15 gratis, así que: (1) validar todo el flujo operativo (Fases 1-4) gratis con **algún cumpleaños infantil** de un conocido — bajo riesgo social, fácil de conseguir rápido; (2) una vez probado el flujo, **lanzamiento comercial con Bodas o 15 años** (mayor ticket, $30-40k de referencia de mercado). El resto de las categorías quedan en el catálogo como "también hacemos esto".
- [x] **Precio inicial: $20.000–$25.000** para las primeras ventas beta, por debajo de la referencia de mercado mientras no hay casos reales — sube una vez que haya 3-5 entregas reales y testimonios de verdad.
- [x] **Marca: "Late"** — corto, cálido, evoca "late el corazón / late la fiesta" sin sonar frío ni over-emocional. Handle de Instagram sugerido: **@late.invitaciones** (con la palabra clave, para aparecer en búsquedas — "Late" solo es muy genérico).
- [x] **Quién es la cara del negocio: Norberto y su mujer** (no la prima, como se había mencionado en una conversación anterior — corregido en `CONTEXTO.md`).
- [ ] **Flujo de entrega manual, por escrito** — ver sección siguiente.

### El flujo de entrega (borrador para validar)

1. El cliente ve el catálogo (link en la bio de @late.invitaciones) y elige un estilo.
2. Toca **"Elegir este estilo"** → se abre WhatsApp con el estilo ya mencionado en el mensaje.
3. Uno de los dos responde, confirma precio ($20-25k mientras es beta) y pide los datos con este checklist:
   - Nombre(s) del homenajeado/protagonistas
   - Fecha y hora del evento
   - Lugar (nombre + dirección para el mapa)
   - Texto del mensaje personalizado (o si prefieren que lo redacten ustedes)
   - Fotos (si quieren incluir — 2 a 4)
   - Número de WhatsApp donde quieren recibir las confirmaciones de asistencia
4. Piden seña (ej. 50%) por transferencia — no hay Mercado Pago todavía (Fase 7).
5. Con los datos, editan el `CONFIG` del template elegido y agregan las fotos reales (Fase 3).
6. Suben el HTML a una carpeta nueva en el repo de entregas (Fase 4) — nunca en el catálogo público.
7. **Prueban el link ustedes mismos antes de mandarlo**: countdown corriendo bien, el mapa apunta a la dirección correcta, el botón de WhatsApp manda al número correcto, las fotos cargan.
8. Le mandan el link al cliente para que lo revise antes de compartirlo con sus invitados.
9. Cliente aprueba (o pide un ajuste chico) → cobran el resto del precio.
10. Cliente comparte el link con sus invitados.
11. En cuanto llegue la primera confirmación, le pasan al cliente el link de su planilla (aparece en el índice "Late — RSVP" — ver `rsvp-apps-script.md`). Ve ahí, en tiempo real, quién confirmó, además de recibir cada aviso por WhatsApp.
12. Pasado el evento, les piden permiso para usar la invitación como caso real en el catálogo (reemplaza un testimonio "Ejemplo").

---

## Fase 1 — Arreglos técnicos rápidos y gratis (1-2 días)

Todo esto no depende de que exista una venta todavía. Se hace una vez, sobre el catálogo y sobre los templates base.

- [x] **Meta tags Open Graph en `index.html`** — título, descripción, `og:image` (captura real del hero, `og-image.png`, sacada con `claude-in-chrome` en vez de un placeholder inventado). *Alcance recortado a propósito*: no se hizo en cada template individual — los templates de este repo son demos del catálogo, no links que se comparten con invitados reales; eso se resuelve al momento de cada entrega real (Fase 4), con los datos de ESE cliente, no con datos de ejemplo.
- [x] **Precio visible**: badge "Desde $22.000 — precio de lanzamiento" en el hero, visible sin scrollear (dentro del rango $20-25k definido en la Fase 0 — ajustable si el número exacto no convence).
- [x] **Favicon** (💓, a tono con el nombre "Late") **y "Late" aplicado como marca** en `<title>`, nav y wordmark del hero — ya no dice "Invitaciones Digitales" genérico en ningún lado visible.
- [x] **Botón de WhatsApp de negocio real**: `5491133536102` (el mismo número que ya usaba la invitación de Aitana, de la mujer de Norberto) — pero **solo en el catálogo** (`WHATSAPP_NEGOCIO` en `index.html`, el botón "Elegir este estilo" que es una consulta de venta). Los templates individuales mantienen su placeholder genérico (`5491100000000`, o `...001`/`...002` en el caso del RSVP doble de "dulce") porque **ese número lo define cada cliente en su pedido** (ya está en el checklist del flujo de entrega de la Fase 0) — no correspondía poner el número de la mujer de Norberto ahí, se corrigió.

---

## Fase 2 — Que el RSVP cumpla lo que promete ✅ Backend listo y probado, ⏸ desactivado en el catálogo hasta Fase 4

- [x] **Código del backend listo** (`rsvp-apps-script.md`, en la raíz del repo): Google Sheet + Apps Script `doPost`/`doGet`, con los pasos exactos para desplegarlo.
- [x] **El motor ahora captura el nombre real**: se agregó un campo "Tu nombre" antes del botón de confirmar, en los 15 templates. Al tocar "Confirmar", el nombre se suma al mensaje de WhatsApp (`Soy <nombre>. <mensaje original>`) y se dispara un `fetch()` de registro (no hace nada si `RSVP_LOG_URL` está vacío). Caso especial resuelto: el template "dulce" (RSVP doble mamá/papá) comparte un solo campo de nombre entre los dos botones; "despedida-soltera/confetti" mantiene su animación de confetti intacta, combinada con el registro nuevo (dos listeners independientes sobre el mismo botón).
- [x] Verificado con `claude-in-chrome` (no solo leído): el nombre llega bien armado al link de WhatsApp, sin errores de consola, en un caso simple (bodas/rústico) y en el caso especial de doble botón (dulce).
- [x] **Apps Script desplegado en producción y probado de punta a punta** (2026-08-13, cuenta `norbertoloyola@gmail.com`, con permiso explícito de Norberto): Sheet "Late — RSVP" + Web App con acceso "Cualquiera" y ejecución "Yo". **Planilla propia por venta**, no una sola mezclada (ver detalle en `rsvp-apps-script.md`): "Late — RSVP" es el **índice** (una fila por evento, con el link de su planilla); la primera confirmación de cada evento crea automáticamente una planilla nueva (`Late RSVP — <evento>`), compartida como "cualquiera con el link puede ver" — esa es la que se le pasa al cliente en la entrega.
- [x] **`RSVP_LOG_URL` desactivado a propósito en los 15 templates del catálogo público** (vuelto a `""`, 2026-08-13): el motor no distingue una confirmación real de alguien probando el catálogo demo — si hubiera quedado la URL real cargada ahí, cualquier visita pública podía crear planillas reales en el Drive de Norberto. La URL real solo se va a cargar en la copia personalizada de cada venta, una vez resuelta la Fase 4 (repo de entregas separado del catálogo). Hasta entonces, el RSVP del catálogo funciona igual que antes: abre WhatsApp con el nombre incluido, sin registrar nada.
- [ ] **Falta, recién con la Fase 4**: al armar la entrega de una venta real, cargar `RSVP_LOG_URL` (la misma URL de siempre, ya desplegada) en el `CONFIG` personalizado de esa venta — ahí sí las confirmaciones son reales y corresponde que caigan en la planilla.

---

## Fase 3 — Fotos reales

No hace falta un sistema de upload todavía — eso es Fase 6.

- [x] **Catálogo público: sacar los placeholders "Foto 1", "Foto 2"...** (2026-08-13). Los 13 templates con galería (`aniversario/carrusel` y `baby-shower/reveal` no tienen sección de fotos, no aplica) ahora muestran fotos reales de stock con licencia libre de uso comercial (Unsplash), elegidas para que encajen con cada categoría — no genéricas. Se descargaron al repo (`templates/<categoría>/img/foto-N.jpg`) en vez de enlazar directo a Unsplash, para no depender de su CDN. Criterio de selección aplicado a mano en cada foto candidata, no solo por relevancia de búsqueda:
  - Sin personajes con copyright/marca reconocible (se descartaron fotos con Disney, Marvel, PAW Patrol, Krispy Kreme, LEGO visibles).
  - Sin nombres, fechas ni datos de clientes reales horneados/escritos en la foto (se descartaron varias tortas con nombres de bebés reales).
  - Ocasión correcta (se descartaron fotos de Año Nuevo, Navidad y "sweet 16" que aparecían mezcladas en los resultados de búsqueda).
  - Rango etario correcto para cumple-infantil (se descartaron fotos de adultos/adolescentes en la búsqueda de "10-12 años").
  - El catálogo demo ahora se ve como un producto terminado, no como una maqueta a medio hacer — esto era parte de la crítica original del negocio.
- [ ] **Entrega real de un cliente**: al personalizar el `CONFIG` de una venta, reemplazar las fotos de stock por las que el cliente mande (por WhatsApp, Google Drive con link público, etc.) — sigue siendo 100% manual, está bien mientras el volumen sea bajo. Documentado como parte del flujo de entrega de la Fase 0.

---

## Fase 4 — Dónde vive cada invitación vendida ✅ Repo listo, falta la primera entrega real

- [x] **Segundo repo creado y publicado** (2026-08-13): [`invitaciones-entregas`](https://github.com/NorbertoLoyola/invitaciones-entregas), separado del catálogo público, GitHub Pages activo sobre `main` → `https://norbertoloyola.github.io/invitaciones-entregas/<carpeta-de-la-venta>/`. Repo público (GitHub Pages privado necesita plan pago Pro/Team, que no tenés hoy) — decisión consciente: mientras el volumen sea bajo, no importa que el repo en sí sea técnicamente navegable en github.com.
- [x] **Convención de entrega documentada** en el `README.md` de ese repo: cada venta = una carpeta nueva (`nombre-evento-fecha/`), copiando el template del catálogo, con el `CONFIG` personalizado, las fotos reales del cliente (Fase 3) y `RSVP_LOG_URL` cargado con la URL real del Apps Script (Fase 2) — sin eso el RSVP no queda registrado.
- [ ] El link que le pasás al cliente es de ese repo, nunca del catálogo de demostración — se aplica recién con la primera venta real (Fase 5).
- [ ] Más adelante (cuando haya ingresos): dominio propio corto, con cada invitación en una ruta linda (`tuinvitacion.com.ar/nombre-evento`) en vez de una URL de GitHub Pages.

---

## Fase 5 — Primera venta real (el paso que más importa)

Todo lo de arriba es preparación. Esto es lo que realmente mueve la aguja:

- [ ] Conseguir **3 a 5 clientes beta**, gratis o con descuento fuerte, a cambio de: (a) permiso para mostrar su invitación real en el catálogo, (b) un comentario real que reemplace los testimonios marcados "Ejemplo".
- [ ] Usar el flujo completo de punta a punta con cada uno (Fase 0 a 4) — esto también valida si el flujo manual definido realmente funciona o hay que ajustarlo.
- [ ] Con las primeras entregas reales, actualizar el catálogo: sacar el badge "Ejemplo" de los testimonios que ya sean reales, y sumar capturas reales si querés (más adelante, variar el banner del hero para incluir alguna).

---

## Fase 6 — Canales de venta (en paralelo a la Fase 5)

- [ ] Instagram no alcanza solo. Este mercado se mueve por referidos: **salones de fiesta, organizadores de eventos, fotógrafos de bodas/15, catering**. Un acuerdo de comisión (ej. 10-15% por venta referida) con 2-3 de esos actores locales trae más volumen que postear sin seguidores.
- [ ] Contenido de Instagram recién tiene sentido *después* de tener el catálogo con precio, marca, y al menos una entrega real para mostrar — publicar antes es gastar el primer impacto en algo incompleto.

---

## Fase 7 — Automatización y pagos (recién acá, no antes)

No arrancar por acá, aunque sea lo más "atractivo" de construir.

- [ ] Automatizar la generación del `CONFIG` por cliente (formulario → genera el HTML solo, sin editar código a mano) — tiene sentido una vez que el volumen manual empieza a doler, no antes.
- [ ] Integrar Mercado Pago en el botón "Elegir este estilo" (reemplazando el `mpLink` vacío que ya está preparado en el catálogo).
- [ ] Recién con volumen real, evaluar dominio propio + mejoras de Fase 4.

---

## Resumen de qué es gratis y qué no

| Fase | Costo | Bloquea a las ventas |
|---|---|---|
| 0 — Decisiones | Gratis, solo tiempo | Sí, es la base de todo |
| 1 — OG tags, precio, marca | Gratis | Sí (afecta primera impresión) |
| 2 — RSVP real (Sheet + Apps Script) | Gratis | No, pero es la promesa central del producto |
| 3 — Fotos reales | Gratis (manual) | Sí, para cualquier venta real |
| 4 — Dónde entregar | Gratis (otro repo) | Sí, sin esto no hay dónde mandar al cliente |
| 5 — Primera venta beta | Costo = regalar 3-5 invitaciones | Es el objetivo, no un bloqueo |
| 6 — Canales | Gratis (tiempo/relaciones) | No bloquea, acelera |
| 7 — Automatización + MP | Tiempo de desarrollo | No, es optimización sobre algo que ya vende |
