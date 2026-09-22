---
name: web-estatica
description: "Construir y mantener webs corporativas estáticas (HTML/CSS/JS sin backend) con identidad propia: canal de compilación, diseño anti-AI-slop, movimiento, textos legales y RGPD, formularios sin backend, seguridad y CSP, SEO e indexación, imágenes y vídeo, y método de verificación. Úsala al arrancar una web nueva, al retomar una existente, al desplegar, o cuando haya que decidir sobre cookies, analítica, legal, rendimiento o posicionamiento."
---

# Webs estáticas con identidad propia

Destilado de un proyecto real llevado de principio a fin: una web corporativa bilingüe
de 16 páginas, sin backend, publicada en alojamiento compartido. Todo lo que hay aquí
costó medirlo o equivocarse primero.

**Todo se midió entre agosto y septiembre de 2026.** Lo que depende de un tercero
—navegadores, servicios de formularios, paneles de buscadores— envejece: las
afirmaciones más perecederas llevan su fecha, y conviene comprobarlas antes de
apoyarse en ellas.

**Léelo como criterio, no como receta.** Cada apartado dice qué decisión se tomó, qué se
midió y qué falló, para que no haya que repetir el error.

## Lo primero, en cada proyecto nuevo

1. **Escribe el contrato antes que el código.** `references/00-arranque.md` trae la
   plantilla de `CLAUDE.md` del proyecto: negocio, stack cerrado, decisiones que no se
   reabren. Sin eso, cada sesión reinventa lo que ya estaba decidido.
2. **Pregunta lo que no puedes deducir** y no lo supongas: dominio, alojamiento, idiomas,
   quién recibe el formulario, si hay local visitable, si habrá tienda, qué datos
   fiscales van en el aviso legal. Una inferencia sobre el negocio del cliente hay que
   marcarla como tal Y preguntarla.
3. **Decide de una vez** (y anótalo): con o sin backend, con o sin cookies, un idioma o
   dos, un dominio o varios. Casi todos los problemas caros de este proyecto vinieron de
   revisar una de estas cuatro a mitad de camino.

## Los apartados

| Tema | Fichero |
|---|---|
| Arranque, contrato y plantilla de CLAUDE.md | `references/00-arranque.md` |
| Canal de compilación: maestros intactos, `dist/`, aserciones | `references/01-compilacion.md` |
| Diseño con identidad y filtro anti-AI-slop | `references/02-diseno.md` |
| Movimiento: apariciones, scroll, `prefers-reduced-motion` | `references/03-movimiento.md` |
| Legal, cookies y privacidad (LSSI-CE y RGPD) | `references/04-legal.md` |
| Formularios e integraciones sin backend | `references/05-formularios.md` |
| Seguridad, cabeceras, CSP y despliegue | `references/06-seguridad.md` |
| SEO, indexación y buscadores | `references/07-seo.md` |
| Imágenes, vídeo y tipografía | `references/08-medios.md` |
| Método: cómo verificar sin engañarse | `references/09-metodo.md` |
| Qué skill usar para cada cosa | `references/10-skills.md` |
| Control de versiones y commits | `references/11-git.md` |

## Ocho reglas que valen en cualquier web de este tipo

1. **Medir, no suponer.** «La regla es correcta sobre el papel» ha sido falso tantas
   veces que ya no cuenta como argumento. Lee el valor computado en el navegador, con la
   tipografía cargada, en una pestaña visible y, si afecta a producción, contra
   producción.
2. **Un `style=` en línea gana a cualquier hoja.** Si el marcado viene de un exportador
   o de una maqueta, la mitad de las reglas necesitarán `!important` y hay que
   comprobarlo leyendo el valor computado, no la captura.
3. **Lo que no se ve en local es lo más caro.** Cabeceras, CSP, redirecciones, `.htaccess`
   y CDN sólo existen en producción: reprodúcelos en el servidor local o acabarás
   descubriendo el fallo con el sitio publicado.
4. **Cada afirmación del texto legal es una afirmación técnica.** Si el formulario, las
   tipografías o los vídeos cambian, el documento legal miente hasta que alguien lo
   cambie. Átalo a la compilación.
5. **Nada de terceros en la ruta crítica** sin una razón que el cliente entienda: cada
   script externo es rendimiento, privacidad, CSP y un párrafo legal más.
6. **El cliente decide el gusto; tú aportas la medición.** Cuando retire algo que
   funcionaba, anota qué se midió por si vuelve, y no lo repropongas sin que lo pida.
7. **Deja el motivo escrito junto al código.** Un arreglo sin el porqué se deshace en la
   siguiente sesión. Los docstrings largos de este proyecto evitaron docenas de
   regresiones.
8. **Al terminar cualquier tanda, actualiza el contrato del proyecto**: qué se cambió,
   qué se probó y se retiró, y qué queda pendiente y de quién depende.
