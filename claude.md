# Contexto del Proyecto: Galería Suyapa Monterroso

Este repositorio es la galería de arte de **Suyapa Monterroso**, publicada con GitHub Pages en
**suyapamonterroso.com**. Antes era un WordPress con tema Divi; hoy es un sitio estático sin
dependencias ni build: se edita un archivo y se ve en el sitio.

Si estás asistiendo a Suyapa directamente: explicale los pasos con paciencia y sin jerga.
Ella usa el sitio desde **Safari en iPhone**.

## Arquitectura

| Archivo | Qué hace |
|---|---|
| `index.html` | Galería pública. HTML + CSS + JS sin librerías. Lee `obras.json`. |
| `obras.json` | La base de datos. Un arreglo con una ficha por obra. |
| `admin.html` | PWA para publicar obras desde el iPhone. Escribe por la API REST de GitHub. |
| `manifest.json` | Permite instalar `admin.html` como app en iOS. |
| `img/obras/` | Imagen grande de cada obra (lado mayor 1600 px). |
| `img/obras/thumbs/` | Miniatura de cada obra (lado mayor 800 px), la que usa la cuadrícula. |
| `CNAME` | El dominio propio. **No borrar**: sin este archivo el dominio deja de funcionar. |

### Navegación de la galería (tres niveles)

Todo pasa dentro de `index.html`, con rutas por *hash*, así que el botón "atrás" del
teléfono funciona y cada vista se puede compartir por enlace:

- `#/` — la portada: el panel, las categorías, las colecciones y la biografía corta.
- `#/tema/Abstractos` — las obras de esa categoría.
- `#/coleccion/Ser Mujer` — las obras de esa colección.
- `#/disponibles` — solo lo que está a la venta.
- `#/biografia` — la biografía completa, con trayectoria y series.
- `#/obra/apasionada` — la ficha de una obra, con medidas, técnica y fotos de detalle.

La función `armar()` decide qué piezas se ven en cada ruta:

| | `#panel` | `#bio-corta` | `#galeria` | `#biografia` |
|---|---|---|---|---|
| portada | sí | sí | sí | no |
| categoría / colección / disponibles | no | no | sí | no |
| `#/biografia` | no | no | no | sí |

El panel y la biografía **solo pertenecen a la portada**. Dentro de una categoría
estorban: quien llegó ahí ya vino a ver obras, no a leer la presentación otra vez.
Si agregás secciones nuevas a la portada, sumalas a `armar()` o van a aparecer
en todas las vistas.

La cuadrícula de categorías no lleva encabezado a propósito. Antes decía
"Colección" justo encima del bloque "Colecciones", y las dos cosas no son lo
mismo: las categorías son temas (Abstractos, Figura Humana) y las colecciones
son series (Ser Mujer, Miradas del Alma).

### Forma de una obra en `obras.json`

```json
{
  "id": "apasionada",
  "titulo": "Apasionada",
  "temas": ["Figura Humana"],
  "coleccion": "Ser Mujer",
  "tecnica": "Óleo",
  "grupoTecnica": "Óleo",
  "medidas": "24\" x 30\"",
  "ano": null,
  "disponible": true,
  "imagen": "img/obras/apasionada.jpg",
  "thumb": "img/obras/thumbs/apasionada.jpg",
  "ancho": 1000, "alto": 1345,
  "detalles": []
}
```

- `id` es el nombre del archivo de imagen, sin extensión. Debe ser único.
- `temas` es un arreglo: una obra puede estar en más de una categoría.
- `grupoTecnica` normaliza la técnica a Óleo / Acrílico / Técnica Mixta.
- `disponible: null` significa "no sabemos"; se muestra distinto a `false`.

## De dónde salieron los datos

Las 71 obras se reconstruyeron en agosto de 2026 a partir de dos fuentes:

- **Las fichas técnicas** (título, colección, medidas, técnica, disponibilidad y categoría)
  se recuperaron de las 63 páginas `/project/<obra>/` del WordPress viejo archivadas en el
  Internet Archive.
- **Las imágenes** salieron del respaldo de `wp-content/uploads` que estaba en Google Drive.
  Se tomó siempre el archivo original, no las miniaturas que generaba WordPress.

Quedaron dos huecos conocidos, y **son huecos reales, no errores**: no hay que rellenarlos
inventando datos, solo Suyapa puede confirmarlos.

1. **Año**: el sitio viejo no publicaba el año de las 63 obras recuperadas, así que salen con
   `"ano": null`. Las 8 obras que ya estaban en el repo sí lo tienen.
