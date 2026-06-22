# 📸 Escáner de tickets → Holded

Web-app para móvil (iOS/Android) que captura un ticket con la cámara, lo
"escanea" (detección automática de bordes + ajuste manual de esquinas, estilo
app de Notas), corrige la perspectiva y lo envía por email al buzón del escáner
de Holded (`remottosl@holdedbox.com`). La foto **nunca se guarda en el móvil**.

## Cómo funciona

- **Página estática** (`index.html`) → se sube a GitHub Pages. Usa la cámara en
  vivo (`getUserMedia`), por eso la foto no llega al carrete: no hay nada que
  borrar.
- **Envío del email** → llama a un endpoint en tu Vercel
  (`/api/purchases?action=scan`) que reenvía el ticket vía **Resend** (el mismo
  que ya usas para los emails semanales). Como Holded acepta cualquier
  remitente, no hace falta Gmail.
- **Seguridad** → el endpoint pide un **PIN** (`SCANNER_SECRET`). Se introduce
  una vez en el móvil y se guarda en `localStorage`. Así nadie que encuentre la
  URL puede spamear Holded con tu cuenta de Resend.

## Puesta en marcha (una sola vez)

### 1. Configura el PIN en Vercel
En tu proyecto de Vercel → Settings → Environment Variables, añade:

```
SCANNER_SECRET = (un PIN a tu elección, p.ej. 7421)
```

Redespliega para que la variable surta efecto. `RESEND_API_KEY` ya está
configurada.

### 2. Sube la página a GitHub Pages
Sube `index.html` a un repo y activa GitHub Pages (Settings → Pages → rama
`main`, carpeta raíz). Te dará una URL tipo
`https://tuusuario.github.io/tu-repo/`.

> Si tu dominio de Vercel **no** es `financecontrolpanelremotto.vercel.app`,
> edita la constante `API_URL` al principio del `<script>` en `index.html`.

### 3. Úsalo
Abre la URL en el móvil → la primera vez te pide el PIN → apunta al ticket →
captura → ajusta esquinas → **Enviar al escáner** → "✅ Foto enviada al escáner".

Para abrirla rápido: en iPhone, Compartir → "Añadir a pantalla de inicio".

## Notas

- HTTPS es obligatorio para la cámara. GitHub Pages ya sirve por HTTPS. ✔️
- Cambiar el PIN en el móvil: icono ⚙︎ arriba a la derecha.
- La detección de bordes usa OpenCV.js (~8 MB, se carga al abrir). Si tarda o
  falla, el editor manual de las 4 esquinas funciona igual.
