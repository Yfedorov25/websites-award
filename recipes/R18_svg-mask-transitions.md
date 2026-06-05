# R18 · SVG Mask Scroll Transitions
> Хвиля 1 (легка). Codrops 11 Mar 2026. Fullscreen image-reveal через SVG-маски + GSAP ScrollTrigger. Кінематографічні переходи зображень на скролі (blinds, grid, shapes).

## ЩО ЦЕ / КОЛИ
Зображення проявляються/змінюються через анімовані SVG-маски на scroll: горизонтальні жалюзі (strips), випадкова сітка клітинок, геометричні форми, що розкриваються. Дешевше за WebGL, працює всюди, дає «дорогий» reveal.
**Коли:** галереї, hero-переходи між кадрами, секції-розповіді, де треба ефектне розкриття зображення без WebGL.

## РІВЕНЬ РИЗИКУ: 🟢
Чистий SVG + GSAP. Claude Code робить легко, працює на всіх пристроях.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **Codrops «SVG Mask Transitions on Scroll with GSAP and ScrollTrigger»** (11 Mar 2026).
- GSAP ScrollTrigger + Lenis (R04).

## ПРИНЦИП
1. Зображення лежить під SVG-маскою (`<mask>` з білими формами = видимі зони).
2. На scroll форми маски анімуються (ширина/scale/кількість) через ScrollTrigger scrub.
3. viewBox у віртуальних одиницях 0-100 → форми незалежні від пікселів.
4. `shape-rendering="crispEdges"` — чіткі краї без antialiasing-щілин.

## КОД-СКЕЛЕТ
```html
<div class="reveal-section">
  <img class="reveal-img" src="/hero.webp" alt=""/>
  <svg class="reveal-mask" viewBox="0 0 100 100" preserveAspectRatio="none">
    <defs>
      <mask id="blinds">
        <!-- 30 горизонтальних смуг (жалюзі) -->
        <!-- генеруються JS: кожна rect height=100/30, width анімується 0→100 -->
        <rect class="strip" x="0" y="0"   width="0" height="3.34" fill="#fff" shape-rendering="crispEdges"/>
        <!-- ... ×30 -->
      </mask>
    </defs>
    <rect width="100" height="100" fill="#000" mask="url(#blinds)"/>
  </svg>
</div>
```
```js
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)

// генеруємо 30 смуг
const STRIPS = 30, mask = document.querySelector('#blinds')
for (let i=0;i<STRIPS;i++){
  const r = document.createElementNS('http://www.w3.org/2000/svg','rect')
  r.setAttribute('x','0'); r.setAttribute('y', (i*100/STRIPS).toString())
  r.setAttribute('width','0'); r.setAttribute('height',(100/STRIPS).toString())
  r.setAttribute('fill','#fff'); r.setAttribute('shape-rendering','crispEdges')
  r.classList.add('strip'); mask.appendChild(r)
}

// анімація маски на scroll (жалюзі розкриваються)
gsap.to('.strip', {
  attr: { width: 100 },
  ease: 'none',
  stagger: { each: 0.02, from: 'random' },   // випадковий порядок = живіше
  scrollTrigger: {
    trigger: '.reveal-section', start: 'top top', end: '+=150%',
    scrub: 2.2, pin: true,
  }
})
```
**Варіації маски:** grid (N×M клітинок, scale 0→1, from:'random'), wipe (одна форма), circles (radius 0→max), diagonal blinds.

## ТОЧНІ ПАРАМЕТРИ (з Codrops)
- viewBox 0-100 (віртуальні одиниці), `preserveAspectRatio="none"` (розтягнути під екран).
- `shape-rendering="crispEdges"` (без щілин між смугами).
- scrub 2.0-2.5 (плавне доганяння).
- Смуг 20-40; клітинок 8×8...12×12.
- stagger `from:'random'` (органічніше за лінійний) або `from:'center'`.
- pin секцію, end `+=150%`.

## PERFORMANCE + FALLBACK
- SVG-маски дешеві (CPU/GPU compositing). Не зловживати кількістю форм (>100 = лаги).
- reduced-motion: показати зображення без маски (instant).
- Mobile: менше смуг (15-20), коротший pin.
- ScrollTrigger.refresh() після завантаження зображень.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R18_svg-mask-transitions.md.
Зроби SVG-mask scroll reveal (GSAP ScrollTrigger + Lenis):
- зображення під SVG <mask> (viewBox 0-100, preserveAspectRatio none, shape-rendering crispEdges)
- 3 варіанти маски: blinds (30 смуг width 0→100), grid (10×10 scale 0→1), wipe
- анімація на scroll: scrub 2.2, pin, stagger from:'random' each 0.02
- reduced-motion → instant; mobile → 15-20 смуг; ScrollTrigger.refresh після load
Базуйся на Codrops «SVG Mask Transitions on Scroll». Поверни компонент + демо з 3 масками.
```
