# Handoff: Generador de placas de búsqueda laboral (LinkedIn) — H&A

**Sitio en vivo:** https://willyesposito.github.io/busquedashya/ — herramienta funcional (`generador-de-placas.html`) reimplementada en HTML/CSS/JS estático a partir de esta especificación, más el canvas de referencia (`publicaciones-linkedin.html`).

## Overview
Herramienta interna para que Hidalgo & Asociados genere placas de búsquedas laborales para LinkedIn (1200×1200 px), respetando el sistema de marca de la consultora. El usuario carga el contenido de la búsqueda (a mano o pegando el texto de una publicación existente), elige uno de 3 layouts, y descarga un PNG listo para publicar.

Incluye también un canvas de exploración (`Publicaciones LinkedIn.dc.html`) con las 3 variantes de layout aplicadas a un caso real, usado para decidir la dirección visual antes de construir el generador.

## About the Design Files
Los archivos de este paquete son **referencias de diseño hechas en HTML** (Design Components de nuestro entorno de prototipado) — prototipos funcionales que muestran el look final y el comportamiento esperado, no código de producción para copiar tal cual. La tarea es **recrear este diseño en el entorno/código de destino** (el stack que uses: React, Vue, vanilla, etc.), respetando exactamente los valores documentados abajo.

## Fidelity
**Alta fidelidad (hifi).** Colores, tipografía, espaciados e interacciones están definidos y deben respetarse pixel a pixel. El HTML adjunto es funcional (podés abrirlo directo en el navegador) y sirve como referencia visual exacta.

## Pantallas / Vistas

### 1. Generador de Placas (`Generador de Placas.dc.html`)
**Propósito:** cargar los datos de una búsqueda laboral y exportar la placa como PNG.

**Layout general:** dos columnas fijas, `display:flex; height:100vh`.
- **Panel izquierdo** (`width:410px`, fondo blanco `#FFFFFF`, borde derecho `1px solid #DDE5EF`, `overflow-y:auto`, padding `28px 26px 40px 26px`, `flex-direction:column`, `gap:18px`): formulario de carga.
- **Panel derecho** (`flex:1`): barra superior con selector de layout + botón descarga, y debajo el área de preview con scroll, centrada.

**Panel izquierdo — de arriba a abajo:**
1. Header: logo (44×44) + título "Generador de placas" (17px/700, `#1E3A5F`) + subtítulo "Búsquedas laborales · LinkedIn" (12px, `#8C837B`).
2. Aviso de privacidad obligatorio: fondo `#FFF8E1`, borde izquierdo `4px solid #00ACD4`, padding `10px 14px`, texto 12px `#5D4037`, con "Aviso de privacidad:" en negrita `#00ACD4`.
3. Dos botones lado a lado (`gap:8px`): "Importar desde texto" (pill, borde `2px solid #00ACD4`, texto `#00ACD4`) y "Cargar ejemplo" (pill, borde `1px solid #DDE5EF`, texto `#8C837B`).
4. Panel de importación (colapsable): textarea de 9 filas + botón "Completar campos" (pill celeste sólido). Al togglear "Importar desde texto" aparece con fondo `#F4F7FA`, borde `1px solid #DDE5EF`, radio 14px, padding 14px.
5. Separador `1px` `#DDE5EF`.
6. Campos de formulario (inputs/textareas con el estilo estándar del design system: `padding:9px 12px; border:1px solid #DDE5EF; border-radius:8px; font-size:13px; color:#1E3A5F; font-family:'Source Sans Pro'`):
   - Cápsula superior (input)
   - Título del puesto (input)
   - Subtítulo — solo aplica al layout B (input)
   - Fila de 3 columnas: Ubicación / Horario / Esquema (inputs)
   - Título de la primera sección (input, default "Principales responsabilidades")
   - Responsabilidades (textarea 8 filas, una por línea; formato `Etiqueta: detalle` pone la etiqueta en negrita)
   - Título de la segunda sección (input, default "Requisitos")
   - Requisitos (textarea 5 filas, mismo formato)
   - Email de postulación (input)

**Panel derecho — barra superior** (`padding:18px 28px`, borde inferior `1px solid #DDE5EF`, fondo blanco):
- Label "LAYOUT" (12px/700 uppercase, `#00ACD4`) + 3 tabs tipo pill: "A · Clásica", "B · Panel lateral", "C · Hero celeste". Tab activo: fondo `#00ACD4`, texto blanco, borde `2px solid #00ACD4`. Inactivo: fondo transparente, texto `#8C837B`, borde `2px solid #DDE5EF`.
- Botón "Descargar PNG" a la derecha: pill celeste sólido, texto blanco 14px/600.

**Área de preview:** la placa se renderiza siempre a 1200×1200px reales y se escala visualmente con `transform:scale()` para caber en el viewport disponible (recalculado en resize). El nodo que se exporta a PNG (via `html-to-image`, librería `html-to-image@1.11.11`) es el div de 1200×1200 sin el `transform` aplicado en el momento de la captura.

