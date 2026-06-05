# R17 · MSDF Dissolve / Gommage (Text → Dust/Petals)
> Хвиля 3 (frontier). Codrops 28 Jan 2026 (Thibault Introvigne). MSDF-текст дезінтегрується у пил/пелюстки через noise-driven TSL-шейдер + синхронні particles. Преміальний text-reveal.

## ЩО ЦЕ / КОЛИ
Чіткий MSDF-текст (нескінченно масштабований) розчиняється/«стирається» (gommage = франц. «стирання»): noise-маска з'їдає літери, а синхронно вилітають dust/petal частинки. Зворотно — текст «збирається» з пилу.
**Коли:** драматична поява заголовка, hero-текст бренду, перехід між словами/секціями, luxury-typography момент.

## РІВЕНЬ РИЗИКУ: 🔴 (Advanced/Expert)
MSDF-генерація + TSL noise-шейдер + instanced particles + MRT bloom. Не для кожного проєкту — як головний акцент.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **Codrops «WebGPU Gommage Effect: Dissolving MSDF Text into Dust and Petals with Three.js & TSL»** (28 Jan 2026, Thibault Introvigne).
- **Three MSDF Text** (Léo Mouraire) — рендер MSDF у Three.js.
- **msdf-bmfont** (Shen Yiming / msdf-bmfont-xml) — генерація MSDF-атласу зі шрифту.
- GSAP (тригер dissolve-прогресу). TSL (R15) + MRT (selective bloom).

## ПРИНЦИП
1. **MSDF-атлас** шрифту (multi-channel signed distance field) — чіткі краї на будь-якому масштабі.
2. **Dissolve-шейдер:** noise(uv, time) порівнюється з порогом `uProgress`; де noise < progress → alpha 0 (стерто). Край порогу світиться (bloom).
3. **Particles:** на «з'їдених» позиціях спавняться instanced dust/petals (читають ту саму noise-маску), розлітаються.
4. **MRT:** край dissolve пише в emissive → selective bloom.
5. GSAP анімує `uProgress` 0→1 (стирання) або 1→0 (збирання).

## КОД-СКЕЛЕТ (концепт TSL)
```js
import { Fn, texture, uv, uniform, step, smoothstep, vec4, mix } from 'three/tsl'
import { MeshBasicNodeMaterial } from 'three/webgpu'

const uProgress = uniform(0)        // 0 = цілий текст, 1 = повністю стерто
const msdfTex = texture(msdfAtlas)
const noiseTex = texture(noiseTexture)

const mat = new MeshBasicNodeMaterial({ transparent:true })
mat.colorNode = Fn(() => {
  // MSDF → alpha літери
  const msdf = msdfTex.sample(uv())
  const sigDist = /* median(msdf.rgb) */ msdf.g
  const letterAlpha = smoothstep(0.4, 0.5, sigDist)
  // dissolve по noise
  const n = noiseTex.sample(uv()).r
  const dissolve = step(uProgress, n)              // 1 поки noise > progress
  const edge = smoothstep(uProgress, uProgress.add(0.05), n) // світний край
  const alpha = letterAlpha.mul(dissolve)
  const glow = letterAlpha.mul(edge.oneMinus())    // край → emissive
  return vec4(mix(baseColor, glowColor, glow), alpha)
})()
// GSAP: gsap.to(uProgress, { value:1, duration:2, ease:'power2.inOut' })
```
> Production: клонувати/адаптувати Codrops-демо (там готові MSDF-loader, particles, MRT-bloom).

## ТОЧНІ ПАРАМЕТРИ
- MSDF threshold ~0.5 (`smoothstep(0.4,0.5,sigDist)`).
- Dissolve edge width 0.03-0.07 (ширина світного краю).
- Particles: спавн на edge, кількість ~5-20k, розліт + gravity + fade.
- GSAP `uProgress` duration 1.5-2.5s, `power2.inOut`.
- Noise: fbm/curl для органічного «з'їдання».
- Bloom threshold 0.7 на emissive-краю.

## PERFORMANCE + FALLBACK
- MSDF дешевий (1 текстура, чіткий на будь-якому розмірі).
- Particles — головна вартість; адаптивна кількість (mobile 2-5k).
- WebGPU краще для particles-compute; WebGL — менше частинок.
- reduced-motion: показати текст без dissolve (статично).
- Fallback без WebGL: звичайний HTML-текст з CSS-fade.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R17_msdf-dissolve.md і R15 (TSL).
Зроби gommage text-dissolve (адаптуй Codrops-демо, НЕ з нуля):
- MSDF-атлас зі шрифту (msdf-bmfont), рендер через Three MSDF Text
- dissolve-шейдер TSL: noise vs uProgress, світний край → emissive (MRT bloom)
- instanced dust/petal particles на edge (5-20k desktop / 2-5k mobile), розліт+gravity+fade
- GSAP анімує uProgress 0→1 (2s power2.inOut); реверс = збирання
- reduced-motion → статичний текст; no-WebGL → HTML+CSS fade
Поверни компонент + який шрифт→MSDF згенерувати. Орієнтир: Codrops Gommage (28 Jan 2026).
```
