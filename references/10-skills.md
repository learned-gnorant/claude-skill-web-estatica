# Qué skill o herramienta usar para cada cosa

Nombres tal como aparecen en el listado de skills de Claude Code. Comprueba el listado de
la sesión: cambia según lo que haya instalado.

| Tarea | Con qué |
|---|---|
| Dirección de diseño, filtro anti-slop, rediseños | `design-taste-frontend` |
| Identidad, tokens, logotipo, piezas gráficas, banners | `design` |
| Pulir una interfaz ya construida | `impeccable`, `emil-design-eng` |
| Revisar UI contra buenas prácticas y accesibilidad | `web-design-guidelines` |
| Construir una animación desde cero | `animate` |
| Ponerle nombre a un efecto que sólo sabes describir | `animation-vocabulary` |
| Auditar el movimiento de un código ya escrito | `improve-animations`, `review-animations` |
| SEO: auditoría, schema, sitemap, contenido, GEO | plugin `claude-seo` (`seo`, `seo-audit`, `seo-technical`, `seo-schema`…) |
| Revisión de seguridad del código | `security-review` |
| Revisión de código de una tanda de cambios | `code-review` |
| Medir en el navegador de verdad | `claude-in-chrome` + el MCP de Chrome DevTools |
| Levantar la app o la web para verla | `run` |
| Tocar permisos, hooks y ajustes de Claude Code | `update-config`, `fewer-permission-prompts` |
| Entregar al cliente algo que se lea fuera del terminal | herramienta `Artifact` + `artifact-design` |
| Gráficos y visualizaciones | `dataviz` |

## Herramientas, y cuándo usar cada una

- **Chrome DevTools (MCP)** para medir: permite emular viewport y `deviceScaleFactor`,
  esperar a las fuentes, ejecutar JavaScript y capturar. Es lo que hace falta para
  geometría, animación y rendimiento.
- **Claude in Chrome** cuando hay que entrar en paneles del cliente (Search Console, Bing,
  el panel del formulario): trabaja sobre su sesión abierta. Pide permiso explícito antes
  de aceptar accesos o permisos OAuth, y cierra las pestañas al terminar.
- **Sub-agentes propios del proyecto** (`.claude/agents/*.md`): útiles para auditorías
  repetibles —seguridad, accesibilidad, preparación del despliegue—. Defínelos cuando una
  comprobación se vaya a repetir muchas veces; para una vez, no compensan.
- **Hooks** (`.claude/settings.json`) para lo que debe pasar SIEMPRE: recompilar al tocar
  el canal, recordar la verificación antes de desplegar.

## Cómo organizar el conocimiento entre proyectos

Tres capas, y cada una tiene su coste:

1. **`CLAUDE.md` del proyecto** — se carga ENTERO en cada sesión. Aquí sólo el contrato:
   negocio, decisiones cerradas, reglas que no se rompen, pendientes. Objetivo: menos de
   400 líneas. El de un proyecto llegó a 3.391 líneas y 214 KB, que son unos 55.000
   tokens gastados en cada sesión antes de escribir la primera instrucción.
2. **Skills de usuario** (`~/.claude/skills/<nombre>/SKILL.md`) — sólo se carga el nombre
   y la descripción; el cuerpo entra cuando hace falta. Aquí va el conocimiento
   reutilizable entre proyectos, como esta misma skill. Los ficheros de `references/` no
   se cargan hasta que se leen: divide por temas y no temas que sean largos.
3. **`~/.claude/CLAUDE.md`** — preferencias personales que valen en todos los proyectos
   (idioma de las respuestas, forma de commitear, gustos de estilo). Muy corto.

Regla para decidir dónde va algo: **si vale para el próximo cliente, es skill; si sólo
vale para este sitio, es CLAUDE.md del proyecto; si vale para cómo trabajas tú, es el
CLAUDE.md global.**
