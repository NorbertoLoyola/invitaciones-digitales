# Despedida de soltera

Categoría nueva — muy pedida como invitación digital según la investigación de mercado.

| Carpeta | Estilo | Animación protagonista | Estado |
|---|---|---|---|
| `confetti/` | Rosa/dorado/violeta, divertido | Explosión de confetti (canvas, físicas simples de gravedad) al tocar "Confirmar asistencia" | Generado |

Mismo motor `CONFIG` y estructura de secciones que el resto. El confetti se dispara con un listener sobre el botón de RSVP — no bloquea la navegación a WhatsApp (que abre en pestaña nueva), así que la animación se ve igual aunque el usuario confirme.
