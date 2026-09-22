# Canal de compilación para un sitio estático

## El patrón que funcionó

**Maestros intactos → script de compilación → `dist/` → servidor.**

- Los ficheros que entrega el diseñador o el exportador (`*.dc.html`, una maqueta, un
  tema) **no se editan nunca**. Son la referencia: si se tocan, se pierde la posibilidad
  de reexportar y de comparar.
- Toda corrección vive en el canal: un módulo por asunto (`legal.py`, `seo.py`,
  `movimiento.py`, `formularios.py`…), cada uno con un docstring que explica qué
  corrige y por qué.
- `dist/` es exactamente lo que se sube. Nada se edita a mano ahí dentro.

Ventaja real, no teórica: cuando el cliente cambia un dato (un teléfono, una cifra, un
correo), se cambia en un sitio y aparece en las 16 páginas, en los dos idiomas, en el
sitemap, en el JSON-LD y en el texto legal.

## Aserciones: que la compilación PARE

Es la técnica más rentable de todo el proyecto. Cada transformación comprueba que
encuentra lo que espera y, si no, **falla ruidosamente**:

- Si el texto que hay que sustituir aparece 0 o 2 veces, para.
- Si una clase generada por el compilador (`hv7`, `paso-3`) deja de coincidir, para.
- Si una página baja del umbral de palabras, para.
- Si el dato legal del pie y el del aviso legal divergen, para.

Sin esto, los cambios se aplican «sin efecto» y nadie se entera hasta que el cliente lo
ve publicado. Pasó tres veces antes de adoptar la regla.

## Otras piezas que hicieron falta

- **Servidor local que imita al de producción.** `python3 -m http.server` no reproduce
  URLs limpias, redirecciones, 404 real ni cabeceras. Un servidor propio de 150 líneas
  que replique las reglas del `.htaccess` evita la peor clase de defecto: el que sólo
  existe en producción.
- **Huella en los assets**: `site.js?v=<hash>`. Sin ella, el navegador sirve la versión
  en caché y se depuran durante horas fallos que ya estaban arreglados.
- **`dist/` en el control de versiones** si contiene material que no se regenera
  (imágenes derivadas, por ejemplo). Es además lo que permite volver a la versión
  publicada anterior.
- **La compilación escribe pero no limpia.** Si se retira una página, hay que borrar su
  HTML de `dist/` a mano o seguirá publicada.
- **Pasada final sobre `dist/`** para lo transversal (URLs, rutas, comprobaciones). Una
  ruta se escribe de seis formas distintas y vive en cinco sitios (`href`, `canonical`,
  `hreflang`, `og:url`, `sitemap`): repartir la corrección por módulos deja fuera el que
  se olvide. Barriendo la salida no hay dónde esconderse.
- **Ojo con lo que el barrido NO alcanza**: un JSON dentro de un `<script>`, el CSS que
  selecciona por `href*=`, las URLs escritas en JavaScript. Al cambiar rutas, búscalas
  también ahí.

## Atributos y marcado generado

- **HTML descarta los atributos repetidos**: si un generador emite dos `data-accion` en
  la misma etiqueta, sólo sobrevive el primero, **sin ningún aviso**, y los manejadores
  que cuelgan de los demás no se disparan nunca. Un atributo por evento
  (`data-ev-click`, `data-ev-keydown`).
- **Los nombres de atributo se reservan**: reutilizar uno que ya usa otro script produce
  efectos invisibles (elementos resaltados, estilos que aparecen sin motivo).
- **Las clases numeradas por el compilador** (`hv1`, `hv2`…) se renumeran si alguien
  añade un estilo antes. Cualquier código que dependa de una necesita su aserción.
