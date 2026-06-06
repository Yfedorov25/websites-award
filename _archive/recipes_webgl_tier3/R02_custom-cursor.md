# R02 · Custom Cursor + Magnetic + Raycaster
> Один з найсильніших "дешевих" сигналів дорожнечі. Є три рівні: (A) DOM-курсор з lerp, (B) magnetic-кнопки, (C) 3D raycaster-курсор (як RayCursor.js у Bruno).

## ЩО ЦЕ / КОЛИ
Заміна системного курсора кастомним, що плавно наздоганяє мишу (lerp), змінює стан на hover (scale/режим), і "притягує" інтерактивні елементи (magnetic). У 3D — raycaster визначає, на що наведено в WebGL-сцені.
**Коли:** майже завжди на преміум-сайтах. Дешево додати, миттєво підвищує сприйняття.

## РІВЕНЬ РИЗИКУ: 🟢 (A, B) / 🟡 (C — 3D raycaster)

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- GSAP `quickTo` (офіційний патерн для smooth follow без лагів).
- Bruno folio `RayCursor.js` — raycaster зі станами intersect/isDown.
- Cuberto — еталон fluid cursor (для натхнення рівня C+).

## КОД-СКЕЛЕТ A — DOM-курсор з lerp (vanilla/React)
```jsx
// useCursor.js
import { useEffect, useRef } from 'react'
import gsap from 'gsap'

export function CustomCursor() {
  const dot = useRef(); const ring = useRef()
  useEffect(() => {
    const xToD = gsap.quickTo(dot.current, 'x', { duration: 0.15, ease: 'power3' })
    const yToD = gsap.quickTo(dot.current, 'y', { duration: 0.15, ease: 'power3' })
    const xToR = gsap.quickTo(ring.current, 'x', { duration: 0.4,  ease: 'power3' }) // ring лагає сильніше
    const yToR = gsap.quickTo(ring.current, 'y', { duration: 0.4,  ease: 'power3' })
    const move = (e) => { xToD(e.clientX); yToD(e.clientY); xToR(e.clientX); yToR(e.clientY) }
    window.addEventListener('pointermove', move)

    // hover-стан на інтерактивних
    const hoverables = document.querySelectorAll('a,button,[data-cursor]')
    const enter = () => gsap.to(ring.current, { scale: 2.4, duration: 0.3, ease: 'power3' })
    const leave = () => gsap.to(ring.current, { scale: 1,   duration: 0.3, ease: 'power3' })
    hoverables.forEach(el => { el.addEventListener('pointerenter', enter); el.addEventListener('pointerleave', leave) })

    return () => { window.removeEventListener('pointermove', move) /* + remove listeners */ }
  }, [])
  return (<>
    <div ref={ring} className="cursor-ring" />
    <div ref={dot}  className="cursor-dot" />
  </>)
}
```
```css
* { cursor: none; }                         /* сховати системний (тільки desktop!) */
.cursor-dot,.cursor-ring{ position:fixed; top:0; left:0; pointer-events:none; z-index:9999;
  border-radius:50%; transform:translate(-50%,-50%); mix-blend-mode:difference; }
.cursor-dot { width:8px; height:8px; background:#fff; }
.cursor-ring{ width:36px; height:36px; border:1px solid #fff; }
```

## КОД-СКЕЛЕТ B — Magnetic button
```jsx
function magnetic(el, strength = 0.4) {
  const xTo = gsap.quickTo(el, 'x', { duration: 0.6, ease: 'elastic.out(1,0.3)' })
  const yTo = gsap.quickTo(el, 'y', { duration: 0.6, ease: 'elastic.out(1,0.3)' })
  el.addEventListener('pointermove', (e) => {
    const r = el.getBoundingClientRect()
    xTo((e.clientX - (r.left + r.width/2)) * strength)
    yTo((e.clientY - (r.top + r.height/2)) * strength)
  })
  el.addEventListener('pointerleave', () => { xTo(0); yTo(0) })
}
```

## КОД-СКЕЛЕТ C — 3D Raycaster cursor (за Bruno RayCursor.js)
```js
const raycaster = new THREE.Raycaster()
const pointer = new THREE.Vector2()
window.addEventListener('pointermove', e => {
  pointer.x = (e.clientX / innerWidth) * 2 - 1
  pointer.y = -(e.clientY / innerHeight) * 2 + 1
})
// у ticker:
raycaster.setFromCamera(pointer, camera)
const hits = raycaster.intersectObjects(interactiveMeshes)
const hit = hits[0]?.object ?? null
if (hit !== current) { /* enter/leave стан: міняй курсор, hover-ефект на меші */ }
```

## ТОЧНІ ПАРАМЕТРИ
- dot lerp: 0.15s; ring lerp: 0.4s (різниця створює "живість").
- hover scale: 2–2.5×; ease: power3.out.
- magnetic strength: 0.3–0.5; release ease: elastic.out(1,0.3).
- mix-blend-mode: difference (працює на світлому/темному однаково).

## PERFORMANCE + FALLBACK
- ТІЛЬКИ desktop / pointer:fine. На touch — `cursor:none` НЕ застосовувати, курсор не рендерити.
  ```js
  if (window.matchMedia('(pointer:fine)').matches) mountCursor()
  ```
- reduced-motion: показати курсор без lerp (миттєвий) або лишити системний.
- quickTo не створює нових tween щокадру — нема GC-тиску.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R02_custom-cursor.md.
Зроби <CustomCursor/> для Next.js:
- dot (lerp 0.15s) + ring (lerp 0.4s) через gsap.quickTo, mix-blend difference
- hover-стан scale 2.4 на a/button/[data-cursor]
- хелпер magnetic(el, 0.4) для CTA-кнопок (elastic release)
- монтувати ТІЛЬКИ при (pointer:fine); на touch не рендерити, системний курсор лишити
- reduced-motion → без lerp
Без memory leaks (зняти всі listeners у cleanup). Поверни файли + демо-сторінку для перевірки.
```
