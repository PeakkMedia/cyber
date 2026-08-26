# Cyber — Peakk Media

Landing (`index.html`) + página de gracias (`gracias.html`) compartiendo una sola hoja de estilos.

## Estructura

```
cyber/
├── index.html      landing principal
├── gracias.html    página de agradecimiento (VSL + agenda)
├── style.css       CSS compartido por las dos páginas
└── assets/         imágenes y archivos locales (38 archivos)
```

## Cómo funciona el CSS compartido

`style.css` tiene tres secciones:

1. **Base compartida** — el framework de la plataforma. Era byte a byte idéntico en las dos
   páginas, así que ahora se descarga una sola vez y las dos lo usan.
2. **Index** — solo aplica a `<html class="page-index">`.
3. **Gracias** — solo aplica a `<html class="page-gracias">`.

Las secciones 2 y 3 están *scopeadas* porque las dos páginas definían 19 selectores iguales con
valores distintos (`.btn`, `h1`, `body`, `:root`, `.wrap`, `.captura`, `.eyebrow`, etc.). Sin el
scope, la última en cargar le pisaba los estilos a la otra.

**Importante:** si agregás CSS nuevo, poné el prefijo de la página que corresponda:

```css
html.page-index  .mi-clase { ... }   /* solo landing */
html.page-gracias .mi-clase { ... }  /* solo gracias */
.mi-clase { ... }                    /* las dos */
```

Y no le saques la clase al tag `<html>` de cada archivo, o la página se queda sin estilos.

## Qué se cambió respecto de los archivos originales

- Se sacaron los `<style>` inline (81 KB en index, 118 KB en gracias) a `style.css`.
- Se eliminó la carpeta `Peakk Media_files/` (el espacio en el nombre rompe rutas en hosting)
  y `index_files/`. Todo lo local vive ahora en `assets/`.
- Librerías y fuentes vuelven a cargarse desde su CDN original en lugar de la copia local:
  Google Fonts, Swiper 11, Wistia, iClosed y el runtime de LeadConnector.
- Se borraron scripts duplicados que ya reinyectan solos GTM y el pixel de Meta
  (`gtm.js`, `fbevents.js`, `gtag`, config del pixel) y el beacon de Cloudflare, que fuera de
  Cloudflare no hace nada.
- Se descartó el CSS del reproductor de Wistia (~40 KB) porque lo vuelve a inyectar `player.js`.
- El formulario de la landing ahora redirige a `gracias.html` en vez de al dominio del funnel.
- El ancla "Quiero mi llamada" de gracias pasó a ser relativa (`#reservar`).

## Archivos que faltan subir a `assets/`

Están en tus carpetas `index_files/` y `Peakk Media_files/`. Copialos **sin cambiarles el nombre**
y sacales el sufijo `.descarga` si lo tienen.

De `Peakk Media_files/`:

```
6a85fdb85113a1715f18b4ab.jpeg
6a85fdb8949d6f49c383ead4.png
tmpifqx9tdm.webp
```

Compartidos por las dos páginas (están duplicados en las dos carpetas, subí una sola copia):

```
6a85fdb8ee714bf542f8e811.png
6a85fdb98bea83db8d490311.png
6a85fdb98bea83db8d490512.png
```

De `index_files/`:

```
69a889fb6fa65843c0c2e51a.png
69a889fbedd08734e11bdf35.png
69ab1fbc36702f610221107f.png
69ab1fbc618c8d0c8832267a.jpg
69ab1fbc618c8d58e3322679.jpg
69ab1fbc7bdf38d131a774aa.jpg
69ab1fbc7bdf38ee27a774ab.png
69ab1fbc8f1e97c0cefb4c00.jpg
69ab1fbca4f7631a4a5c079e.jpg
69ab1fbcb003fa76eff24807.jpg
69ab1fbcb003fa7e0af247fc.jpg
69ab1fbcb3fc00d2e32bcaf8.jpg
69ab2b9336702f0fc7230b98.png
69ab2b93618c8d153a342475.png
69ab2b93618c8d4228342476.png
69ab2b93a4f7632d395e0db1.png
69ab2b93b3fc00024c2dccc8.jpeg
69ab2b93b3fc0081f52dccd0.jpeg
69ab2b93b3fc00bf962dcccc.png
69ab2d58618c8d03a0346ea8.jpeg
69aebb458d3eae9e709fab27.jpeg
69aebb45bfc81f582a0f34d3.jpeg
69aece32b9be70c2540980dd.png
69aed09c13dc9cef5d33959a.jpeg
69b1b71bbfc81f2f7a8fc06d.png
69b1b9987fc07ca1239cffe4.png
69b9b754dac58444ebaae572.png
69b9b87bf8443c2b85617976.png
69b9b99ddac58488d8ab161a.jpeg
69b9bb02ad02766c0c58da69.png
bundle.min.js          ← venía como bundle.min.js.descarga
tmpuqtfd50_.webp
```

## Subir al repo

1. En `github.com/PeakkMedia/cyber`, borrá las carpetas `Peakk Media_files/` e `index_files/`
   y los `index.html` / `gracias.html` viejos.
2. Subí `index.html`, `gracias.html`, `style.css` y `README.md` a la raíz.
3. Creá la carpeta `assets/` y subí ahí los 38 archivos de la lista.

En Vercel el deploy se dispara solo con el push.
