# Reglas de desarrollo — LP Visoría EC10 Talentos (Bogotá · Medellín)

## Stack
- Página = Design Component único: `Visoria EC10.dc.html` (estilos inline; Tailwind solo para utilidades responsivas de layout: `grid`, `lg:grid-cols-*`, `hidden lg:block`, `flex-col lg:flex-row`, anchos `lg:w-[..]`).
- Dependencias por CDN en `<helmet>` (sin bundler): Tailwind Play CDN, GSAP 3.12.5, ScrollTrigger 3.12.5, Lenis 1.1.x. Fuente: Source Serif 4.
- Todo el contenido en **español** (público: atletas de 11 a 25 años y sus padres/acudientes).

## Motion
- **GSAP**: todas las animaciones complejas (intro, reveals, timelines). Nunca animar con CSS `@keyframes` en el template.
- **ScrollTrigger**: toda animación ligada al scroll (scrub, pin, horizontal, progreso).
- **Lenis**: único responsable del smooth scroll. Integración obligatoria:
  - `lenis.on('scroll', ScrollTrigger.update)`
  - `gsap.ticker.add(t => lenis.raf(t * 1000))` + `gsap.ticker.lagSmoothing(0)`
  - No usar `requestAnimationFrame` propio para Lenis, ni `scroll-behavior: smooth` nativo, ni `scrollIntoView`.
  - Anclas internas: `lenis.scrollTo(target)`; fallback `window.scrollTo` si Lenis no existe (reduced motion).
- Breakpoints de animación con `gsap.matchMedia()` (horizontal pin solo ≥1024px).
- Sin `overflow: hidden` ni `transform` en ancestros de secciones con `pin` (usar `overflow-x: clip` en el root).

## Cleanup (obligatorio)
- Todo tween/trigger dentro de `gsap.context(fn, root)` → `ctx.revert()` en `componentWillUnmount`.
- `mm.revert()`, `gsap.ticker.remove(raf)`, `lenis.destroy()`, `clearInterval/clearTimeout`, `removeEventListener`.

## Performance y accesibilidad
- Animar solo `transform`, `opacity`, `clip-path`. Nada de animar `top/left/width/height`.
- `prefers-reduced-motion: reduce` → sin Lenis, sin intro, sin scrub; contenido visible.
- Fallback: si las librerías no cargan en 8s, el contenido se muestra sin animación.
- Skip link, `:focus-visible` dorado, labels en todos los campos, contraste ≥ 4.5:1 para texto.

## Marca
- Paleta derivada del escudo EC10 Talentos: navy `#050d22` / `#0a1a3f` / `#1b3a85`, oro `#e3b23c` / `#f2d27a`, crema `#f4efe3`, texto secundario `#9aa6c4`.
- Logo: `assets/ec10-crest.png` (recorte de baja resolución — reemplazar por versión vectorial/HD).

## Copy
- No prometer contratos ni resultados. "Una visoría es una puerta, no una garantía."
- No inventar cifras, testimonios ni sedes. Sede y valores: "se confirman por WhatsApp".
