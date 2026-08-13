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
11. Van viendo las confirmaciones que lleguen (por WhatsApp por ahora; automatizado en Fase 2).
12. Pasado el evento, les piden permiso para usar la invitación como caso real en el catálogo (reemplaza un testimonio "Ejemplo").

---

## Fase 1 — Arreglos técnicos rápidos y gratis (1-2 días)

Todo esto no depende de que exista una venta todavía. Se hace una vez, sobre el catálogo y sobre los templates base.

- [x] **Meta tags Open Graph en `index.html`** — título, descripción, `og:image` (captura real del hero, `og-image.png`, sacada con `claude-in-chrome` en vez de un placeholder inventado). *Alcance recortado a propósito*: no se hizo en cada template individual — los templates de este repo son demos del catálogo, no links que se comparten con invitados reales; eso se resuelve al momento de cada entrega real (Fase 4), con los datos de ESE cliente, no con datos de ejemplo.
- [x] **Precio visible**: badge "Desde $22.000 — precio de lanzamiento" en el hero, visible sin scrollear (dentro del rango $20-25k definido en la Fase 0 — ajustable si el número exacto no convence).
- [x] **Favicon** (💓, a tono con el nombre "Late") **y "Late" aplicado como marca** en `<title>`, nav y wordmark del hero — ya no dice "Invitaciones Digitales" genérico en ningún lado visible.
- [x] **Botón de WhatsApp de negocio real**: `5491133536102` (el mismo número que ya usaba la invitación de Aitana, de la mujer de Norberto) — pero **solo en el catálogo** (`WHATSAPP_NEGOCIO` en `index.html`, el botón "Elegir este estilo" que es una consulta de venta). Los templates individuales mantienen su placeholder genérico (`5491100000000`, o `...001`/`...002` en el caso del RSVP doble de "dulce") porque **ese número lo define cada cliente en su pedido** (ya está en el checklist del flujo de entrega de la Fase 0) — no correspondía poner el número de la mujer de Norberto ahí, se corrigió.

---

## Fase 2 — Que el RSVP cumpla lo que promete

Hoy "vos ves quién confirmó sin planillas" es falso — el botón solo abre WhatsApp. Opción realista sin backend pago:

- [x] **Código del backend listo** (`rsvp-apps-script.md`, en la raíz del repo): Google Sheet + Apps Script `doPost`/`doGet`, con los pasos exactos para desplegarlo. **Pendiente que Norberto lo despliegue con su cuenta de Google** (no se puede hacer por él) y pase la URL resultante — con eso se completa `RSVP_LOG_URL` en los 15 templates (hoy vacío, así que no rompe nada mientras tanto).
- [x] **El motor ahora captura el nombre real**: se agregó un campo "Tu nombre" antes del botón de confirmar, en los 15 templates. Al tocar "Confirmar", el nombre se suma al mensaje de WhatsApp (`Soy <nombre>. <mensaje original>`) y se dispara un `fetch()` de registro (silencioso, no bloquea ni rompe nada si `RSVP_LOG_URL` todavía está vacío). Caso especial resuelto: el template "dulce" (RSVP doble mamá/papá) comparte un solo campo de nombre entre los dos botones; "despedida-soltera/confetti" mantiene su animación de confetti intacta, combinada con el registro nuevo (dos listeners independientes sobre el mismo botón).
- [x] Verificado con `claude-in-chrome` (no solo leído): el nombre llega bien armado al link de WhatsApp, sin errores de consola, en un caso simple (bodas/rústico) y en el caso especial de doble botón (dulce).
- [ ] **Falta**: que Norberto despliegue el Apps Script y pase la URL — recién ahí las confirmaciones empiezan a caer en la planilla de verdad. Hasta entonces, el RSVP funciona igual que antes (abre WhatsApp), solo que ahora con el nombre incluido en el mensaje.

---

## Fase 3 — Fotos reales

No hace falta un sistema de upload todavía — eso es Fase 6. Para las primeras ventas:

- [ ] Al personalizar el `CONFIG` de un cliente, reemplazar los `<div class="ph">Foto 1</div>` placeholder por `<img>` reales, con las fotos que el cliente te mande (por WhatsApp, subidas a donde sea — imgur, Google Drive con link público, etc.).
- [ ] Es 100% manual por ahora — está bien, mientras el volumen sea bajo. Documentarlo como parte del flujo de entrega de la Fase 0.

---

## Fase 4 — Dónde vive cada invitación vendida

Hoy todo cuelga del mismo repo `invitaciones-digitales` con datos de ejemplo — no hay dónde entregar una invitación real sin mezclarla con el catálogo público.

- [ ] Crear un **segundo repo** (ej. `invitaciones-entregas`), separado del catálogo público, con GitHub Pages activado.
- [ ] Cada venta = una subcarpeta nueva ahí (`/nombre-evento-fecha/index.html`), con el `CONFIG` ya personalizado y las fotos reales de la Fase 3.
- [ ] El link que le pasás al cliente es de ese repo, nunca del catálogo de demostración.
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
