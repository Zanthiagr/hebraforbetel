# HEBRA — Página web (catálogo vía WhatsApp)

Página web de catálogo para HEBRA (manillas en acero, oro laminado, rodio y piedras). Es un sitio estático de un solo archivo (`index.html`), pensado para alojarse en Netlify y conectarse a Google Sheets como catálogo editable.

## Cómo está armado

- **`index.html`** — todo el sitio: HTML, CSS y JavaScript en un solo archivo. No necesita build ni instalación de nada.
- **Catálogo en vivo desde Google Sheets** — el sitio lee los productos desde una hoja de Google publicada como CSV. Si esa hoja falla o no carga, el sitio usa automáticamente un catálogo de respaldo que ya viene escrito dentro del HTML, así nunca se queda en blanco.

## Cómo editar el catálogo (para la clienta o quien administre)

El catálogo se edita **directamente en Google Sheets**, sin tocar el código.

La hoja debe tener estas columnas exactas en la primera fila:

| Columna | Qué va ahí | Ejemplo |
|---|---|---|
| `ref` | Código de referencia (opcional, se autogenera si se deja vacío) | `HB-01` |
| `nombre` | Nombre del producto | `Bocas de Cenizas` |
| `material` | Uno de: `Acero`, `Oro Laminado`, `Rodio`, `Piedras` | `Acero` |
| `precio` | Precio en pesos, solo números | `58000` |
| `descripcion` | Texto descriptivo corto | `Acero quirúrgico en dos tonos...` |
| `foto_url` | Link directo a una foto (opcional). Si se deja vacío, se muestra un ícono ilustrado en su lugar | (vacío o un link) |
| `foto_url_2` | Segunda foto (opcional). Solo se ve en el detalle del producto, como carrusel | (vacío o un link) |
| `foto_url_3` | Tercera foto (opcional). Igual que la anterior | (vacío o un link) |
| `etiqueta` | Texto corto opcional, ej. "Más vendida" o "Exclusiva" | `Más vendida` |
| `parejas` | Marca si es un set de pareja. Escribir `si` o dejar vacío para `no` | `si` |

**Sobre la columna `parejas`:** en la página, el catálogo tiene dos pestañas independientes arriba: **"Individual"** y **"Sets de Pareja"**. Un producto marcado con `si` en esta columna aparece *solo* en la pestaña "Sets de Pareja", y uno sin marcar aparece *solo* en "Individual" — nunca se mezclan. Cada pestaña tiene sus propios filtros de material (Acero, Oro Laminado, Rodio, Piedras), independientes entre sí.

**Para que los cambios en la hoja se reflejen en la web automáticamente:**
1. La hoja de Google ya debe estar publicada en la web como CSV: `Archivo → Compartir → Publicar en la web → formato CSV`.
2. Mientras la hoja siga publicada en ese mismo link, cualquier edición (agregar fila, cambiar precio, etc.) se actualiza sola en la página — no hace falta volver a tocar el código ni resubir nada a Netlify.

⚠️ **Si algún día se necesita cambiar el link del Google Sheet** (por ejemplo, por una cuenta nueva como pasó antes), eso sí requiere editar el código: hay que cambiar el valor de `sheetCsvUrl` dentro del bloque `CONFIG` al inicio del `<script>` en `index.html`, y luego subir ese cambio a GitHub (con `git push`) para que Netlify lo despliegue.

## Archivos de imagen incluidos

- **`favicon.ico`, `favicon-16.png`, `favicon-32.png`, `favicon-48.png`, `favicon-180.png`** — el ícono que aparece en la pestaña del navegador y al guardar el sitio en el celular. Deben subirse a GitHub junto con `index.html`, en la misma carpeta (raíz del repo).
- **`og-image.png`** — la imagen que aparece como vista previa cuando se comparte el link por WhatsApp, Instagram o Facebook. También va en la raíz del repo.

⚠️ **Importante después del primer despliegue:** el `og:image` en el `<head>` de `index.html` usa una ruta relativa (`og-image.png`). Para que WhatsApp y Facebook puedan mostrar la vista previa correctamente, hay que cambiarla por la URL completa del sitio una vez Netlify asigne el dominio. Por ejemplo, si el sitio queda en `https://hebra-manillas.netlify.app`, hay que buscar en `index.html`:
```html
<meta property="og:image" content="og-image.png">
<meta name="twitter:image" content="og-image.png">
```
Y cambiarlo a:
```html
<meta property="og:image" content="https://hebra-manillas.netlify.app/og-image.png">
<meta name="twitter:image" content="https://hebra-manillas.netlify.app/og-image.png">
```
(Reemplazando por el dominio real del sitio.) Sin este paso, es posible que algunas apps no muestren la imagen al compartir el link.

## Cómo desplegar / actualizar el sitio

Este repo está pensado para conectarse a Netlify mediante GitHub (no subiendo archivos a mano), para evitar los problemas de suspensión de cuenta que da el borrar/resubir el sitio manualmente.

1. Conectar este repo de GitHub en Netlify: `Add new site → Import an existing project → GitHub → seleccionar este repo`.
2. No requiere build command. El "publish directory" es la raíz del repo (donde está `index.html`).
3. A partir de ahí, cualquier cambio en el código se despliega automáticamente al hacer:
   ```bash
   git add .
   git commit -m "Descripción del cambio"
   git push
   ```
4. **Nunca borrar el sitio en Netlify para "resetear" algo.** Si hay un problema, usar la pestaña "Deploys" de Netlify para reintentar el despliegue o volver a una versión anterior.

## Otras cosas configurables en el código

Dentro del bloque `CONFIG` al inicio del `<script>` en `index.html`:

- `whatsappNumber` — número de WhatsApp del negocio (al que llegan los pedidos).
- `instagram` — link de Instagram (actualmente es un placeholder, falta poner el real).
- `sheetCsvUrl` — link del catálogo en Google Sheets (ver sección anterior).

## Contacto del negocio (datos actuales en el sitio)

- WhatsApp: +57 320 298 8520
- Taller: Villavicencio, Meta — Colombia
