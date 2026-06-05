# 🔐 Configuración del formulario de "Solicitar CV"

Esta guía te lleva paso a paso para conectar el formulario de la página con un Google Form que captura los datos de cada solicitud en una hoja de cálculo y te avisa por correo.

**Tiempo estimado:** 15-20 minutos.
**Costo:** $0 (Google Workspace gratuito).
**Resultado:** Cada solicitud te llega por email + queda registrada en una Google Sheet con trazabilidad completa.

---

## 📋 Resumen del flujo

```
Visitante hace click en "Solicitar CV"
  ↓
Llena el formulario en el modal de tu página
  ↓
Los datos se envían silenciosamente a tu Google Form
  ↓
Google Sheet se actualiza automáticamente
  ↓
Tú recibes notificación por email
  ↓
Tú revisas la solicitud (¿reclutador legítimo? ¿empresa real?)
  ↓
Tú envías el CV por email manualmente al solicitante
```

---

## Paso 1 — Crear el Google Form

1. Ve a **[forms.google.com](https://forms.google.com)** y crea un formulario en blanco.
2. Dale un título: **"Solicitud de CV — Sergio Triviño"**.
3. Descripción (opcional): *"Registro interno de solicitudes de CV desde sergiotrivino.github.io"*.
4. Crea las **6 preguntas** en este orden exacto, con estos tipos:

| # | Pregunta | Tipo | Obligatoria |
|---|---|---|---|
| 1 | Nombre completo | Respuesta corta | ✅ Sí |
| 2 | Email | Respuesta corta | ✅ Sí |
| 3 | Empresa o institución | Respuesta corta | ✅ Sí |
| 4 | Cargo / rol | Respuesta corta | ✅ Sí |
| 5 | Motivo del interés | Párrafo | ✅ Sí |
| 6 | LinkedIn (opcional) | Respuesta corta | ❌ No |

> ⚠️ **Importante:** El orden y los tipos de pregunta deben coincidir. No agregues otras preguntas en medio.

---

## Paso 2 — Conectar a una Google Sheet

1. En el Google Form, ve a la pestaña **"Respuestas"** (arriba).
2. Click en el **icono verde de Sheets** (esquina superior derecha de "Respuestas").
3. Selecciona **"Crear una hoja de cálculo nueva"** y dale un nombre: **"CV Requests — Sergio Triviño"**.
4. Click en **"Crear"**.

Ahora cada solicitud aparecerá automáticamente como una fila nueva en esa Sheet.

---

## Paso 3 — Activar notificaciones por email

1. Sigue en la pestaña **"Respuestas"** del Google Form.
2. Click en los **3 puntos verticales (⋮)** arriba a la derecha.
3. Click en **"Recibir notificaciones por correo electrónico de respuestas nuevas"**.

Cada vez que alguien envíe el formulario, recibirás un email instantáneo en `sergio.trivino97@gmail.com`.

---

## Paso 4 — Obtener el Form ID y los IDs de campo

Esta es la parte un poco técnica pero no es difícil. Sigue exactamente estos pasos.

### 4.1 — Obtener el FORM ID

1. En el Google Form, click en **"Enviar"** (botón azul arriba a la derecha).
2. Selecciona el icono de **enlace 🔗** y copia el URL. Se ve así:
   ```
   https://docs.google.com/forms/d/e/1FAIpQLSdXXXXXXXXXXXXXXXXXXXXXXXXXX/viewform
   ```
3. **El Form ID es la parte entre `/e/` y `/viewform`.** En el ejemplo de arriba sería:
   ```
   1FAIpQLSdXXXXXXXXXXXXXXXXXXXXXXXXXX
   ```
4. Cópialo y guárdalo en un bloc de notas — lo usaremos en el paso 5.

### 4.2 — Obtener los IDs de cada campo (entry.XXXXXXXXX)

Esto requiere abrir el formulario en modo de "vista previa" e inspeccionar el HTML. Es más fácil de lo que suena.

1. Abre el Google Form como lo vería un visitante: usa el URL de `viewform` que copiaste arriba (o haz click en el ícono del ojo 👁 arriba a la derecha del editor).
2. Haz **click derecho en cualquier parte de la página** → **"Inspeccionar"** (o presiona `Ctrl+Shift+I` en Windows / `Cmd+Option+I` en Mac).
3. Se abrirá el panel de DevTools del navegador. Ve a la pestaña **"Console"** (no "Elements").
4. Pega este código en la consola y presiona Enter:

   ```javascript
   document.querySelectorAll('div[data-params]').forEach((el, i) => {
     const m = el.getAttribute('data-params').match(/"(\d{6,})"/);
     if (m) console.log((i+1) + '. entry.' + m[1]);
   });
   ```

5. Verás algo como:
   ```
   1. entry.1234567890
   2. entry.0987654321
   3. entry.1122334455
   4. entry.5544332211
   5. entry.9988776655
   6. entry.4433221100
   ```

6. **El orden corresponde al orden de las preguntas en tu Form**. Anótalos así:

   | Pregunta | ID |
   |---|---|
   | 1. Nombre completo | `entry.1234567890` |
   | 2. Email | `entry.0987654321` |
   | 3. Empresa o institución | `entry.1122334455` |
   | 4. Cargo / rol | `entry.5544332211` |
   | 5. Motivo del interés | `entry.9988776655` |
   | 6. LinkedIn | `entry.4433221100` |

> 💡 **Si el script no te devuelve nada:** método alternativo. Llena el formulario tú mismo con datos de prueba (por ejemplo "TEST" en cada campo). Antes de enviar, abre DevTools → pestaña "Network", filtra por `formResponse`, envía el formulario, click en la petición POST que aparece, ve a "Payload" y verás todos los campos `entry.XXX` con sus valores. Esos son los IDs.

---

## Paso 5 — Pegar los IDs en `index.html`

1. Abre el archivo `index.html` en tu editor.
2. Busca este bloque cerca de la parte superior (línea ~25):

   ```javascript
   window.GFORM_CONFIG = {
     formId: "REEMPLAZAR_CON_TU_FORM_ID",
     fields: {
       name:    "entry.0000000001",
       email:   "entry.0000000002",
       company: "entry.0000000003",
       role:    "entry.0000000004",
       reason:  "entry.0000000005",
       linkedin:"entry.0000000006"
     }
   };
   ```

3. Reemplaza los valores con los tuyos:

   ```javascript
   window.GFORM_CONFIG = {
     formId: "1FAIpQLSdXXXXXXXXXXXXXXXXXXXXXXXXXX",  // ← tu Form ID
     fields: {
       name:    "entry.1234567890",  // ← tu ID de "Nombre completo"
       email:   "entry.0987654321",  // ← tu ID de "Email"
       company: "entry.1122334455",  // ← tu ID de "Empresa o institución"
       role:    "entry.5544332211",  // ← tu ID de "Cargo / rol"
       reason:  "entry.9988776655",  // ← tu ID de "Motivo del interés"
       linkedin:"entry.4433221100"   // ← tu ID de "LinkedIn"
     }
   };
   ```

4. Guarda el archivo.

---

## Paso 6 — Probar el formulario

1. Abre `index.html` en tu navegador (doble click, o sirviéndolo con `python3 -m http.server`).
2. Click en cualquier botón **"Solicitar CV"**.
3. Llena el formulario con datos de prueba.
4. Click en **"Enviar solicitud"**.
5. Verifica:
   - Que aparezca el mensaje de éxito ✓
   - Que llegue una nueva fila a tu Google Sheet
   - Que llegue el email de notificación a `sergio.trivino97@gmail.com`

Si algo falla, revisa el **Paso 4** (los IDs son lo más fácil de equivocar) y la **consola del navegador** (F12 → Console) para mensajes de error.

---

## Paso 7 — Sacar el PDF del repo público (importante)

⚠️ **Por seguridad, el archivo `Sergio_Trivino_Resume_EN.pdf` NO debe estar en tu repo de GitHub Pages**, porque cualquiera con el URL directo lo descargaría sin llenar el formulario.

### Opciones para almacenar el CV de forma privada:

#### Opción A — Google Drive (recomendada)

1. Sube el PDF a tu Google Drive.
2. Click derecho → "Compartir" → mantén en **"Restringido"** (solo personas con acceso).
3. Cuando apruebes una solicitud, en el email de respuesta:
   - Adjunta el PDF directamente, **o**
   - Click derecho en el archivo → "Compartir" → agrega el email del solicitante con permiso "Lector" → copia el enlace y mándaselo. Puedes revocar el acceso cuando quieras.

#### Opción B — Adjuntar al email cada vez

Más simple: tienes el PDF guardado en tu computador y lo adjuntas manualmente al email de respuesta. Para 5-10 solicitudes al mes es perfectamente manejable.

> Tu CV ya **NO** debe estar en la carpeta del sitio. La página solo redirige al modal del formulario.

---

## Paso 8 — Plantilla de email para responder solicitudes (opcional)

Para que respondas más rápido, te dejo una plantilla en español e inglés que puedes guardar en Gmail como plantilla:

### Español

> **Asunto:** Tu solicitud de CV — Sergio Triviño
>
> Hola [Nombre],
>
> Gracias por tu interés en mi perfil. Adjunto encontrarás mi CV actualizado.
>
> Si te interesa conversar sobre [empresa / posición / proyecto que mencionaron], me encantaría agendar una llamada. Mi calendario está abierto los [días] entre [horas] (GMT-5).
>
> Saludos,
> Sergio Triviño Ortega
> Biomedical Engineer
> sergiotrivino.github.io

### English

> **Subject:** Your CV request — Sergio Triviño
>
> Hi [Name],
>
> Thanks for your interest in my profile. Please find my updated CV attached.
>
> If you'd like to discuss [company / position / project they mentioned], I'd be happy to schedule a call. My calendar is open on [days] between [hours] (GMT-5).
>
> Best,
> Sergio Triviño Ortega
> Biomedical Engineer
> sergiotrivino.github.io

---

## 🛡️ Sobre la protección anti-spam

El formulario incluye dos protecciones:

1. **Honeypot field** — un campo invisible para humanos pero que los bots automáticos llenan. Si ese campo trae texto, la solicitud se descarta silenciosamente (el bot cree que envió bien, pero nada llegó).
2. **Validación cliente** — todos los campos requeridos deben estar bien llenos para enviar.

Si en algún momento te llega spam, podemos agregar reCAPTCHA o validación más estricta de email. Por ahora, con el honeypot y la fricción del formulario, debería ser suficiente.

---

## 📊 Bonus — Email automático "Solicitud recibida" (Apps Script)

Si quieres que **al solicitante** le llegue automáticamente un email diciendo "Recibí tu solicitud, te respondo en 24h" (muy profesional), agrega esto a tu Google Sheet:

1. En tu Google Sheet, ve a **Extensiones → Apps Script**.
2. Borra el código existente y pega esto:

   ```javascript
   function onFormSubmit(e) {
     const values = e.values;
     // Index: [0]=Timestamp, [1]=Nombre, [2]=Email, [3]=Empresa, [4]=Cargo, [5]=Motivo, [6]=LinkedIn
     const name = values[1];
     const email = values[2];
     const subject = "Recibí tu solicitud de CV — Sergio Triviño";
     const body = `Hola ${name},

   Gracias por solicitar mi CV. Recibí tu mensaje y lo estoy revisando personalmente.

   Te enviaré el CV por email en menos de 24 horas (normalmente respondo el mismo día).

   Si necesitas algo urgente, puedes escribirme directamente a sergio.trivino97@gmail.com.

   Saludos,
   Sergio Triviño Ortega
   Biomedical Engineer · Pediatric Assistive Robotics
   https://sergiotrivino.github.io`;

     MailApp.sendEmail(email, subject, body);
   }
   ```

3. Guarda (icono 💾).
4. En el menú lateral izquierdo, click en el reloj ⏰ (**Activadores**).
5. **Agregar activador**:
   - Función: `onFormSubmit`
   - Evento: `Desde la hoja de cálculo`
   - Tipo: `Al enviar el formulario`
6. Guarda. Te pedirá autorización (acéptala — es tu propia cuenta).

Listo. Cada solicitud nueva genera dos emails: uno **a ti** (notificación) y uno **al solicitante** (acuse de recibo).

---

## ✅ Checklist final antes de publicar

- [ ] Google Form creado con las 6 preguntas en el orden correcto
- [ ] Google Sheet conectada al Form
- [ ] Notificaciones por email activadas
- [ ] Form ID y IDs de campo pegados en `index.html`
- [ ] Probado en local: el modal envía y los datos llegan a la Sheet
- [ ] PDF removido de la carpeta del sitio
- [ ] (Opcional) Apps Script de auto-respuesta configurado
- [ ] (Opcional) Plantilla de email guardada en Gmail

---

## ❓ Solución de problemas frecuentes

**"Envío el formulario y no llega nada a la Sheet"**
→ Los IDs de `entry.XXX` están mal. Vuelve al Paso 4 y verifica el orden.

**"En la consola del navegador veo errores de CORS"**
→ Eso es normal y esperado. Google Forms bloquea la lectura de la respuesta, pero la submission sí se registra. Por eso el código usa `mode: 'no-cors'`.

**"Llegan emails al solicitante pero a mí no"**
→ Revisa que el Paso 3 esté hecho. La notificación a ti es independiente del Apps Script.

**"Quiero cambiar las preguntas o agregar campos"**
→ Cualquier cambio en el Form requiere actualizar los IDs en `index.html`. Repite los pasos 4 y 5.

**"¿Puedo migrar a otro servicio en el futuro?"**
→ Sí. Toda la lógica está en la función `cvForm.addEventListener('submit', ...)`. Si en el futuro quieres usar Formspree, Web3Forms u otro, solo cambias esa función. El resto del HTML queda igual.
