# Portal de Investigaciones GRD — Quarto site

Landing page (Quarto static site) for UNGRD's "Investigaciones en Gestión del
Riesgo de Desastres para Colombia" book series (2021/2023/2025 editions).
Live at https://scr-ungrd.github.io/investigaciones-grd-colombia/ (repo:
`scr-ungrd/investigaciones-grd-colombia`). Three pages: `index.qmd` (hero +
series covers + full chapter grid + other publications), `acerca-de.qmd`
(call for chapters, editorial process), `instrucciones-autor.qmd` (author
guidelines).

## Design system (2026-08-03 redesign)

Brought in line with the other two SCR sibling sites — `catalogo-scr` and
`fichas-departamentales-escenarios-riesgo` (each has its own CLAUDE.md) —
same institution, same brand manual, meant to read as one system:

- **Typography switched from Playfair Display + Lora + Source Sans 3
  (Google Fonts) to Helvetica Neue/Arial.** This was the most serious
  brand-manual violation found across the three sites — Playfair
  Display and Lora are both serif, and the manual explicitly disallows
  serif faces. Removed the Google Fonts `@import` entirely.
- **Navbar is the same translucent light "masthead" as the sibling
  sites, in both color modes.** `images/logo-ungrd.png` has its wordmark
  baked in as dark navy on transparent — no reversed/white version
  exists — so the bar can't go dark in dark mode without repeating the
  illegible-logo problem fixed first in catalogo-scr. Nav-link colors
  are hardcoded hex (`#232A60` / `#E54D23` / `#BF3D1A`, this site's own
  palette, unchanged from before), not swapped by the dark-mode CSS
  variables, for the same reason documented in the other two CLAUDE.mds.
  Added HUB SCR, GitHub, and translate-to-English navbar icons.
- **Hero, chapter grid, and cover-stack markup were untouched** — this
  site already used catalogo-scr's own class names (`nature-hero-section`,
  `chapter-card`, `section-title`, etc.) before this redesign, so only
  `custom.scss` needed rework: font-family everywhere, plus a stray
  non-brand hover color on `.chapter-title` (`#63b3ed`/`#0792f5`, not
  from the UNGRD palette) replaced with `var(--ungrd-blue-mid)` to match
  catalogo-scr's own chapter-card hover treatment.
- Cleaned up the dead `[data-bs-theme="dark"]` compound selectors
  (`[data-bs-theme="dark"] .foo, body.quarto-dark .foo { ... }`) down to
  just `body.quarto-dark .foo` — see catalogo-scr's CLAUDE.md gotcha #3
  for why the `[data-bs-theme="dark"]` half never actually matched
  anything (Quarto hardcodes that attribute to `"dark"` on the `<nav>`
  regardless of real color mode).
- Kept this project's own color tokens (`--ungrd-blue: #232A60`,
  `--ungrd-orange: #E54D23`, etc.) and the light-hero-with-dark-mode-navy
  pattern already in place — did **not** force the always-dark hero that
  `fichas-departamentales-escenarios-riesgo` explicitly asked for; that
  was a one-off request for that specific site, not a house rule.

## Deployment

GitHub Actions renders and publishes to `gh-pages` on push to `main` (same
pattern as the sibling sites). Local dev: `quarto preview` — this repo
isn't usually running a preview server between sessions.

## Barra de navegación institucional (2026-11-23) — reemplaza la "masthead" clara

Lo que este archivo dice arriba sobre una barra clara translúcida "porque no existe
un logo UNGRD en blanco" ya no aplica: ahora existe `images/logo-ungrd-blanco.svg`
(firma UNGRD a una tinta en negativo, según el Manual de Identidad Visual UNGRD 2024,
p. 11; el relleno interior de la mano usa el azul de la barra para dejar ver el
contorno). Las cinco páginas de inicio de la SCR (HUB, Catálogo, Fichas,
Investigaciones y Cuadernos Reproducibles) comparten la misma barra, con
Cuadernos Reproducibles como modelo:

- Fondo azul `#213362` con franja amarilla `#ffcd00` de 4 px abajo, en modo claro y oscuro.
- Orden: firma UNGRD | nombre del proyecto en dos líneas (la segunda en negrita y
  subrayada) · secciones del sitio · "Acerca de" · íconos (HUB, GitHub, Traducir) ·
  buscador · botón de tema en el extremo derecho. Sin ícono de casa: el logo y el
  nombre llevan a la portada.
- El nombre se escribe `"línea 1|línea 2"` en `navbar.title` de `_quarto.yml`; un
  script al final de `_footer.html` lo parte en dos líneas.
- Los estilos están en el bloque "BARRA DE NAVEGACIÓN INSTITUCIONAL" al final de
  `custom.scss`, que se impone a la sección NAVBAR antigua (se dejó por historia).
- Pie de página: mismo modelo (azul con franja amarilla, título, descripción,
  "Este sitio" y aviso de derechos), en `_footer.html` vía `include-after-body`.

- En este sitio el nombre va en una sola línea ("Investigaciones en GRD"), porque el nombre largo no cabía en la barra; el script de `_footer.html` le da el estilo de la segunda línea (negrita y subrayado).
