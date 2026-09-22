# Método: cómo verificar sin engañarse

Este apartado es el que más veces salvó el proyecto. Cada punto viene de una medición
que dio verde con la cosa rota, o roja con la cosa bien.

## Dónde medir

- **En una pestaña VISIBLE.** En una pestaña en segundo plano (`visibilityState:
  "hidden"`) el `IntersectionObserver` no dispara y parece que nada aparece.
- **En iframes sólo la geometría.** Un iframe fuera de pantalla no interseca nunca, así
  que cualquier lógica basada en visibilidad da un falso resultado. Y espera a
  `document.fonts.ready`: con la tipografía de reserva los textos salen más altos y la
  sonda detecta cruces que no existen.
- **Contra producción lo que sólo existe en producción**: cabeceras, CSP, redirecciones,
  compresión, certificados. Y saltándote la CDN.
- **En el navegador, no en el código.** «La regla casa» no es verificación: lee el valor
  computado. Varias reglas correctas sobre el papel no pintaban nada por un `style=` en
  línea.

## Qué medir

- **El resultado, no la declaración.** Comprobar que una animación está declarada
  (`animationName !== "none"`) da verde con la animación rota: mide la opacidad a lo
  largo del scroll.
- **Un 200 no significa «ha llegado»** cuando hay un tercero que acepta y descarta.
- **Los números que se comparan tienen que poder diferir.** Una condición que compara dos
  magnitudes que la geometría hace iguales no es una decisión: es un sorteo que decide el
  redondeo, y se manifiesta como un fallo aleatorio imposible de reproducir.
- **Un clic simulado no es un clic.** `elemento.click()` no pasa por `pointerdown` /
  `mousedown` / `mouseup`, así que no reproduce ningún defecto que dependa de dónde cae
  cada uno. Si el cliente dice que algo no responde y en tu prueba responde, **sospecha
  de la prueba antes que del cliente**.
- **Una medida en un solo ancho no vale.** Casi todos los defectos de geometría de este
  proyecto cambiaban con la anchura: mide en 390, 760, 1001, 1280, 1440 y 1920, en los
  dos idiomas.
- **Cuidado al contar con `grep` sobre HTML generado**: los atributos salen en otro orden
  del que esperas y las cadenas aparecen también en comentarios y en el CSS. Comprueba
  qué has contado.

## Cómo contarlo

- Di **qué mediste, con qué número y en qué condiciones**. «Tirón máximo de 3,3× a 1,6×,
  medido en ventanas de 40 px de 1100×700 a 2560×1440» vale; «ahora va más suave» no.
- **Si algo no se ha podido verificar, dilo.** Ejemplos reales: la emulación táctil no
  reproduce la heurística de iOS; la barra del navegador móvil no existe en el emulador.
- **Corrige en cuanto lo sepas.** Una afirmación falsa en la documentación cuesta días:
  aquí una nota describió durante una semana un defecto creyendo que describía la
  solución.

## Cuando el cliente repite una queja

Si señala lo mismo dos veces, **la causa no es la que creías**. Deja de ajustar el
parámetro que ya ajustaste y busca otra explicación: en este proyecto, una «imperfección»
que se atribuyó tres veces a la geometría era la fase del punteado, y un quiebro que se
corrigió dos veces sólo estaba corregido en la pantalla donde se midió.
