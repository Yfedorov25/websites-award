# R19 · GSAP Flip Grid Morphs (Layout Transitions)
> Хвиля 1 (легка). Codrops 20 Jan 2026. Плавний морфінг між layout-станами (меню↔сітка, grid↔list, фільтрація) через GSAP Flip (FLIP technique).

## ЩО ЦЕ / КОЛИ
Елементи плавно «перетікають» між різними розкладками: меню розгортається у сітку проєктів, картки перешиковуються при фільтрі, один елемент розширюється на весь екран. GSAP Flip автоматично рахує різницю позицій (First-Last-Invert-Play) і анімує.
**Коли:** портфоліо-фільтри, меню→галерея, grid↔list toggle, expand-картка, будь-який layout-перехід без «стрибка».

## РІВЕНЬ РИЗИКУ: 🟢
GSAP Flip plugin робить усю математику. Claude Code легко.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **Codrops «Animating Responsive Grid Layout Transitions with GSAP Flip»** (20 Jan 2026).
- Репо: codrops/MenuToGrid, codrops/GridLayoutAnimation, codrops/OneElementScroll.
- GSAP Flip plugin (безкоштовний з GSAP 3.12+).

## ПРИНЦИП (FLIP)
1. **First:** `Flip.getState(elements)` — зняти поточні позиції/розміри.
2. Змінити DOM/CSS (новий layout — клас, grid-template, переміщення в інший контейнер).
3. **Last/Invert/Play:** `Flip.from(state, {...})` — GSAP рахує дельту й анімує від старого до нового.

## КОД-СКЕЛЕТ
```js
import gsap from 'gsap'
import { Flip } from 'gsap/Flip'
gsap.registerPlugin(Flip)

const items = gsap.utils.toArray('.grid-item')
const container = document.querySelector('.grid')

function morphLayout(newClass) {
  const state = Flip.getState(items)        // FIRST: зняти стан
  container.className = `grid ${newClass}`   // змінити layout (CSS grid-template міняється)
  Flip.from(state, {                          // LAST+INVERT+PLAY
    duration: 0.8, ease: 'power3.inOut',
    stagger: 0.04,
    absolute: true,                           // позиціонувати absolute під час анімації (плавніше)
    onEnter: el => gsap.fromTo(el, {opacity:0,scale:0}, {opacity:1,scale:1,duration:0.5}),
    onLeave: el => gsap.to(el, {opacity:0,scale:0,duration:0.5}),
  })
}

// перемикачі
document.querySelector('#grid-view').onclick = () => morphLayout('view-grid')
document.querySelector('#list-view').onclick = () => morphLayout('view-list')
```
```jsx
// React-версія (useFlip pattern)
import { useRef } from 'react'
import { Flip } from 'gsap/Flip'
function Gallery({ filter }) {
  const ref = useRef()
  useGSAP(() => {
    const state = Flip.getState(ref.current.querySelectorAll('.item'))
    return () => Flip.from(state, { duration:0.7, ease:'power3.inOut', stagger:0.03, absolute:true })
  }, { dependencies:[filter], scope:ref })  // спрацьовує при зміні filter
  return <div ref={ref} className="grid">{/* items */}</div>
}
```

## ТОЧНІ ПАРАМЕТРИ
- duration 0.7-0.9, ease `power3.inOut` (симетричний вхід/вихід).
- stagger 0.03-0.05 (легка хвиля).
- `absolute: true` — обов'язково для складних reflow (елементи не штовхають одне одного під час анімації).
- `onEnter`/`onLeave` — для елементів, що з'являються/зникають (фільтр).
- `nested: true` — якщо анімуються вкладені Flip-елементи.

## PERFORMANCE + FALLBACK
- Flip дешевий (transform-based). `absolute:true` уникає layout-thrash.
- Багато елементів (>50) — обмежити stagger або вимкнути на mobile.
- reduced-motion: миттєва зміна layout без анімації (`duration:0`).
- Уникати анімації width/height напряму — Flip робить через scale (дешевше).

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R19_gsap-flip-grid.md.
Зроби layout-морфінг через GSAP Flip:
- Flip.getState → зміна CSS-класу контейнера (grid-template) → Flip.from
- сценарії: grid↔list toggle, фільтр проєктів (onEnter/onLeave fade+scale), menu→grid
- params: duration 0.8, power3.inOut, stagger 0.04, absolute:true
- React: useGSAP з dependencies:[filter]
- reduced-motion → instant (duration 0); mobile → менше stagger
Репо-орієнтир: codrops/MenuToGrid, codrops/GridLayoutAnimation. Поверни компонент + демо (фільтр-галерея).
```
