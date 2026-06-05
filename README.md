# sergiotrivino.github.io

Sitio web personal de **Sergio Triviño Ortega** — Biomedical Engineer.

One-page bilingüe (ES/EN), estética **Swiss Editorial** moderna, con formulario de **Solicitar CV** que captura datos para trazabilidad.

---

## 📦 ¿Qué hay en esta carpeta?

```
.
├── index.html          ← El sitio completo (HTML + CSS + JS, todo aquí)
├── SETUP.md            ← Cómo configurar el formulario de Solicitar CV (15-20 min)
├── README.md           ← Este archivo
└── img/                ← Carpeta para tus fotos (la creas tú al subir imágenes)
```

⚠️ **NO incluyas el PDF de tu CV en esta carpeta.** El sistema de "Solicitar CV" sirve precisamente para que el PDF quede privado y solo lo envíes a quienes apruebes.

---

## 🚀 Pasos para publicar

### 1. Configurar el formulario de Solicitar CV

**Esto es lo primero, antes de subir el sitio.** Sigue [`SETUP.md`](./SETUP.md) — toma 15-20 minutos.

Sin esta configuración el formulario no enviará datos a ningún lado.

### 2. Crear el repositorio de GitHub Pages

1. Crea un repo público en GitHub llamado **exactamente**:
   ```
   sergiotrivino.github.io
   ```
2. Clónalo a tu computador:
   ```bash
   git clone https://github.com/sergiotrivino/sergiotrivino.github.io.git
   cd sergiotrivino.github.io
   ```
3. Copia `index.html` y `README.md` (y la carpeta `img/` cuando agregues fotos) a esa carpeta.
4. Sube los cambios:
   ```bash
   git add .
   git commit -m "Initial portfolio site"
   git push origin main
   ```

### 3. Activar GitHub Pages

1. En GitHub, ve a tu repo → **Settings → Pages**.
2. **Source**: `Deploy from a branch` → **Branch**: `main` → **Folder**: `/ (root)` → **Save**.
3. Espera 1-2 minutos. Tu sitio queda live en:
   👉 **https://sergiotrivino.github.io**

---

## 📷 Cómo agregar las imágenes

El sitio tiene **4 placeholders de imagen** marcados claramente en `index.html`. Cuando tengas las fotos:

### Paso 1 — Crear la carpeta `img/`

En la raíz del proyecto:
```bash
mkdir img
```

### Paso 2 — Comprimir las imágenes

