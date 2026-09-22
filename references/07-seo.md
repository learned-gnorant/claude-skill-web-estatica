# SEO e indexación

## Lo primero, y no es opcional: Search Console

Ninguna sonda externa detecta el problema más caro que tuvo este proyecto. El sitio
llevaba **meses sin poder ser rastreado** porque el dominio había estado aparcado y la
página de aparcado servía un `robots.txt` que lo prohibía todo; Google se lo creyó, lo
guardó y no volvió.

- **Si el dominio estuvo aparcado o en construcción, da por hecho que hay un `robots.txt`
  prohibitivo en la caché de Google** y fuerza el rastreo desde Search Console.
- **Antes de decirle a alguien «espera, Google ya pasará», compruébalo en Search
  Console.** La herramienta equivocada da una respuesta tranquilizadora y falsa.
- Verificación de la propiedad: si el cliente tiene Google Workspace en ese dominio, se
  autoverifica por DNS. Un dominio que sólo redirige **no se puede verificar por fichero
  ni por etiqueta**: la única vía es un registro TXT.

## La lista de comprobación

- **`robots.txt`** con `Allow: /` y la línea `Sitemap:`. Es público a propósito.
- **`sitemap.xml`** generado por la misma función que escribe los enlaces; si divergen,
  entregas a Google direcciones que el sitio no usa.
- **`canonical`** en cada página y **`hreflang` recíproco** si hay dos idiomas
  (incluyendo `x-default`).
- **Títulos y descripciones distintos en cada página y en cada idioma.** Dos portfolios
  con el mismo título es un fallo real que pasa desapercibido.
- **JSON-LD**: `Organization` + `WebSite` con `@id` compartido, `WebPage` y
  `BreadcrumbList` en las interiores. **No añade nada al índice**: describe lo que ya
  hay para que el resultado se muestre mejor. `LocalBusiness` sólo si hay local visitable.
- **`og:image`** de 1200×630 y `twitter:card: summary_large_image`. Sin ella, cada enlace
  compartido por WhatsApp o LinkedIn sale con un hueco.
- **La 404 debe devolver 404** y no heredar el `canonical`, los `hreflang` ni el JSON-LD
  de la portada.
- **`alt` de verdad** en las imágenes con contenido.

## Posicionar por contenido

- La etiqueta `keywords` no la lee nadie desde 2009. Lo que pesa es dónde están las
  palabras: `<title>`, `<h1>`, `<h2>`, primer párrafo y texto de los enlaces.
- **Mide antes de escribir**: cuenta cuántas veces aparecen en el sitio los términos por
  los que el cliente quiere que le encuentren y si alguno está en un encabezado. En este
  proyecto, «maquetación» salía 5 veces y en ningún encabezado.
- **Los titulares aprobados no se tocan.** Busca dónde caben esas palabras de forma
  natural: nombres de etapas que eran `<span>` y debían ser `<h3>`, una página nueva que
  el negocio necesitaba de todas formas.
- **Una página por intención**, no diez intenciones en una: dos páginas propias compiten
  entre sí por la misma consulta.
- **Umbral de contenido**: por debajo de ~400 palabras una página cuenta como escasa y
  arrastra al dominio. Veintiséis fichas de 40 palabras es el patrón de páginas-puerta
  que Google penaliza: si no hay datos de verdad por proyecto, no hagas la página.
- **Sede y NAP**: si el cliente quiere aparecer «en Madrid», el sitio tiene que decir en
  algún sitio visible que está en Madrid, con la misma dirección que en el resto de
  internet. Y el bloque de mapas exige perfil de Google Business, que es decisión del
  cliente (y requiere local o zona de servicio).

## Tras cada despliegue

1. **IndexNow** con las URLs que hayan cambiado (09/2026): llega a Bing, Yandex, Seznam y Naver, y
   de Bing tiran DuckDuckGo, Ecosia y los buscadores de ChatGPT y Copilot. Publica el
   fichero de clave en la raíz y comprueba que responde antes de avisar (si no, 403).
2. **Search Console**: pedir indexación de lo que ha cambiado de verdad. El límite no es
   un cupo diario duro; si da error, reintenta al rato.
3. **Bing Webmaster Tools**: se da de alta una vez, y se puede importar desde Search
   Console con permiso de sólo lectura.
4. A las 1-2 semanas, mirar consultas y páginas indexadas en ambos.

## Sobre los escáneres automáticos de SEO o «IA-readiness»

Mezclan hallazgos reales, falsos positivos por suponer el idioma o la estructura, y la
agenda de quien los escribe. **Mide cada afirmación contra el sitio** antes de tocar
nada: en este proyecto, de nueve hallazgos, tres eran reales, dos falsos y el resto,
convenciones propuestas que ningún buscador usa.