### 1a/1b/1c — Layout A: "Clásica blanca"
Placa 1200×1200, fondo blanco, `flex-direction:column`.
- Header (padding `56px 72px 0 72px`, `justify-content:space-between`): cápsula celeste pill (texto configurable, ej. "Buscamos para nuestro cliente" — 19px/700 uppercase, `letter-spacing:0.05em`, padding `7px 26px`) + título (66/56/48/40px según longitud, 700, `color:#00ACD4`, `line-height:1.05`) a la izquierda; logo oficial 110×110px a la derecha (imagen original, sin recortar, sobre fondo blanco).
- Fila de "metas" (ubicación/horario/esquema) como pills con borde `2px solid #00ACD4`, texto `#00ACD4`, 22px/600, padding `8px 24px`, `gap:14px`.
- Cuerpo (padding `36px 72px 0`, `gap` proporcional a un tamaño de fuente que se autoajusta — ver "Auto-fit" abajo):
  - Título de sección responsabilidades (0.84em, 700, uppercase, `letter-spacing:0.09em`, `#00ACD4`)
  - Lista de responsabilidades: bullet circular celeste (0.46em) + texto con la etiqueta en negrita
  - Separador horizontal `2px` `#E7E6E6`
  - Fila de dos columnas: título "Requisitos" a la izquierda (ancho fijo ~220px) + lista de requisitos a la derecha
- Footer: banda celeste sólida (padding `30px 72px`), texto blanco "Postulate enviando tu CV a" (25px/600) + pill blanca con el mail en celeste (28px/700).

### Layout B: "Panel lateral celeste"
Placa 1200×1200, `display:flex` en fila.
- Columna izquierda (`flex:1`, padding `56px 48px 52px 64px`): cápsula + título (58/47/40/34px según longitud) + subtítulo opcional (23px/300, `#8C837B`) + cuerpo con responsabilidades y requisitos apilados verticalmente (mismo patrón de bullets que layout A, tamaños ligeramente menores).
- Columna derecha fija (`width:330px`, fondo `#00ACD4`, textura de fondo `bg-olas-apiladas.png` al 18% de opacidad superpuesta): logo circular 92×92 con borde blanco `4px` (usa la versión recortada del logo, ver Assets) + lista de metas (label 16px/700 uppercase 80% opacidad + valor 25px/600) + al final, pegado abajo (`margin-top:auto`), "Postulate a" (18px/700 uppercase) y pill blanca con el mail en celeste.

### Layout C: "Hero celeste"
Placa 1200×1200, `flex-direction:column`.
- Hero superior celeste (`background:#00ACD4`, padding `48px 64px 42px 64px`, textura `bg-olas-apiladas.png` 22% opacidad, posición `center bottom`): cápsula blanca con texto celeste + título blanco (62/50/43/36px aprox.) + fila de pills de metas con borde blanco translúcido; a la derecha, círculo blanco 100×100 con el logo recortado adentro (90×90).
- Cuerpo en grid de 2 columnas (`1.45fr 1fr`, `gap` proporcional): responsabilidades a la izquierda (mismo patrón de bullets); requisitos a la derecha como tarjetas individuales (borde `1px solid #E7E6E6`, radio 14px, padding `0.7em 0.9em`).
- Footer con borde superior `1px solid #E7E6E6`: logo (58×58, imagen original) + "Hidalgo & Asociados" (26px/300, `#8C837B`) a la izquierda; "Postulate a" (22px/600) + pill celeste sólida con el mail (25px/700) a la derecha.

## Auto-fit de texto largo
El cuerpo de cada layout se envuelve en un contenedor con `overflow:hidden` y un `font-size` base (`fitPx`, arranca en 24px) del que casi todos los tamaños internos derivan en unidades `em`. Después de cada render, si el contenido se pasa de alto (`scrollHeight > clientHeight`), se reduce `fitPx` en 1px y se vuelve a medir, hasta que entra o se llega a un piso de 13px. Esto evita que responsabilidades/requisitos largos rompan el layout de la placa.

El tamaño del **título** y del **email** se resuelve aparte, por rangos de longitud de caracteres (ver breakpoints en cada layout arriba), no por el mecanismo de auto-fit del cuerpo.

## Importar desde texto
Un textarea de "pegado libre" + función de parseo heurística (no IA, reglas por regex) que:
- Detecta el email por regex `[\w.+-]+@[\w.-]+\.\w+`.
- Limpia bullets/emojis comunes al inicio de línea (`•, -, –, —, ▪, ✅, ☑️, 🔹, 🚀, 📍, *, ·`).
- Toma la primera línea que no sea de contacto como título (removiendo prefijos "Búsqueda laboral:" / "Búsqueda:").
- Busca líneas de ubicación/horario/esquema por keywords.
- Detecta encabezados de sección por keywords (`respons|tareas|cuáles serán` → responsabilidades; `requisit|valoramos|perfil buscado` → requisitos; `datos de interés` como límite) y agrupa las líneas siguientes hasta el próximo encabezado.
- Al confirmar, sobrescribe solo los campos que pudo inferir; el resto queda para edición manual.

