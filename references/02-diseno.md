# Diseño con identidad, y filtro anti-AI-slop

## El principio

Una web de empresa tiene que parecerse a esa empresa y a ninguna otra. La identidad se
construye con **una metáfora del oficio del cliente**, no con adornos: en una casa de
artes gráficas, las marcas de corte, la banda CMYK del canto del pliego, los filetes y
la trama de impresión. En una bodega, una constructora o un despacho la metáfora será
otra, pero la regla es la misma: **el recurso gráfico tiene que significar algo**.

Cuando el cliente ya tiene una maqueta aprobada, **esa maqueta manda**: no se sustituye
por «algo más limpio». Lo que se hace es corregirla donde falla de verdad.

## Lo que delata a una web hecha por una IA

Evítalo salvo que lo pidan expresamente:

- Degradado violeta/azul de fondo, resplandores de neón, héroe centrado sobre malla
  oscura, tres tarjetas idénticas en fila, cristal esmerilado en todo.
- Tipografía Inter por defecto, titulares enormes que sólo gritan, serif «porque es
  creativo». Si se usa serif, que haya un motivo que se pueda decir en una frase.
- Rótulos pequeños en versalitas sobre CADA sección (`01 · CAPACIDADES`), separadores
  de punto medio en todas las líneas, textos de relleno con métricas inventadas,
  nombres de marca tipo «Acme» o «Nexus», verbos de folleto («impulsa», «revoluciona»).
- Capturas de producto falsas hechas con `<div>`, iconos SVG dibujados a mano, páginas
  sin una sola imagen real.
- Animaciones en bucle infinito por todas partes, marquesinas repetidas, avisos de
  «scroll» al pie del héroe.
- Rayas decorativas y cruces sólo para que «parezca diseñado».

**Regla práctica**: si un elemento no comunica jerarquía, estado o contenido, sobra.

## Tipografía

- **Autoalójala.** Servirla desde Google Fonts comunica a Google la IP de cada visitante:
  es una transferencia internacional que hay que declarar y que nadie pidió. Descarga
  sólo los subconjuntos que se usan (latin y latin-ext suelen bastar) y deduplica: las
  variables sirven el mismo fichero para todos los pesos.
- **Precarga las caras de la primera pantalla** con `<link rel="preload">` y `crossorigin`
  (obligatorio aunque el fichero sea del propio dominio; sin él se descarga dos veces).
  Una fuente no se pide hasta que el navegador maqueta el texto que la usa, así que sin
  precarga siempre llega tarde.
- **`font-display: fallback`**, no `optional` ni `swap`. `optional` NO intercambia si la
  fuente llega tarde: en cualquier primera visita real el sitio se compone con la letra
  de reserva. Se descubrió en producción, no en local, porque en local la red no existe.
- **Ajusta la métrica de la reserva** (`size-adjust`) para que el cambio de fuente no
  reajuste los titulares. Mídelo: en este proyecto, Times era un 12 % más estrecha que la
  Bodoni y Georgia ajustada al 101,6 % dejó el salto en 0,8 %.
- **Cuidado con las sondas de tipografía**: medir una fuente que el navegador ha decidido
  no aplicar devuelve el ancho de la de reserva y las conclusiones salen invertidas.
  Comprueba siempre que la medida NO coincide con la del serif por defecto.

## Cuerpo de texto y retícula

- **El cuerpo puede crecer en pantallas anchas, pero empezando tarde**: un portátil de
  1512 px no es «pantalla ancha». En este proyecto el texto es plano hasta 1600 px y sube
  de ahí a 1920.
- **`clamp()` necesita espacios alrededor de los operadores.** `clamp(1rem,.6vw+.96rem,1.2rem)`
  es inválido y el navegador **descarta la declaración entera sin avisar**: 16 declaraciones
  del maestro servían 16 px en cualquier pantalla.
- **El contenedor debe crecer con la ventana** si las bandas van a sangre; si no, en un
  monitor grande el color ocupa todo y el texto se queda en una columna estrecha en medio.
- **Hover sólo con puntero fino**: `@media (hover: hover) and (pointer: fine)`. En táctil,
  una regla de hover hace que Safari de iOS se coma el primer toque, y el cliente lo
  describe como «tengo que pulsar dos veces». Audita TODAS las fuentes de hover, también
  las que genere el compilador.
- **Zonas táctiles de 44 px** como mínimo en los enlaces de navegación.
- **Contraste**: mide, pero no rompas la jerarquía por cumplir un número. Si el arreglo
  evidente iguala dos niveles distintos, hay que decidirlo con el cliente y dejar por
  escrito que se sabe.

## Cómo trabajar el diseño con skills

- `/design-taste-frontend` para el filtro anti-slop y la dirección; su lista de «tells»
  es la mejor que hay a mano.
- `/design` para identidad, tokens, logotipo o piezas gráficas.
- `/impeccable` o `/emil-design-eng` para pulir una interfaz existente.
- No mezcles sistemas de diseño: uno por proyecto.