2. **Categoría**: 15 obras quedaron en `"Otras obras"` — las 8 originales del repo y 7 piezas
   de 2025 que se publicaron después de que el sitio viejo dejara de usar categorías.
   `rotondas-2` además no tiene medidas.

## Trabajo pendiente

- Que Suyapa reasigne las 15 obras que están en "Otras obras" a su categoría real.
- Que complete el año de las obras que lo tengan a mano.
- El `admin.html` hoy permite **agregar** y **quitar** obras. Falta poder **editar** una
  obra existente (cambiarle categoría, año o disponibilidad sin borrarla y volver a subirla).

## La biografía

Está en `index.html`, dentro de `<section id="biografia">`, y tiene tres partes:
el texto en primera persona, la trayectoria por año, y las descripciones de sus
dos series. **Todo es material suyo**, recuperado del sitio archivado — no hay
una sola frase inventada.

Dos decisiones que conviene conocer antes de editarla:

- El texto base es la biografía de **2019**, que es más completa que la de 2021
  (incluye a sus maestros y la Mención Honorífica de Pebeo). Pero esa versión
  decía *"actualmente soy la presidente de AMAH"*, cosa que en 2021 ella misma
  ya corregía a **ex-presidenta por tres años**. Se usa la redacción de 2021 en
  ese punto para no publicar algo que hoy es falso.
- Las exposiciones de 2016 que en el texto de 2019 aparecían como
  *"mis últimas exposiciones"* se movieron a la trayectoria con su año. Doce
  años después ya no son las últimas.

En la trayectoria **solo llevan año los eventos que ella misma fechó** en sus
publicaciones. Lo que escribió sin año (Panamá, el segundo lugar en Guatemala,
FIFAC Estambul, la subasta de Infracnovi) está en "Otros reconocimientos", sin
fecha. Si aparece la fecha, se mueve; no se deduce.

La excepción es el homenaje de 2024: ella no puso el año, pero la XX edición de
"El Arte y la Paz" está documentada en prensa como 2024, y ahí sí se fechó.

## Nota de seguridad sobre la llave de GitHub

`admin.html` guarda un token de GitHub en el `localStorage` del teléfono de Suyapa. Es la única
opción en un sitio estático sin servidor, y **no es un almacenamiento seguro**: cualquiera con
acceso al teléfono, o cualquier XSS en el sitio, puede leerlo.

La mitigación es acotar el daño, no esconder el token. Por eso la app pide expresamente un
**fine-grained PAT** limitado a *este solo repositorio*, con permiso únicamente de
`Contents: Read and write` y con fecha de expiración. Si se filtra, lo peor que puede pasar es
que alguien edite la galería — no que toque el resto de la cuenta.

### La llave se pide UNA sola vez

La llave vive en el `localStorage` del teléfono, bajo la constante `CLAVE` de
`admin.html`. El `localStorage` pertenece al **origen** (`suyapamonterroso.com`),
no al archivo: podés reescribir `admin.html` entero, cambiarle el diseño o
agregarle campos, y Suyapa sigue conectada sin hacer nada.

Lo único que la desconecta:

- **Renombrar la constante `CLAVE`.** El teléfono buscaría en una gaveta vacía.
  Está marcado con un comentario en el código; respetalo.
- Que el token expire. Por eso conviene generarlo con expiración larga.
- Que borre la app de la pantalla de inicio y la vuelva a instalar: iOS puede
  descartar el almacenamiento de la app al desinstalarla.
- Que borre los datos de sitios web en el teléfono.

Nota sobre iOS: instalada en la pantalla de inicio, la app **no** está sujeta al
límite de 7 días que Safari le aplica al almacenamiento de sitios que no visitás.
Usada como pestaña normal de Safari, sí podría perder la llave por inactividad.
Otra razón para instalarla y usarla desde el ícono.

**No cambies esto por un token clásico ni le agregues más permisos.** Si algún día hace falta
algo más seguro, la vía correcta es un backend mínimo (una función serverless) que guarde el
token del lado del servidor.

## Reglas al modificar este repositorio

- Sin frameworks, sin paso de build, sin dependencias por npm. Todo tiene que funcionar
  abriendo el archivo directamente.
- Paleta: `#9c7b50` (acento), `#faf8f5` (fondo), `#2c2927` (texto), `#1f1d1b` (oscuro).
  Tipografías: **Cormorant Garamond** para títulos, **Montserrat** para el resto.
- Si agregás obras a mano, agregá también las dos imágenes (`img/obras/` y `img/obras/thumbs/`)
  o la galería va a mostrar un hueco.
- El sitio es de una artista y la obra es suya: **no inventes títulos, años, medidas ni
  categorías**. Si un dato no se sabe, dejalo en `null`.
