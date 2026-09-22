# Formularios e integraciones sin backend

## El servicio de formularios

Funciona bien (Formspree, Basin, Formcarry…), pero hay que saber esto:

- **El endpoint es público por diseño**: está en el `action`. Cualquiera puede mandarle un
  POST sin abrir la página. Por tanto:
  - Un límite de envíos escrito en JavaScript **no es una defensa**; sirve para evitar el
    doble clic, que es lo que de verdad pasa.
  - Los `maxlength` **no son una defensa**: evitan teclear de más, que ya es útil.
  - Las defensas reales están en el panel del servicio: dominios permitidos, filtro de
    spam, cuota.
- **El destino del correo se configura en el panel**, no en el marcado. Los campos tipo
  `_to` se retiraron hace años justo porque permitían usar formularios ajenos como relé.
- **Hay que abrir el correo de verificación** que el servicio manda al buzón de destino:
  sin eso el formulario no entrega nada y no avisa.
- **La restricción de dominio y el captcha son POR FORMULARIO**, no por cuenta. Al crear
  uno nuevo hay que rehacerlos.
- **Para «Responder» al visitante**, añade `_replyto` con el correo tecleado en el momento
  del envío (no como campo oculto con valor fijo).

## Dos trampas medidas

1. **El honeypot cazaba personas.** Un campo trampa escondido con `left:-9999px` sigue
   renderizado, y los gestores de contraseñas y el autorrelleno lo rellenan ignorando
   `autocomplete="off"`. El servicio responde **200** a un envío atrapado, así que la web
   decía «Consulta enviada» y la consulta se archivaba en spam: **consultas de clientes
   perdiéndose en silencio**. Si se usa honeypot, `display:none`, nunca fuera de lienzo. Y
   un 200 no significa «ha llegado»: hay que mirar la bandeja de destino.
2. **El captcha del servicio era incompatible con el envío por `fetch`** (Formspree, 09/2026). El captcha
   espera que el navegador NAVEGUE al POST para enseñar el desafío; con `fetch` no hay
   página que enseñar y el envío se rechaza sin explicación. O envío clásico, o sin
   captcha.

## Detalles de uso que el cliente agradece

- **Enviar por `fetch`** con `Accept: application/json` para no perder la página, con
  aviso en pantalla y un `mailto:` de reserva si falla.
- **La barra de progreso avanza al SALIR del campo** (`change`), no en cada tecla: con
  `input` se agita con cada letra y cuenta como completo un campo con una letra.
- **El área de texto empieza a la altura de su propio marcador** y crece con lo escrito;
  si el visitante arrastra el tirador, manda su medida.
- **`type="tel"` no valida nada**: sin `pattern` entra cualquier frase. Un patrón de 7 a
  15 cifras admite formatos internacionales. **Ojo**: en una clase de caracteres, los
  paréntesis y puntos sin escapar hacen que el navegador descarte el patrón entero en
  silencio. Verifícalo midiendo `checkValidity()`, no leyendo el atributo.
- **Un campo opcional no se convierte en obligatorio sin motivo**: exigir el teléfono
  cuesta consultas y va contra la minimización de datos.
- **Vigila el panel del servicio de vez en cuando**: si rechaza envíos (cuota, dominio,
  captcha), el visitante ve el aviso pero nadie de la casa se entera.
