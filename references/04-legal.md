# Legal, cookies y privacidad (España / UE)

**No eres el asesor legal del cliente.** Redactas y maquetas lo que describe lo que el
sitio HACE, avisas de las contradicciones y dejas que un profesional valide lo dudoso.
Lo que sigue es el criterio técnico que evita publicar afirmaciones falsas.

## Las dos páginas mínimas

- **Aviso legal** (LSSI-CE art. 10): titular, NIF, domicilio, datos registrales, correo
  de contacto, actividad. Más condiciones de uso, propiedad intelectual, enlaces,
  responsabilidad, ley aplicable y jurisdicción.
- **Política de privacidad** (RGPD art. 13): responsable, qué datos se recogen y cuáles
  son opcionales, finalidad, base jurídica, plazos, encargados del tratamiento con
  nombre y país, transferencias internacionales y su garantía, derechos y cómo
  ejercerlos, reclamación ante la AEPD.

Y en el formulario, una casilla de consentimiento **sin marcar** que enlace a la política.

## La regla de oro

**Cada frase del documento es una afirmación técnica sobre el sitio.** Si cambia el
formulario, las tipografías, el vídeo o el alojamiento, el documento pasa a ser falso.

La única forma de que no se olvide es **atarlo a la compilación**: el párrafo se escribe
desde la misma constante que gobierna la integración, y si la integración desaparece, el
párrafo desaparece con ella. En este proyecto eso salvó tres veces el texto:

- Al autoalojar las tipografías, el punto que hablaba de Google Fonts pasó a ser falso.
- Mientras el teléfono fue obligatorio, el punto que lo listaba como opcional era falso.
- Los párrafos de Formspree y de YouTube estuvieron retenidos hasta que existieron de
  verdad.

**No borres apartados: sustitúyelos.** Quitar el punto 8 obliga a renumerar del 9 al 11,
que es donde se cuelan los errores en un texto legal.

## Encargados del tratamiento que suele haber, y que se olvidan

| Quién | Por qué está |
|---|---|
| El alojamiento | guarda la IP y los registros de acceso de CADA visita, desde el primer día |
| El servicio de formularios | recibe nombre, correo, teléfono y mensaje |
| YouTube / Vimeo | al reproducir, recibe la IP del visitante (EE. UU. en el caso de Google) |
| Tipografías, mapas, chats, analítica | cada uno, su párrafo y su transferencia |

Si alguno está fuera de la UE, hay que nombrar la garantía (normalmente Cláusulas
Contractuales Tipo) y declarar la transferencia.

## Cookies

- **Sin cookies ni analítica no hace falta banner.** Es la situación más limpia y la más
  rápida; consérvala mientras se pueda.
- **Las cookies analíticas necesitan consentimiento previo** (LSSI art. 22.2). Un banner
  de verdad tiene que permitir rechazar con la misma facilidad que aceptar y no cargar
  nada antes de la respuesta.
- **Contenido incrustado con fachada**: no cargues el reproductor hasta que se pulse. El
  aviso tiene que decir A QUIÉN se conecta ANTES del clic («Al pulsar se carga YouTube
  (Google, EE. UU.)»), también en el `aria-label`, para que el consentimiento sea
  informado. Mide que en reposo hay **0 peticiones externas, 0 cookies y 0 iframes**.
- **«nocookie» es un nombre comercial, no una garantía**: al reproducir se contactan ocho
  hosts de Google y varios se llaman «stats» y «ptracking». Medir el panel de red ENTERO,
  no las primeras peticiones.
- **Al cerrar el panel, retira el `<iframe>`**: pausar deja la conexión abierta.

## Analítica: cuándo compensa

Google Analytics obliga a banner de cookies, reescribe tres apartados de la política,
mete un script de terceros y, con banner, da datos incompletos. Para una web corporativa
con poco tráfico, casi nunca compensa. Alternativas por orden:

1. **Search Console y Bing Webmaster Tools**: gratis, sin cookies, y responden la
   pregunta que de verdad importa (qué se busca para llegar).
2. **El panel del formulario**: las consultas recibidas son la conversión real.
3. **Analítica sin cookies** (Plausible, Umami, Matomo autoalojado) si hacen falta visitas.
   Aun así, hay que retocar el apartado de analítica de la política.

## Coherencia de datos

Un mismo dato aparece en la ficha legal, el pie, el schema y a veces el cuerpo: **que
salga todo de una constante** y que la compilación pare si divergen. Y decide de una vez
qué correo va en cada sitio (contacto general, contacto identificativo del aviso legal,
ejercicio de derechos): mezclarlos es el error más común.
