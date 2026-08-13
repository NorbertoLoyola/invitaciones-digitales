# RSVP real — Google Sheet + Apps Script (Fase 2, parte 1)

Backend liviano y gratis para que las confirmaciones de asistencia queden en una planilla, en vez de perdidas en mensajes de WhatsApp sueltos.

## Pasos

1. Andá a [sheets.google.com](https://sheets.google.com) → **Hoja de cálculo en blanco**. Ponele de nombre "Late — RSVP".
2. En la primera fila, escribí los encabezados (una vez, a mano):
   `Fecha de registro | Evento | Invitado | Fecha del evento | Estilo`
3. Menú **Extensiones → Apps Script**.
4. Borrá todo el código de ejemplo que aparece (`function myFunction() {...}`) y pegá esto:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var datos = JSON.parse(e.postData.contents);
  sheet.appendRow([
    new Date(),
    datos.evento || '',
    datos.invitado || '',
    datos.fechaEvento || '',
    datos.estilo || ''
  ]);
  return ContentService.createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}

function doGet(e) {
  return ContentService.createTextOutput('Late RSVP Logger activo.');
}
```

5. Guardá el proyecto (ícono de disquete, nombre sugerido: "Late RSVP Logger").
6. Arriba a la derecha, **Implementar → Nueva implementación**.
7. Tipo: **Aplicación web**.
8. Configuración:
   - **Ejecutar como**: Yo (tu cuenta)
   - **Quién tiene acceso**: **Cualquier usuario** (esto es importante — el que confirma es un invitado anónimo desde su celular, sin loguearse a nada)
9. Al implementar, Google te va a pedir autorizar permisos — es tu propio script, es seguro, aceptá.
10. Te va a dar una **URL que termina en `/exec`**. Copiala.
11. **Pasame esa URL** — con eso completo el `RSVP_LOG_URL` en el motor de todos los templates (ver `PLAN-COMERCIAL.md`, Fase 2 parte 2).

## Cómo probarlo vos mismo (opcional, antes de pasarme la URL)

Pegá la URL en la barra del navegador — si ves el texto "Late RSVP Logger activo.", el despliegue funcionó. Eso confirma el `doGet`; el `doPost` (que es el que realmente registra confirmaciones) se prueba solo una vez esté conectado a un template real.

## Si más adelante hace falta cambiar el código

Volvé a Extensiones → Apps Script en esta misma planilla, editá, guardá, y **Implementar → Gestionar implementaciones → Editar (ícono lápiz) → Nueva versión**. La URL no cambia.
