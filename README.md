# Guía: webs estáticas con identidad propia

Skill para Claude Code con lo aprendido construyendo y publicando una web corporativa
bilingüe, estática y sin backend: canal de compilación, diseño, movimiento, textos
legales y RGPD, formularios sin backend, seguridad, SEO, medios, método de verificación
y control de versiones.

## Instalación

```bash
git clone <url-de-este-repo> ~/.claude/skills/web-estatica
```

Claude Code la detecta sola: aparece en el listado de skills de cualquier proyecto y se
invoca con `/web-estatica` o pidiéndola por su nombre al arrancar.

## Contenido

- `SKILL.md` — índice y las ocho reglas que valen en cualquier web de este tipo.
- `references/` — un fichero por tema; sólo se leen cuando hacen falta.

## Avisos

**No es asesoramiento jurídico.** El apartado legal explica cómo evitar que una web
publique afirmaciones falsas sobre sí misma (qué datos trata, a quién se conectan sus
páginas, qué cookies instala) y qué exige la normativa española y europea a grandes
rasgos. Los textos concretos los valida un profesional.

**Tiene fecha.** Todo se midió entre agosto y septiembre de 2026, con las versiones de
navegadores y servicios de entonces. Lo que depende de un tercero —el comportamiento de
un servicio de formularios, los plazos de carga de tipografías del navegador, los
paneles de los buscadores— envejece: compruébalo antes de apoyarte en ello. Las
afirmaciones más perecederas llevan su fecha entre paréntesis.

**Licencia:** CC BY 4.0 (ver `LICENSE`).

## Cómo mantenerla

Al terminar cada proyecto, lleva aquí lo aprendido que sirva para el siguiente. Lo que
sólo vale para ese sitio se queda en su `CLAUDE.md` (plantilla en
`references/00-arranque.md`).
