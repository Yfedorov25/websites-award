# R16 · Ping-Pong Fluid X-Ray Reveal (Dual-Scene)
> Хвиля 2-3. Реальний код: github.com/cullenwebber/three-skull (Codrops 23 Mar 2026, MIT, Three r183 + TSL/WebGPU + maath). Курсор «розкриває» внутрішній шар крізь fluid-маску. Драматичний reveal-ефект.

## ЩО ЦЕ / КОЛИ
Дві накладені сцени (зовнішня «solid» + внутрішня «X-ray»/Fresnel). Рух миші створює fluid-слід (ping-pong симуляція), що працює як маска — крізь нього видно внутрішній шар. Composite + bloom + scan lines + CRT-post.
**Коли:** «розкрий приховане» (продукт зсередини, скелет/анатомія, tech-нутрощі), драматичний hero, інтерактивний reveal бренду. Висока «вау»-цінність.

## РІВЕНЬ РИЗИКУ: 🔴 (WebGPU/TSL) → 🟡 з WebGL-гілкою
Не пишемо fluid-solver з нуля — беремо готове репо. WebGPU-версія + WebGL-фолбек уже в репо (гілка WebGL).

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **github.com/cullenwebber/three-skull** (MIT). Стек: `three@0.183` (three/webgpu + TSL), `maath` (easing). WebGL fallback у окремій гілці.
- Codrops «Dual-Scene Fluid X-Ray Reveal» (23 Mar 2026, Cullen Webber).
- Demo: tympanus.net/Tutorials/SkeletonFluidReveal/

## ПРИНЦИП (з опису репо)
1. **Mouse-trail → CanvasTexture** (2D-слід руху миші).
2. **Fluid sim на двох RenderTargets** — ping-pong (читання з A, запис у B, swap; advection+dissipation). Дає рідкий розтічний слід.
3. **Дві instanced-сцени:** solid-модель (зовні) + Fresnel X-ray модель (всередині).
4. **Composite:** fluid-маска визначає де показати X-ray крізь solid.
5. **Post:** bloom + scan lines + CRT-style (TSL nodes / MRT).

## КОД-СКЕЛЕТ (концепт; деталі — у репо)
```js
import * as THREE from 'three/webgpu'
import { Fn, texture, uv, vec4, /* TSL */ } from 'three/tsl'
import { easing } from 'maath'

// 1. mouse trail у CanvasTexture
const trailCanvas = document.createElement('canvas')
const trailTex = new THREE.CanvasTexture(trailCanvas)
// малюємо м'які кола по руху миші, fade щокадру

// 2. ping-pong fluid RenderTargets
let rtA = new THREE.RenderTarget(w, h), rtB = new THREE.RenderTarget(w, h)
const fluidPass = /* TSL Fn: читає rtA + trailTex, advection+dissipation, пише rtB */
function stepFluid(renderer){
  renderer.setRenderTarget(rtB); renderer.render(fluidQuad, ortho)
  [rtA, rtB] = [rtB, rtA]  // swap
}

// 3. дві сцени (instanced solid + Fresnel xray)
// 4. composite: mask = fluid value → mix(solidColor, xrayColor, mask)
// 5. post: bloom(MRT emissive) + scanlines + CRT
```
> Для production: `git clone github.com/cullenwebber/three-skull`, замінити моделі на свої (solid + inner), налаштувати кольори/post. WebGL-версія в гілці `WebGL`.

## ТОЧНІ ПАРАМЕТРИ
- Fluid dissipation 0.95-0.98 (як швидко зникає слід).
- Trail radius під розмір екрана; fade trailCanvas ~0.05/кадр.
- Fresnel power 2-3 (X-ray краї).
- Bloom threshold 0.7-0.85, scan lines субтильні.
- maath `easing.damp` для згладження mouse-позиції.

## PERFORMANCE + FALLBACK
- Ping-pong fluid = 2 RT-проходи щокадру → помірно дорого; resolution маски менше за екран (half-res).
- WebGPU-гілка швидша; **WebGL-гілка репо = фолбек** для браузерів без WebGPU.
- Mobile / pointer:coarse: вимкнути fluid-маску (показати solid або статичний reveal).
- reduced-motion: статичний composite без trail.
- Detect WebGPU → інакше WebGL-гілка.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R16_fluid-xray-reveal.md.
Клонуй github.com/cullenwebber/three-skull (НЕ пиши fluid solver). Адаптуй:
- mouse-trail → CanvasTexture; ping-pong fluid на 2 RenderTargets (dissipation 0.97, half-res)
- дві instanced-сцени: solid (зовні) + Fresnel X-ray (всередині), composite по fluid-масці
- post: bloom (threshold 0.8) + scanlines + CRT (TSL/MRT)
- заміни моделі на [наші: solid-продукт + inner-шар]
- WebGPU + WebGL-фолбек (гілка WebGL); mobile/reduced-motion → статичний reveal
Поверни робочу інтеграцію + які 2 моделі підготувати (solid.glb + inner.glb).
```
