# Control de versiones y commits

## Desde el primer día

- `git init` en la raíz, rama `main`, y el primer commit ANTES de empezar a tocar el
  maestro. Lo que no está versionado se pierde en la primera pasada masiva.
- **`.gitignore`**: `.DS_Store`, `node_modules/`, `.env`, carpetas temporales. Los
  `.DS_Store` además hay que borrarlos de la carpeta que se publica (ver `06-seguridad.md`).
- **Versiona lo que no se puede regenerar**, aunque parezca generado: si la carpeta de
  publicación contiene imágenes derivadas que ningún script vuelve a producir, entra
  entera. Y es lo que permite volver a la versión publicada anterior.
- **Los originales en alta resolución entran** si no están en ningún otro sitio: son
  material irreemplazable del cliente.

## Remoto: es decisión del cliente

**Subir el repositorio publica su código y su contenido**, incluidos textos que quizá no
estén aprobados. No se configura un remoto sin pedirlo, y cuando se pida:

- **Repositorio privado**, salvo que el cliente diga lo contrario por escrito.
- Antes del primer `push`, un barrido de claves, tokens, credenciales y datos personales
  que no deban salir (`06-seguridad.md`).
- Nada de reescribir la historia de un repositorio que otro ya haya clonado.

## Cómo escribir los commits

En el idioma del proyecto, y pensando en quien lo lea dentro de seis meses:

- **Título en una línea**, en presente y en términos del visitante: «Cita de la carta en
  la portada y hilo sin quiebros», no «cambios varios en pasos.py».
- **Cuerpo con el porqué y lo medido**: qué pedía el cliente, qué se cambió, qué número
  lo respalda y qué se probó y se retiró. Es el mismo criterio que el CLAUDE.md del
  proyecto, y evita repetir propuestas ya descartadas.
- **Las líneas de atribución** que pida la configuración de la sesión van al final del
  mensaje, tal cual.
- **Un commit por tanda con sentido propio**, no uno por fichero ni uno por semana.

## Commits y despliegue

- **Commit antes de subir.** Lo que se publica tiene que existir en la historia; si no,
  no hay forma de volver atrás.
- **Recompila antes del commit** para que la carpeta publicada y el código vayan
  sincronizados en el mismo punto.
- **Cuidado con los ficheros de configuración del servidor**: si el sitio calcula hashes
  de los scripts para su política de seguridad, cualquier cambio de JavaScript cambia ese
  fichero, y hay que subirlo con el resto o la web se rompe **sólo en producción**.
- **Revisa `git status` antes de confirmar**: en un sitio compilado, un cambio toca
  muchos ficheros de salida a la vez. Un fichero que cambia sin que sepas por qué es una
  señal, no ruido.
- Tras la subida, comprueba que lo que sirve el servidor coincide con lo compilado en ese
  commit.
