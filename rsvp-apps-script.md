# RSVP real — Google Sheet + Apps Script (Fase 2)

Backend liviano y gratis para que las confirmaciones de asistencia queden ordenadas, en vez de perdidas en mensajes de WhatsApp sueltos.

## Arquitectura: una planilla "índice" + una planilla por venta

No todo cae en una sola planilla mezclada. Hay dos niveles:

- **"Late — RSVP"** (la planilla original, tuya): funciona como **índice**. Una fila por evento: `Evento | ID Planilla | Link | Fecha de creación`. Vos la mirás para encontrar el link de cualquier venta.
- **Una planilla nueva por venta**, creada automáticamente la primera vez que confirma un invitado de ese evento (`Late RSVP — <nombre del evento>`), compartida como **"cualquiera con el link puede ver"**. Esa es la que le mandás al cliente — ve solo su lista, sin ruido de otros eventos, sin necesitar cuenta de Google.

El propio Apps Script se encarga de crear la planilla, ponerle el encabezado, compartirla y anotarla en el índice — no hace falta tocar nada a mano por cada venta nueva.

## Código (`Código.gs`)

```javascript
function doPost(e) {
  var lock = LockService.getScriptLock();
  lock.waitLock(10000);
  try {
    var datos = JSON.parse(e.postData.contents);
    var evento = datos.evento || 'Sin nombre';
    var hoja = obtenerOCrearHojaEvento(evento);
    hoja.appendRow([
      new Date(),
      datos.evento || '',
      datos.invitado || '',
      datos.fechaEvento || '',
      datos.estilo || ''
    ]);
    return ContentService.createTextOutput(JSON.stringify({ ok: true }))
      .setMimeType(ContentService.MimeType.JSON);
  } finally {
    lock.releaseLock();
  }
}

function obtenerOCrearHojaEvento(evento) {
  var indice = SpreadsheetApp.getActiveSpreadsheet();
  var hojaIndice = indice.getSheets()[0];
  var datosIndice = hojaIndice.getDataRange().getValues();

  for (var i = 1; i < datosIndice.length; i++) {
    if (datosIndice[i][0] === evento) {
      var ssExistente = SpreadsheetApp.openById(datosIndice[i][1]);
      return ssExistente.getSheets()[0];
    }
  }

  var nuevaSs = SpreadsheetApp.create('Late RSVP — ' + evento);
  var hoja = nuevaSs.getSheets()[0];
  hoja.setName('RSVP');
  hoja.appendRow(['Fecha de registro', 'Evento', 'Invitado', 'Fecha del evento', 'Estilo']);
  hoja.getRange(1, 1, 1, 5).setFontWeight('bold');

  DriveApp.getFileById(nuevaSs.getId())
    .setSharing(DriveApp.Access.ANYONE_WITH_LINK, DriveApp.Permission.VIEW);

  hojaIndice.appendRow([evento, nuevaSs.getId(), nuevaSs.getUrl(), new Date()]);

  return hoja;
}

function doGet(e) {
  return ContentService.createTextOutput('Late RSVP Logger activo.');
}
```

`LockService` evita que dos confirmaciones que lleguen casi al mismo tiempo (típico el día del evento) creen dos planillas duplicadas para el mismo evento.

## Estado actual (2026-08-13)

- [x] Planilla índice: [Late — RSVP](https://docs.google.com/spreadsheets/d/1PR52BFZqdBGf7Bw1vooeBZ4lf2nRwnPi4-X2hsQkOh4/edit)
- [x] Apps Script "Late RSVP Logger" desplegado, `Ejecutar como: Yo`, `Acceso: Cualquier usuario`.
- [x] URL en producción, ya cargada en `RSVP_LOG_URL` de los 15 templates: `https://script.google.com/macros/s/AKfycbwGLit2nqmSdkGmVQ0AkfWIrZCIO-vGibaWQ9VYZ7XsrhbU7KoK4t3XSYVMpSE_fyPB/exec`

## Flujo de entrega (agregado al checklist de la Fase 0)

Cuando entregues una venta real: una vez que llegue la primera confirmación de ese evento (podés generarla vos mismo probando el link antes de mandarlo al cliente), va a aparecer una fila nueva en el índice con el link de "su" planilla. **Ese link es lo que le pasás al cliente** junto con la invitación — así ve la lista de confirmados en tiempo real, sin tener que pedírtela a vos.

## Si más adelante hace falta cambiar el código

Extensiones → Apps Script en la planilla índice, editá, guardá, y **Implementar → Administrar las implementaciones → Editar (ícono lápiz) → Nueva versión → Implementar**. La URL `/exec` no cambia — no hace falta tocar los templates de nuevo. Si el código nuevo usa un servicio de Google que no usaba antes (como acá, que se sumó `DriveApp` para compartir), Google va a pedir volver a autorizar permisos al implementar.
