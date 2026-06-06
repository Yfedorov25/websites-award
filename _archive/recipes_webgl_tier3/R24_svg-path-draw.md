# R24 · Scroll-Driven SVG Path Draw + Camera Follow
> Хвиля 1 (легка). Cinematic SVG-анімація: лінія/маршрут малюється на scroll, камера-в'юпорт слідує за точкою. Ідеально для маршрутів, таймлайнів, процесів, мап.

## ЩО ЦЕ / КОЛИ
SVG-шлях (дорога, маршрут, лінія процесу, контур) поступово «малюється» зі scroll-прогресом; опційно SVG-viewBox «їде» вздовж шляху (камера), фокусуючись на активній точці. POI-маркери з'являються по ходу.
**Коли:** «як дістатися» (нерухомість/готель), таймлайн історії бренду, крок-за-кроком процес, інтерактивна стилізована мапа, storytelling-маршрут.

## РІВЕНЬ РИЗИКУ: 🟢
GSAP DrawSVG + MotionPath + ScrollTrigger. Claude Code легко.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- Codrops «cinematic SVG map animation» (path-drawing + motion-tracking + smooth camera).
- GSAP DrawSVGPlugin (малювання шляху), MotionPathPlugin (рух уздовж шляху), ScrollTrigger.
- Чистий SVG-альтернатива: `stroke-dasharray`/`stroke-dashoffset` (без plugin).

## ПРИНЦИП
1. SVG-`<path>` з `stroke-dasharray = довжина шляху`, `stroke-dashoffset = довжина` (схований).
2. На scroll `dashoffset → 0` (малюється) через ScrollTrigger scrub.
3. Маркер/«машинка» рухається вздовж шляху (MotionPath) синхронно.
4. Опційно: viewBox SVG зсувається, тримаючи активну точку в центрі (камера).

## КОД-СКЕЛЕТ (GSAP)
```js
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { DrawSVGPlugin } from 'gsap/DrawSVGPlugin'
import { MotionPathPlugin } from 'gsap/MotionPathPlugin'
gsap.registerPlugin(ScrollTrigger, DrawSVGPlugin, MotionPathPlugin)

const tl = gsap.timeline({
  scrollTrigger: { trigger:'.map-section', start:'top top', end:'+=300%', scrub:1, pin:true }
})

// 1. малюємо маршрут
tl.fromTo('#route', { drawSVG:'0%' }, { drawSVG:'100%', ease:'none' })

// 2. маркер їде по маршруту синхронно
tl.to('#marker', {
  motionPath: { path:'#route', align:'#route', alignOrigin:[0.5,0.5], autoRotate:true },
  ease:'none'
}, 0)  // паралельно (position 0)

// 3. POI з'являються по ходу
gsap.utils.toArray('.poi').forEach((poi,i) => {
  tl.fromTo(poi, {scale:0,opacity:0}, {scale:1,opacity:1,duration:0.1}, i*0.25)
})
```
**Без plugin (чистий CSS/JS):**
```js
const path = document.querySelector('#route')
const len = path.getTotalLength()
path.style.strokeDasharray = len
path.style.strokeDashoffset = len
gsap.to(path, { strokeDashoffset: 0, ease:'none',
  scrollTrigger:{ trigger:'.map-section', start:'top top', end:'+=300%', scrub:1, pin:true }})
```

## ТОЧНІ ПАРАМЕТРИ
- scrub 1, pin true, end `+=200-300%` (довжина = «час» малювання).
- ease `none` (лінійно, прив'язано до scroll).
- MotionPath `autoRotate:true` (маркер повертається за напрямком шляху).
- DrawSVG потребує GSAP Club (платний) — або безкоштовний `stroke-dashoffset` метод (вище).
- POI stagger вздовж timeline по позиціях (0.25, 0.5, 0.75...).

## PERFORMANCE + FALLBACK
- SVG-анімація дешева. `will-change` обережно.
- reduced-motion: показати повний намальований маршрут одразу.
- Mobile: коротший pin; viewBox-камеру можна вимкнути (просто draw).
- getTotalLength() — раз на init, не щокадру.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R24_svg-path-draw.md.
Зроби scroll-driven SVG-маршрут (GSAP ScrollTrigger):
- SVG <path> малюється на scroll (stroke-dashoffset len→0, або DrawSVG якщо є Club)
- маркер їде по шляху (MotionPath align autoRotate) синхронно
- POI-точки з'являються по ходу (stagger по timeline)
- scrub 1, pin, end +=300%, ease none
- reduced-motion → повний маршрут одразу; mobile → коротший pin
Поверни компонент + демо (стилізований маршрут «від метро до ЖК» з 4 POI).
```
