# Imágenes, vídeo y piezas gráficas

## Imágenes

- **WebP, ancho máximo ~2000 px**, y varias medidas si hay visor ampliado (por ejemplo
  400 / 1200 / 2000). Suelen ser el 80 % de los bytes del sitio.
- **`loading="lazy"` y `decoding="async"`** en todo lo que no esté en la primera pantalla.
- **Si el JS cambia el `src` por código, un `<picture>` con `<source>` no sirve**: decide
  entre uno u otro desde el principio.
- **Decodifica ANTES de poner la imagen.** Cambiar `src` vacía el hueco de inmediato y la
  nueva imagen aún no está: se ven 2-3 fotogramas en blanco incluso en local. Descarga
  fuera del DOM, espera a `decode()` y sustituye entonces. Precarga la anterior y la
  siguiente en un carrusel.
- **Avisa si la espera se alarga**: baja el tono con `filter` a los 300 ms. Con `opacity`
  no funciona si hay una animación sujetando ese valor.
- **El rótulo entra con la imagen**, no antes: son un solo dato.
- **Los logotipos de cliente se igualan por ÁREA DE TINTA, no por altura.** Igualar
  alturas hace que un logo apaisado ocupe el doble; e igualar el área del archivo tampoco
  basta, porque muchos traen márgenes enormes con fondo opaco. Mide la caja de tinta y
  dimensiona a un área constante.
- **Al exportar piezas de marca desde el navegador, comprueba que la tipografía real ha
  cargado** (si no, exportas la de reserva y en una miniatura no se nota) y usa `canvas`
  si necesitas transparencia: las capturas de pantalla aplanan el alfa.

## Vídeo

- **Nunca un archivo propio.** YouTube o Vimeo, siempre incrustado.
- **Siempre con fachada**: una portada dibujada con el CSS del sitio y el `<iframe>`
  montado al pulsar, contra `youtube-nocookie.com`. Mide que en reposo no hay ninguna
  petición externa.
- **No uses la miniatura de YouTube**: se pide a sus servidores en cada visita, que es
  justo lo que la fachada evita.
- **Al cambiar de pieza o cerrar el panel, retira el `<iframe>`**, no lo pauses.
- El aviso de a quién se conecta va ANTES del clic (ver `04-legal.md`).

## Iconos y elementos gráficos

- **No dibujes iconos SVG a mano**: usa una familia (Phosphor, Tabler, Radix, Hugeicons) y
  una sola por proyecto, con el mismo grosor de trazo.
- Una pieza vectorial propia sí compensa cuando es identidad (una marca, un recurso del
  oficio del cliente): 2 KB que escalan y toman el color exacto.
- **Evita capturas de producto falsas hechas con `<div>`**: es el tell más reconocible.
