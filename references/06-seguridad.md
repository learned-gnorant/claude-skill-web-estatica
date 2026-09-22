# Seguridad, cabeceras y despliegue

## Cabeceras que debe servir cualquier sitio

```apache
Header always set Content-Security-Policy "default-src 'self'; script-src 'self' 'sha256-…'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self'; connect-src 'self' https://<servicio-formulario>; form-action 'self' https://<servicio-formulario>; frame-src https://www.youtube-nocookie.com; frame-ancestors 'self'; base-uri 'self'; object-src 'none'"
Header always set X-Content-Type-Options "nosniff"
Header always set X-Frame-Options "SAMEORIGIN"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set Permissions-Policy "geolocation=(), microphone=(), camera=(), interest-cohort=()"
# HSTS: sólo cuando el certificado esté confirmado, y SIN includeSubDomains
# si el certificado no es comodín. Una vez enviado, el navegador exige https un año.
Header always set Strict-Transport-Security "max-age=31536000"
```

- **La CSP con hashes, no con `unsafe-inline`.** Cada script en línea lleva su
  `sha256-…`. **Recalcula los hashes en cada compilación**: si se hacen a mano, un
  cambio de una coma deja la página sin JavaScript sólo en producción.
- **Comprueba que no falta ninguno**: recorre el HTML publicado, calcula el hash de cada
  `<script>` sin `src` y verifica que está en la CSP que SIRVE el servidor.
- **`frame-src` y `connect-src` salen de las constantes de la integración**, no de buscar
  cadenas en el HTML: una vez «acertaron» porque la cadena estaba en el texto legal, y
  con otra redacción habrían desaparecido.

## `.htaccess`: lo demás que hace falta

- **URLs limpias** (`/servicios` sirve `servicios.html`, y `/servicios.html` redirige con
  301 a la limpia). Cuidado con el bucle: la reescritura interna no debe disparar la
  redirección. Condición `%{ENV:REDIRECT_STATUS} ^$`.
- **Un solo nombre de dominio**: redirige todo lo que no sea el canónico, POR DESCARTE,
  antes de la regla de HTTPS (así se corrige protocolo y nombre en un solo salto y los
  dominios nuevos entran solos).
- **Ficheros ocultos bloqueados**: `<FilesMatch "^\.">` con `Require all denied`. Tapa
  `.DS_Store`, `.env` y `.git*` y no tapa `/.well-known/`, así que el certificado se
  sigue renovando.
- **Borra los `.DS_Store` de la carpeta que se sube.** Los crea el Finder entre la
  compilación y la subida; quien pide `/img/.DS_Store` obtiene el listado del directorio.
- **Antirrobo de enlaces** para imágenes y fuentes (`Referer` de otro dominio), con tres
  permisos: referer vacío, el propio dominio con y sin `www`, y Google/Bing para que las
  imágenes se indexen.
- **Compresión**: `AddOutputFilterByType` casa el tipo MIME EXACTO. Algunos alojamientos
  sirven los `.js` como `application/x-javascript` y la regla no los alcanza: declara el
  tipo y añade los nombres antiguos. En este proyecto eran 60 KB en vez de 18.

## Auditoría antes de publicar

Barrido del repositorio y de la carpeta a publicar:

- Claves, tokens y credenciales (`AKIA…`, `sk-…`, `ghp_…`, `AIza…`).
- `localStorage`, `sessionStorage`, `document.cookie`, `indexedDB`.
- Peticiones de salida: sólo las que se hayan decidido.
- `console.*`, `debugger`, `target="_blank"` sin `rel`, enlaces `http://`.
- `innerHTML`: comprueba que ninguno recibe datos del visitante. Lo que se pinta de un
  formulario va con `textContent`.
- Si hay JSON dentro de `<script type="application/json">`, que no contenga `</script`.

## Despliegue

- **Sube también los ficheros ocultos.** `.htaccess` es invisible en el Finder (Cmd+Mayús+.)
  y sin él se caen la CSP, las URLs limpias y las redirecciones. Ha pasado.
- **Verifica contra producción**, no contra local: cabeceras reales, páginas idénticas a
  las compiladas, redirecciones, 404 con código 404, certificado.
- **Si hay CDN delante, sáltatela para medir** (un parámetro aleatorio en la URL). Una
  regla del `.htaccess` puede parecer rota sólo porque la CDN sirve una copia cacheada.
- **Un 404 donde esperabas un 403 no siempre es un fallo**: si el fichero no existe, 404
  es la respuesta correcta y además revela menos.