## Interacciones y comportamiento
- Cambiar de layout (A/B/C) resetea el auto-fit (`fitPx` vuelve a 24) para recalcular contra el nuevo layout.
- Cualquier cambio de campo también resetea `fitPx` a 24 antes de volver a medir.
- "Cargar ejemplo" restaura todos los campos a los valores por defecto (el caso "Analista de RRHH").
- El estado del formulario (todos los campos + layout elegido) persiste en `localStorage` bajo la key `hya-generador-placas`, así el usuario no pierde el trabajo al recargar.
- "Descargar PNG": renderiza el nodo de 1200×1200 (sin el `transform:scale` de preview) a PNG vía `html-to-image` y dispara la descarga con nombre `placa-<slug-del-titulo>.png`.
- El preview se reescala automáticamente en `resize` de ventana para siempre caber en el espacio disponible, sin recortar.

## Design Tokens
Todos tomados de `colors_and_type.css` del design system H&A:
- **Celeste primario:** `#00ACD4` (título, cápsulas, bullets, botones, panel lateral, hero)
- **Celeste hover/dark:** `#0090B4`
- **Gris cálido (wordmark/subtítulos):** `#8C837B`
- **Negro (cuerpo de texto):** `#000000`
- **Blanco:** `#FFFFFF`
- **Bordes:** `#E7E6E6` (marca/slides), `#DDE5EF` (UI de la herramienta)
- **Fondo UI herramienta:** `#F4F7FA`, texto UI `#1E3A5F` / `#8C837B`
- **Aviso de privacidad:** fondo `#FFF8E1`, texto `#5D4037`, acento `#00ACD4`
- **Tipografía:** `'Source Sans Pro', Arial, Helvetica, sans-serif` en toda la pieza (marca), pesos 300/400/600/700
- **Radios:** `9999px` en todos los botones/pills/badges (nunca cuadrados ni levemente redondeados); `14px` en tarjetas/paneles de importación
- **Sombra de la placa:** `0 12px 40px rgba(30,58,95,0.15)`
- **Easing/transición de tabs:** `all 0.22s cubic-bezier(.4,0,.2,1)`

## Assets
- `assets/logo-consultora.png` — isotipo oficial H&A (252×252, **fondo blanco, NO transparente**). Usar tal cual cuando se apoya directo sobre fondo blanco (header layout A, footers).
- `assets/logo-consultora-tight.png` — **versión recortada** del mismo isotipo: el archivo original trae ~5% de margen blanco alrededor del círculo celeste; al insertarlo dentro de otro círculo blanco (badge del panel lateral en layout B, badge del hero en layout C) ese margen se sumaba al borde blanco agregado por CSS y se veía como un anillo blanco grueso feo. Esta versión recorta ese margen y llena el cuadro completo, para usar únicamente dentro de badges circulares con fondo/borde propios.
  - Si se regenera el logo, replicar el recorte: el círculo útil ocupa aprox. el 86.5% del lienzo cuadrado (centrado), o recrear la versión negativa/tight directamente desde el archivo fuente del logo si el design system la provee.
- `assets/bg-olas-apiladas.png` — textura de fondo "olas apiladas" del design system, usada al 18–22% de opacidad sobre las superficies celeste (panel lateral B, hero C).
- Librería externa: `html-to-image@1.11.11` (CDN `unpkg`), usada solo para la exportación a PNG.

## Archivos
- `Generador de Placas.dc.html` — la herramienta completa (formulario + 3 layouts + export).
- `Publicaciones LinkedIn.dc.html` — canvas de exploración con las 3 variantes de layout aplicadas a contenido real, usado para decidir la dirección visual (útil como referencia de contexto, no es la pieza final).
- `assets/` — logos y fondos usados por ambos archivos (ver arriba).
- `_ds/` (no incluido en este paquete — extraer del proyecto original si hace falta) — bundle completo del design system H&A: `colors_and_type.css`, `_ds_bundle.js`.

## Notas para la reimplementación
- Los archivos `.dc.html` son prototipos de nuestro entorno de diseño (streaming templates + lógica de React embebida); **no se referencian ni se copian directo a producción** — son la especificación visual y de comportamiento a recrear en el stack real del equipo.
- Si el codebase de destino no tiene un framework definido para este tipo de herramienta interna, React simple + una librería de captura de nodo a imagen (la que ya usa el equipo, o `html-to-image`/`dom-to-image`) es la ruta más directa dado que el diseño ya está pensado como "formulario controla un nodo DOM que se exporta".