Usa una herramienta gratuita como [squoosh.app](https://squoosh.app) o [tinyjpg.com](https://tinyjpg.com) para comprimir cada foto **antes de subirla**. Objetivo: cada JPG debe pesar **menos de 200 KB**. Esto es crítico para que el sitio cargue rápido.

### Paso 3 — Renombrar y guardar

Renombra los archivos así (no uses espacios ni acentos):

| Archivo | Qué va aquí | Tamaño recomendado |
|---|---|---|
| `img/sergio-portrait.jpg` | Tu foto retrato profesional | 800×1000 px (vertical 4:5) |
| `img/prosthesis-1.jpg` | Vista completa de la prótesis | 800×800 px (cuadrada) |
| `img/prosthesis-2.jpg` | Detalle modular o CFRP | 800×800 px (cuadrada) |
| `img/prosthesis-3.jpg` | Ensayo mecánico o usuaria final | 800×800 px (cuadrada) |

### Paso 4 — Reemplazar los placeholders en `index.html`

Busca en `index.html` los comentarios `📷 IMAGEN` (hay 4). Cada uno te indica exactamente qué reemplazar.

**Ejemplo — placeholder del retrato:**

Antes (placeholder):
```html
<div class="photo-frame" id="photo-portrait">
  <div class="photo-placeholder">
    <svg ...>...</svg>
    <strong>Foto retrato</strong>
    <span>800×1000 px · vertical 4:5</span>
  </div>
</div>
```

Después (con tu imagen):
```html
<div class="photo-frame has-image" id="photo-portrait">
  <img src="img/sergio-portrait.jpg" alt="Sergio Triviño Ortega — Biomedical Engineer" />
</div>
```

> 💡 Si solo tienes algunas de las fotos y otras no, **deja los placeholders** en las que falten. La página se ve bien con o sin ellos. Cuando consigas las restantes, las cambias.

### Paso 5 — Sube los cambios

```bash
git add img/ index.html
git commit -m "Add photos"
git push
```

En 1-2 minutos verás los cambios en vivo en `https://sergiotrivino.github.io`.

---

## 🌐 Idiomas

El sitio es bilingüe **ES / EN**. La preferencia se guarda en el navegador del visitante.

Para cambiar cualquier texto, busca el atributo `data-es="..."` y `data-en="..."` en el HTML. Los textos visibles iniciales corresponden al español (idioma por defecto).

---

## 🛠️ Vista previa local

Antes de subir cambios a GitHub, prueba siempre en local:

```bash
# Opción 1: doble click en index.html
# (Funciona pero el formulario puede dar error de CORS al enviar)

# Opción 2 (recomendada): servir desde un servidor local
python3 -m http.server 8000
# Abre http://localhost:8000 en tu navegador
```

---

## 🔒 Sobre la seguridad de tu CV

El sistema implementado tiene estas protecciones:

- ✅ El PDF **no está en el repo público** → nadie puede descargarlo sin pasar por el formulario.
- ✅ El formulario tiene **honeypot anti-spam** → bots automáticos no pueden llenarlo.
- ✅ Cada solicitud se **registra con timestamp** en tu Google Sheet → trazabilidad total.
- ✅ **Tú decides** a quién enviar el CV y a quién no → control absoluto.

Limitaciones honestas (que conviene saber):
- Cualquier persona puede llenar el formulario con datos falsos. Tu trabajo es leer cada solicitud y decidir si la respondes.
- No hay validación automática de "este email pertenece a esta empresa" — eso lo verificas tú visualmente.
- Para volúmenes altos de spam, podemos agregar reCAPTCHA más adelante.

---

## ✏️ Cómo actualizar contenido

### Cambiar textos
Edita directamente `index.html` — todos los textos están bilingües (`data-es` y `data-en`).

### Agregar un nuevo proyecto
Busca la sección `<!-- ============ OTHER PROJECTS ============ -->` y duplica un bloque `<article class="project-row">`.

### Agregar una nueva noticia/medio
Busca la sección `<!-- ============ PRESS ============ -->` y duplica un bloque `<a class="press-row">`.

### Agregar un trabajo nuevo
Busca la sección `<!-- ============ EXPERIENCE ============ -->` y duplica un bloque `<div class="exp-row">`.

### Cambiar el color de acento
En el CSS, busca `:root { ... --accent: #FF4D2E; }` y cambia el código hexadecimal.

---

## 🌐 Custom domain (futuro)

Si más adelante compras un dominio propio (ej. `sergiotrivino.com`):

1. Crea un archivo llamado `CNAME` (sin extensión) en la raíz del repo, con este contenido:
   ```
   sergiotrivino.com
   ```
2. En tu registrar de dominio, configura un `CNAME` apuntando a `sergiotrivino.github.io`.
3. En GitHub: Settings → Pages → Custom domain → escribe tu dominio → Save → Enable HTTPS.

---

## 🆘 Solución de problemas

**"El sitio no se actualiza después de hacer push"**
→ GitHub Pages tarda 1-2 minutos. Si después de 5 min sigue sin actualizar, ve a Settings → Pages y verifica que el branch sea `main` y la carpeta `/ (root)`.

**"Las imágenes no aparecen"**
→ Verifica que el path sea `img/nombre.jpg` (minúsculas, sin acentos) y que el archivo esté efectivamente en la carpeta `img/`. Las rutas son sensibles a mayúsculas en GitHub.

**"El formulario no envía"**
→ Revisa `SETUP.md`, especialmente los IDs en `window.GFORM_CONFIG`.

**"No me llega el email cuando alguien envía el formulario"**
→ Revisa el Paso 3 del `SETUP.md`. Puede que las notificaciones no estén activadas.

---

## 📄 Licencia

Código: tuyo, haz lo que quieras.
Contenido (textos, fotos, datos personales): © Sergio Triviño Ortega — todos los derechos reservados.
Press article links: propiedad de los respectivos medios.

---

**Hecho con cuidado para presentarte de la mejor forma posible al mundo.**
