# Cumpleaños infantil — subestilos por rango de edad

Categoría madre "cumpleaños infantil", segmentada en cuatro subrangos etarios. Cada subcarpeta va a tener su propio `index.html` con el sistema `CONFIG` (mismo patrón que `templates/bodas/` y `templates/quince/`) una vez que se genere el template.

| Carpeta | Rango | Estilo | Motivos decorativos sugeridos | Paleta sugerida |
|---|---|---|---|---|
| `0-2-anos/` | 0–2 años (1er/2do añito) | Suave/pastel — hereda el lenguaje visual de `base-aitana/` | nubes, lunas, globitos, ositos genéricos | pasteles (rosa/celeste/crema) |
| `3-5-anos/` | 3–5 años (jardín/preescolar) | Colorido y juguetón | animalitos de granja o selva genéricos, dinosaurios, circo, hadas/princesas genéricas | colores vivos saturados |
| `6-9-anos/` | 6–9 años (escolares) | Temáticas con "acción" | superhéroes genéricos, espacio/astronautas, fútbol, unicornios/arcoíris, ciencia/experimentos | contraste fuerte, varía según el tema elegido por subestilo |
| `10-12-anos/` | 10–12 años (preadolescentes) | Más "cool", menos infantil | gamer/pixel-art, neón/glow party, skate/urbano, dance | neón sobre fondo oscuro, o monocromo con acento de color |

## Regla de copyright (heredada del `CONTEXTO.md` general)

Ningún subestilo puede reproducir personajes con copyright (Dora la Exploradora, Plim Plin, superhéroes de marca registrada, etc.). Usar siempre:
- Diseños **inspirados** en la temática con personajes/elementos genéricos propios (ej: "astronauta genérico" en vez de un personaje de Disney/Pixar puntual), o
- Clip art con licencia comercial que el cliente ya tenga comprado.

## Progreso

- `0-2-anos/index.html` — generado. Reusa el lenguaje visual pastel de `base-aitana/` (fondo con gradiente radial suave, elementos flotantes tipo globitos, título con efecto de tipeo letra por letra) pero migrado al motor `CONFIG` de `bodas/`/`quince/` (mismo patrón de secciones: hero, countdown, itinerario/lugar, nota, galería, mensaje, RSVP, footer). Sin sección de dress code — se reemplazó por una sección "Un dato" (`CONFIG.nota`) para avisos tipo "no hace falta traer regalo" o info de la mesa dulce.
- `3-5-anos/index.html` — generado, tema selva/animalitos genéricos (hojas, mariposas, huellitas vía emoji — sin depender de clip art con licencia ni de personajes con marca). Paleta vivos: verde, amarillo, turquesa, coral. Mismo motor `CONFIG` y misma estructura de secciones que `0-2-anos/`. Es un subestilo dentro del rango 3-5; otros motivos posibles (dinosaurios, circo, hadas/princesas genéricas) quedan como variantes futuras del mismo rango, no reemplazan a este.
- `6-9-anos/index.html` — generado, tema espacial/astronautas genéricos (estrellas, cohete, planeta, luna vía emoji — sin personajes con marca). Fondo oscuro con acentos neón (cian/magenta/oro) para el contraste fuerte que pide este rango; `color-scheme: dark only`, a diferencia de los subestilos claros de 0-2 y 3-5 años. Mismo motor `CONFIG` y misma estructura de secciones, con etiquetas de sección tematizadas ("La misión", "La tripulación"). Otros motivos posibles para este rango (superhéroes genéricos, fútbol, ciencia/experimentos) quedan como variantes futuras.
- `10-12-anos/index.html` — generado, tema gamer/pixel-art/neón (joystick, control, invasor 8-bit, rayo vía emoji — sin personajes ni consolas de marca). Tipografía 'Press Start 2P' (pixel) para títulos/nombre + 'Chakra Petch' para el cuerpo — estética más "cool" y menos infantil que el resto de la categoría, como pedía la definición del rango. `color-scheme: dark only`. Mismo motor `CONFIG` y misma estructura de secciones, con etiquetas tematizadas ("La partida", "Capturas", "Cargando...").

Con este ya están los cuatro subestilos base de la categoría generados.
