# R15 · TSL Dual-Pipeline (WebGL/WebGPU Shaders)
> Хвиля 3 (frontier). Головний технологічний зсув 2025-2026. Node-based шейдери `three/tsl`, що компілюються і у WebGL, і у WebGPU. Заміна raw GLSL для нових проєктів.
> Розширює R09 (WebGPU/TSL міграція) — тут фокус на ПИСАННІ шейдерів у TSL.

## ЩО ЦЕ / КОЛИ
TSL (Three Shading Language) — пишеш шейдери як JS-ноди (`Fn`, `vec3`, `uniform`, `texture`, `mix`...), а Three.js компілює у WGSL (WebGPU) або GLSL (WebGL2) автоматично. Один код → обидва бекенди + compute-шейдери на WebGPU.
**Коли:** будь-який новий проєкт з кастомними шейдерами у 2026; коли треба GPGPU/compute (R07); коли хочеш future-proof без дублювання GLSL/WGSL.

## РІВЕНЬ РИЗИКУ: 🔴 (Expert)
TSL-синтаксис відрізняється від GLSL; обмеження WebGPU (8 vertex buffers — обхід через bit-packing). Але фолбек на WebGL безкоштовний.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- `three/webgpu` + `three/tsl` (Three.js r168+, стабільніше r180+).
- Підтверджені кейси: Cartier W&W 2026, three-skull (R16), Gommage (R17), Shader.se pipeline, False Earth (WebGPU compute, >1M травинок).
- Bruno folio-2025 (R09 — реальний приклад renderer-конфігу).
- Maxime Heckel «Field Guide to TSL and WebGPU»; vite-plugin-tsl-operator.

## КЛЮЧОВИЙ ПРИНЦИП
```js
import * as THREE from 'three/webgpu'
import { Fn, vec3, vec4, uv, uniform, texture, mix, sin, time, positionLocal } from 'three/tsl'

// renderer з авто-фолбеком
const renderer = new THREE.WebGPURenderer({ forceWebGL: false })  // false = WebGPU+fallback
await renderer.init()
```

## КОД-СКЕЛЕТ — кастомний матеріал у TSL
```js
import { MeshBasicNodeMaterial } from 'three/webgpu'
import { Fn, vec3, vec4, uv, uniform, texture, sin, time, mix, mul, add } from 'three/tsl'

const uColorA = uniform(new THREE.Color('#c8794a'))
const uColorB = uniform(new THREE.Color('#1a1411'))
const uTex = texture(myTexture)

const material = new MeshBasicNodeMaterial()

// fragment-логіка як node-граф
material.colorNode = Fn(() => {
  const wave = sin(add(mul(uv().x, 10), time)).mul(0.5).add(0.5)  // 0..1 хвиля
  const base = uTex.sample(uv())
  const tint = mix(uColorA, uColorB, wave)
  return vec4(base.rgb.mul(tint), 1)
})()

// vertex-зсув
material.positionNode = Fn(() => {
  const p = positionLocal
  const offset = sin(add(p.y.mul(3), time)).mul(0.1)
  return vec3(p.x.add(offset), p.y, p.z)
})()
```

## COMPUTE (GPGPU на WebGPU — основа R07)
```js
import { Fn, instancedArray, instanceIndex, deltaTime } from 'three/tsl'
const positions = instancedArray(COUNT, 'vec3')
const update = Fn(() => {
  const p = positions.element(instanceIndex)
  p.addAssign(/* velocity */ vec3(0, deltaTime.mul(0.1), 0))
})()
renderer.computeAsync(update)  // у ticker
```

## ОБХІД ОБМЕЖЕНЬ WebGPU (з threejs.paris case study)
- **8 vertex buffers max** → bit-pack instance-дані у 4×vec4 (threejs.paris кодував avatar як 10-символьний base62 через mixed-radix).
- KTX2 (ETC1S/UASTC) текстур-атласи замість багатьох окремих текстур.
- iOS float-precision + Android shadow-баги — патчити/тестувати на реальних пристроях.

## ТОЧНІ ПАРАМЕТРИ
- `forceWebGL:false` (WebGPU+фолбек) / `true` (тест WebGL-гілки).
- `antialias` лише при pixelRatio<2.
- Node-материали: `MeshStandardNodeMaterial`, `MeshBasicNodeMaterial`, `SpriteNodeMaterial`.
- Post через TSL: `pass`, `mrt`, `bloom`, `output`, `emissive`.

## PERFORMANCE + FALLBACK
- `forceWebGL:false` = авто WebGL2 де немає WebGPU (Firefox не-Win, старі браузери, частина iOS).
- Тестувати ОБИДВА бекенди (поведінка шейдерів може різнитись).
- WebGPU: виграш на compute (R07), MRT, великих сценах; не критично для простих сцен.
- Якщо mobile-трафік >40% або LCP-бюджет <2.5s → лишитись на WebGL+R05 (не форсувати WebGPU).

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R15_tsl-dual-pipeline.md і R09.
Налаштуй TSL-шейдери (three/webgpu + three/tsl) з фолбеком:
- WebGPURenderer({forceWebGL:false}) + await init; R3F: gl={async init}
- приклад кастомного MeshBasicNodeMaterial (colorNode Fn з uv/time/mix, positionNode зсув)
- (опц.) compute через instancedArray + computeAsync
- тест обох бекендів (forceWebGL true/false); antialias лише pixelRatio<2
- KTX2-текстури; якщо mobile>40% → лишитись WebGL+R05
Поверни конфіг + 1 кастомний TSL-матеріал + перевірку WebGPU/WebGL.
```
