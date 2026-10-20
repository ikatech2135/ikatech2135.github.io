# Guía de mantenimiento — Ikatech

Manual personal para no olvidar ningún paso y poder cambiar la estética de la página.

**Última actualización:** 2026-10-20

---

## 0. Datos del proyecto

| Dato | Valor |
| :--- | :--- |
| **Página publicada** | https://ikatech2135.github.io/ |
| **Repositorio** | https://github.com/ikatech2135/ikatech2135.github.io |
| **Usuario GitHub** | `ikatech2135` |
| **Instagram** | [@ikatech.libertad.uy](https://instagram.com/ikatech.libertad.uy) |
| **Email** | ikatech.libertad.uy@gmail.com |
| **Hoja de registro** | Google Sheets (renombrar a "Registro de descargas") |
| **Endpoint Apps Script** | `https://script.google.com/macros/s/AKfycbyqBkXX…/exec` |

> El repo **debe** llamarse `ikatech2135.github.io` (minúsculas, con punto) para que la URL sea `https://ikatech2135.github.io/` sin sufijo.

---

## 1. Estructura del proyecto

```
ikatech2135.github.io/      ← nombre exacto del repo
├── index.html              ← la página completa
├── GUIA-IKATECH.md         ← este documento
└── descargas/              ← (opcional) tus .zip
    ├── proyecto-a-1.0.zip
    └── proyecto-b-0.5.zip
```

---

## 2. Migración desde `ikari2011` (referencia)

1. Verificar que `ikatech2135` esté libre en `https://github.com/ikatech2135`
2. Settings → Account → Change username → `ikatech2135`
3. Renombrar el repo `ikatech-web` a `ikatech2135.github.io` (Settings → General)
4. Settings → Pages → `main` / `/ (root)` → Save
5. Esperar 1–2 min → verificar `https://ikatech2135.github.io/`

**Apps Script, hoja y endpoint NO cambian.**

---

## 3. Añadir un proyecto nuevo

**Paso 1 — Copia la tarjeta.** Duplica un `<article class="project">` dentro de `<main>`.

**Paso 2 — Cambia los datos visibles:**
- `<h2>Nombre del proyecto</h2>`
- `<p class="desc">` con la descripción
- Los dos `<span class="badge">`: versión y licencia

**Paso 3 — Cambia los datos del botón:**

```html
<button class="btn btn-primario"
        data-url="descargas/mi-proyecto-1.0.zip"
        data-terms="terminos-mipro"
        data-project="mi-proyecto"
        data-version="1.0">
  <svg ...>...</svg>
  Descargar
</button>
```

**Rutas de `data-url`:**

| Dónde está el .zip | Valor de `data-url` |
| :--- | :--- |
| Carpeta `descargas/` | `descargas/mi-proyecto-1.0.zip` |
| GitHub Releases | `https://github.com/ikatech2135/ikatech2135.github.io/releases/download/v1.0/mi-proyecto-1.0.zip` |
| Otro dominio | URL completa con `https://` |

**Paso 4 — Crea su `<template>`** (fuera de `<main>`):

```html
<template id="terminos-mipro">
  <h3>Nombre de la licencia</h3>
  <p>Texto completo de los términos…</p>
</template>
```

**Paso 5 — Commit changes** en GitHub. En 1 min se actualiza.

> ⚠️ Cada proyecto necesita un `data-terms` distinto.

---

## 4. Cambiar la estética

Todo en las **variables CSS** de `:root`. Cambiar solo los valores de la derecha.

### 4.1 Colores

```css
--primario:          #6366f1;
--primario-hover:    #4f46e5;
--primario-gradient: linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%);
```

**Paletas listas:**

| Nombre | primario | hover | gradient |
| :--- | :--- | :--- | :--- |
| Índigo (actual) | `#6366f1` | `#4f46e5` | `linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%)` |
| Azul | `#2563eb` | `#1d4ed8` | `linear-gradient(135deg, #2563eb 0%, #3b82f6 100%)` |
| Verde | `#16a34a` | `#15803d` | `linear-gradient(135deg, #16a34a 0%, #22c55e 100%)` |
| Rosa | `#db2777` | `#be185d` | `linear-gradient(135deg, #db2777 0%, #ec4899 100%)` |
| Naranja | `#ea580c` | `#c2410c` | `linear-gradient(135deg, #ea580c 0%, #f97316 100%)` |
| Negro | `#111827` | `#000000` | `linear-gradient(135deg, #111827 0%, #374151 100%)` |

### 4.2 Tema oscuro

```css
--fondo:         #0f172a;
--fondo-tarjeta: #1e293b;
--fondo-suave:   #334155;
--texto:         #f1f5f9;
--texto-medio:   #cbd5e1;
--texto-suave:   #94a3b8;
--borde:         #334155;
--overlay:       rgba(0,0,0,.75);
--badge-version-bg: #1e3a8a;
--badge-license-bg: #14532d;
```

### 4.3 Tipografía

```css
--fuente: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
```

### 4.4 Tamaños

```css
--tam-h1: 2.4rem;
--tam-h2: 1.35rem;
--tam-h3: 1.05rem;
```

### 4.5 Forma

```css
--radio:       14px;
--radio-boton: 10px;
```

### 4.6 Ancho

```css
--ancho-max: 760px;
```

### 4.7 Badges

```css
--badge-version-bg:  #eff6ff;
--badge-version-txt: #1d4ed8;
--badge-license-bg:  #f0fdf4;
--badge-license-txt: #15803d;
```

---

## 5. Apps Script (referencia)

```js
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    if (sheet.getLastRow() === 0) {
      sheet.appendRow(['timestamp', 'project', 'version']);
    }
    sheet.appendRow([
      new Date(),
      e.parameter.project || '(sin project)',
      e.parameter.version || '(sin version)'
    ]);
    return ContentService.createTextOutput(JSON.stringify({ ok: true }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ ok: false, error: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet() {
  return ContentService.createTextOutput('Endpoint activo.');
}
```

**Recordatorio:** cada vez que edites el script → `Implementar → Administrar implementaciones → ✏️ → Nueva versión → Implementar`.

**Si la URL del `/exec` cambia**, actualízala en `index.html`:
```html
<form id="formLog" ... action="https://script.google.com/macros/s/NUEVA/exec">
```

---

## 6. Publicar cambios

**Sin terminal:**
1. Abre el repo → `index.html`
2. Lápiz ✏️ → edita
3. **Commit changes** (rama `main`)
4. Espera 1 min

**Subir archivos nuevos:** Add file → Upload files → arrastra → Commit changes.

---

## 7. Consultar descargas

Hoja de cálculo → cada fila es una descarga.

**Fórmulas útiles en otra pestaña:**

Total por proyecto:
```
=QUERY('Sheet1'!A:C; "SELECT B, COUNT(C) WHERE B IS NOT NULL GROUP BY B LABEL COUNT(C) 'Descargas'"; 0)
```

Últimos 7 días:
```
=QUERY('Sheet1'!A:C; "SELECT TO_DATE(A), COUNT(B) WHERE A IS NOT NULL GROUP BY TO_DATE(A) ORDER BY TO_DATE(A) DESC LIMIT 7"; 0)
```

Versión más descargada por proyecto:
```
=QUERY('Sheet1'!A:C; "SELECT B, C, COUNT(A) WHERE B IS NOT NULL GROUP BY B, C ORDER BY B, COUNT(A) DESC"; 0)
```

---

## 8. Activar donaciones

Dentro de `<main>`, antes del `</main>`:

```html
<section id="donaciones" style="margin-top:3rem; padding-top:2rem; border-top:1px solid var(--borde);">
  <h2>Apoyar el proyecto</h2>
  <p style="color:var(--texto-medio);">
    Si estos proyectos te resultan útiles, puedes contribuir a su mantenimiento.
    Las donaciones son voluntarias y no otorgan licencia adicional sobre los proyectos.
  </p>
  <div style="display:flex; gap:.75rem; flex-wrap:wrap; margin-top:1rem;">
    <a class="btn btn-primario" href="https://ko-fi.com/TUUSUARIO" target="_blank" rel="noopener">Ko-fi</a>
    <a class="btn btn-secundario" href="https://paypal.me/TUUSUARIO" target="_blank" rel="noopener">PayPal</a>
  </div>
</section>
```

Opciones: Ko-fi (0 %), Buy Me a Coffee (5 %), PayPal.me, GitHub Sponsors.

---

## 9. Checklist antes de publicar

- [ ] Cabecera y tagline correctos
- [ ] Proyecto de ejemplo reemplazado
- [ ] Cada proyecto tiene su `<template>` con `id` único
- [ ] `data-project` sin espacios ni mayúsculas
- [ ] `data-url` apunta a archivos existentes
- [ ] URL del `action` en `formLog` coincide con Apps Script
- [ ] Descarga completa probada (modal → aceptar → descarga → fila en hoja)
- [ ] Instagram y email correctos
- [ ] Año de copyright se actualiza solo

---

## 10. Problemas frecuentes

| Síntoma | Solución |
| :--- | :--- |
| No aparece la fila en la hoja | Abrir URL `/exec` → debe decir "Endpoint activo." |
| Error `doGet` | Falta la función `doGet()` en el script |
| "Página no encontrada" en `/exec` | Implementación mal configurada → volver a implementar |
| Modal no abre | F12 → Consola → verificar `data-terms` coincide con `id` del template |
| 404 en `ikatech2135.github.io` | El archivo debe llamarse `index.html` en la raíz del repo |
| Candado amarillo HTTPS | Esperar unos min tras el primer deploy |
| Cambio no se refleja | Esperar 1–2 min; recargar con Ctrl+Shift+R |

---

## 11. Conectar dominio propio (futuro)

1. Comprar dominio en `nic.com.uy` (.uy) o Namecheap/Cloudflare (.com)
2. GitHub → **Settings → Pages → Custom domain** → `tudominio.com` → Save
3. En el registrador:
   - **Raíz**: 4 registros A → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - **www**: 1 CNAME → `ikatech2135.github.io`
4. GitHub → Settings → Pages → **Enforce HTTPS**
5. Esperar propagación DNS

La URL `ikatech2135.github.io` seguirá funcionando como redirección.

---

## 12. Ideas futuras

- Contador de visitas (GoatCounter, gratis, sin cookies):
  ```html
  <script data-goatcounter="https://TUUSUARIO.goatcounter.com/count"
          async src="//gc.zgo.at/count.js"></script>
  ```
- Sección "Sobre mí"
- Buscador (15+ proyectos)
- Filtros por etiqueta
- Política de privacidad
- Dominio propio

---

*Ikatech · ikatech.libertad.uy@gmail.com · @ikatech.libertad.uy*