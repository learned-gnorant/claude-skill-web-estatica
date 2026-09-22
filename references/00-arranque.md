# Arranque de un proyecto y contrato de sesión

## Qué hay que saber antes de escribir una línea

Pregúntalo. No lo deduzcas del maestro ni de la marca:

- **Negocio**: a qué se dedica, a quién vende, cuántos años lleva, qué la distingue.
  Cualquier cifra que se publique («más de 25 años», «300 títulos») tiene que salir de
  aquí y ser la MISMA en todo el sitio.
- **Dominios**: cuál es el bueno, cuáles redirigen, si alguno estuvo aparcado (ver
  `07-seo.md`, deja rastro en Google durante meses).
- **Alojamiento**: compartido o VPS, si hay `.htaccess`, si hay CDN delante, quién sube
  los archivos y cómo (FTP, panel, git).
- **Idiomas**: uno o dos. Si son dos, ¿carpetas duplicadas o un gestor? En estático,
  carpetas: `/` y `/en/`, cada página existe dos veces con su slug traducido.
- **Formulario**: a qué buzón entrega, quién tiene la cuenta del servicio.
- **Legal**: razón social, NIF, domicilio, datos registrales, teléfonos, correos. Si hay
  local abierto al público (decide si cabe `LocalBusiness` en el schema y ficha de Google).
- **Cookies**: si no hay analítica ni terceros, no hace falta banner. Decidirlo pronto
  evita instalar y desinstalar media web.
- **Imágenes y vídeos**: quién los aporta, si hay derechos, si los vídeos van en YouTube.

## Plantilla de CLAUDE.md del proyecto

Corto (menos de 400 líneas) y en la raíz del repositorio. Es lo único que se carga en
cada sesión, así que sólo va lo que no se puede deducir del código.

```markdown
# <Cliente> — web corporativa

Sitio estático de **<dominio>**. Este archivo es el contrato de la sesión: si algo aquí
contradice lo que parece decir el código, gana este archivo.

## 1. Negocio
Qué es la empresa, a quién se dirige, cifras oficiales (y sus fuentes), qué NO es.

## 2. Arquitectura — decidido, no reabrir
- Alojamiento, backend (o su ausencia), base de datos.
- Idiomas y estructura de carpetas.
- Formulario: servicio y endpoint.
- Cookies y analítica: qué hay y qué no.
- Vídeo, imágenes: formatos y reglas.
- Lo que está FUERA de alcance.

## 3. Identidad
Paleta con el papel de cada color, tipografías y de dónde se sirven, recursos gráficos
propios (bandas, filetes, tramas) con su porqué.

## 4. Estructura del proyecto
Qué hace cada módulo del canal de compilación, en una línea por módulo.

## 5. Decisiones tomadas y por qué
Una entrada por decisión, con fecha. Incluye lo PROBADO Y RETIRADO: qué se midió y por
qué se quitó, para no reproponerlo.

## 6. Reglas que no se rompen
Lo que debe hacer parar la compilación, lo que nunca se toca a mano, lo que no se
publica sin verificar.

## 7. Pendiente, y de quién depende
Separado en «lo que puedo hacer yo» y «lo que depende del cliente».
```

## Cómo crece ese documento

- **Una sección por cambio de verdad**, con la fecha y el motivo. Lo valioso no es qué se
  hizo, es **qué se midió y qué falló**.
- **Anota lo retirado.** «Se probó X, el cliente lo quitó el mismo día, esto es lo que se
  midió» ahorra repetir la propuesta dentro de tres semanas.
- **Poda cuando pase de ~400 líneas**: lo que ya es estable y evidente en el código sale;
  lo que sigue siendo contraintuitivo se queda. Si un tema crece mucho (movimiento,
  legal), muévelo a `docs/` o a una skill y deja en CLAUDE.md una línea con el puntero.

## Orden de trabajo que funcionó

1. Contrato y decisiones (arriba).
2. Canal de compilación y una página de prueba end-to-end, **incluido el despliegue**:
   subir algo el primer día descubre el 80 % de las sorpresas del servidor.
3. Contenido e identidad, página a página.
4. Legal y formulario, atados a la compilación.
5. Seguridad y cabeceras.
6. SEO e indexación: sólo cuando el contenido es el definitivo.
7. Rendimiento y accesibilidad medidos, no intuidos.
