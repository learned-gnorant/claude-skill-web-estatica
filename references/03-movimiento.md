# Movimiento

## Reglas de partida

- **Ningún listener de `scroll`.** Provocan jank. Se usa `IntersectionObserver`,
  `ViewTimeline`/`ScrollTimeline` (WAAPI) o animaciones CSS.
- **Sólo `transform`, `opacity`, `filter` y `clip-path`.** Nada que dispare maquetación.
- **`prefers-reduced-motion` va con la animación**, no después. Significa «menos y más
  suave», no «nada»: se quitan desplazamientos, se conservan las transiciones que ayudan
  a entender.
- **Nada del contenido puede depender de una animación para verse.** Si el JS falla o el
  navegador no soporta el mecanismo, el sitio se ve entero.
- **Lo que se ve al cargar no se anima.** El héroe está ya en pantalla: animarlo sólo
  retrasa la primera lectura.

## Apariciones al hacer scroll

Lo que funcionó: un `IntersectionObserver` en línea en todas las páginas, y **tres
gramáticas** para que dieciocho elementos no parezcan un bucle:

| Gramática | Qué hace | Para qué |
|---|---|---|
| revelar | sube 22 px y aparece, 720 ms | textos y bloques |
| barrer | `clip-path` de izquierda a derecha, 950 ms | procesos y series |
| enfocar | entra con `blur(9px)` y afina, 800 ms | imágenes |

Detalles que costaron una pasada cada uno:

- **La unidad que aparece es el BLOQUE, no la línea**: agrupa los textos por su
  `<section>` y que el primero que cruza arrastre a los demás en el mismo fotograma. Un
  titular que llega antes que su párrafo son dos sucesos, no un bloque.
- **Una sección demasiado grande revela cosas que están dos pantallas más abajo.** Si una
  página es un solo `<section>`, se revela todo de golpe: divide por apartados.
- **Ocultar desde el JS, nunca desde el HTML.** Si el script falla, se ve todo.
- **La clase que oculta necesita `!important`** cuando la prosa trae su opacidad en
  `style=`: si no, el texto se ve, y al cruzar el umbral la animación lo tira a 0 y lo
  sube otra vez — un parpadeo del bloque entero.
- **Ningún fotograma final debe declarar `opacity: 1`**: se lleva por delante las
  opacidades de diseño y produce un segundo parpadeo al terminar.
- **Sáltate lo no renderizado** (`[hidden]`, sin `client rects`, dentro de un acordeón
  cerrado): nunca interseca, así que quedaría oculto para siempre.
- **`animation-timeline: view()` en CSS falla a medias**, que es peor que no tenerlo:
  mide contra el scrollport MÁS CERCANO, así que cualquier ancestro con `overflow:hidden`
  congela el progreso; y la duración en segundos no significa nada en un reloj de scroll.

## Animación ligada al scroll (WAAPI)

Si hay que dibujar algo a medida que se baja (un hilo, una línea, un recorrido):

- `element.animate(keyframes, { timeline: new ViewTimeline({subject}), rangeStart, rangeEnd, fill: "both" })`.
- **El sujeto tiene que ser un elemento normal FUERA de cualquier contenedor con
  `overflow:hidden`**, o el reloj no avanza nunca.
- **Reparte el avance con criterio**: si el progreso depende sólo de la altura del punto,
  los tramos horizontales se recorren de golpe y se lee como un tirón. Dale a cada píxel
  un coste mínimo y suaviza con una media móvil; mide la velocidad máxima frente a la
  media antes y después.
- **Si dos cosas comparten el reloj** (el trazo y lo que aparece a su paso), que lean la
  MISMA tabla: dos fórmulas paralelas se desincronizan al primer ajuste.
- **En móvil, `innerHeight` cambia al aparecer y desaparecer la barra del navegador** y
  eso rehace la animación con otro alto: fija el alto mientras no cambie el ancho.
- **Animaciones que se suman** (`composite: "add"`) para que una entrada no pelee con el
  progreso del scroll.

## Geometría dibujada (SVG)

- **Mide con `offsetLeft/offsetTop`, no con `getBoundingClientRect`**, si los elementos
  rotan: la caja envolvente de algo girado no es el elemento.
- **Los quiebros se ven.** Un desnivel de 5 px resuelto en 40 px de recorrido se lee como
  un fallo; el mismo desnivel repartido en 500 px no se nota. Y el desnivel cambia con el
  ancho de pantalla: **mídelo en varios**, no en uno.
- **Continuidad de tangente Y de curvatura**: dos arcos que empalman con la misma
  dirección pero distinta curvatura todavía se ven como un quiebro.
- Un trazo discontinuo que se cruza consigo mismo necesita que el cruce caiga en el hueco
  del punteado, no en la punta de un guion; si no, se lee como una mancha.
