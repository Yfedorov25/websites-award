# R09 · WebGPU / TSL Migration (Future-Proof Upgrade Path)
> Не окрема фіча — стратегія апгрейду рендера. Bruno folio-2025 уже на цьому. Тренд 2026 (research-звіт).

## ЩО ЦЕ / КОЛИ
Перехід з WebGLRenderer на WebGPURenderer + TSL (Three.js Shading Language — пишеш шейдери як JS-ноди, компілюються у WGSL для WebGPU або GLSL для WebGL2-фолбеку). Дає compute-шейдери (GPGPU R07), near-native performance, єдиний код під обидва бекенди.
**Коли:** новий проєкт у 2026 з прицілом на майбутнє; коли потрібен GPGPU/fluid на WebGPU; коли аудиторія переважно Chrome/Edge 113+/Safari 26+.

## РІВЕНЬ РИЗИКУ: 🟡
Двома рядками міняється renderer; складність — у міграції кастомних GLSL-шейдерів у TSL (але TSL фолбекає на WebGL2 сам).

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **Bruno folio-2025** (реальний код): `import * as THREE from 'three/webgpu'`, `import {pass, mrt, bloom} from 'three/tsl'`, `new THREE.WebGPURenderer({ forceWebGL: false })`, bloom через TSL BloomNode, кастомний cheapDOF.
- Three.js docs WebGPU; ICS MEDIA «Getting started with Three.js on WebGPU».
- Maxime Heckel «Field Guide to TSL and WebGPU».

## КЛЮЧОВА ЗМІНА (з коду Bruno)
```js
// WebGL (старе):
import * as THREE from 'three'
const renderer = new THREE.WebGLRenderer({ antialias:true })

// WebGPU (нове, Bruno folio):
import * as THREE from 'three/webgpu'
import { pass, mrt, output, emissive, bloom } from 'three/tsl'
const renderer = new THREE.WebGPURenderer({
  canvas, powerPreference:'high-performance',
  forceWebGL: false,            // false = WebGPU з авто-фолбеком на WebGL2
  antialias: pixelRatio < 2,
})
```
Post-processing на TSL (Bruno):
```js
const scenePass = pass(scene, camera)
scenePass.setMRT(mrt({ output, emissive }))
const bloomPass = bloom(scenePass.getTextureNode('emissive'))
renderer.outputNode = scenePass.add(bloomPass)  // node-based composition
```

## TSL ОСНОВИ
- Шейдери = JS-ноди: `Fn(() => {...})`, `vec3()`, `uniform()`, `texture()`, `mix()`, `instanceIndex`.
- Один код → WGSL (WebGPU) або GLSL (WebGL2) автоматично.
- Compute: `instancedArray`, `renderer.computeAsync()` (база для R07 GPGPU).
- drei/R3F: `<Canvas>` з `gl={async (props)=> { const r = new THREE.WebGPURenderer(props); await r.init(); return r }}` (R3F підтримує WebGPU renderer).

## СТРАТЕГІЯ МІГРАЦІЇ
1. **Новий проєкт:** одразу `three/webgpu` + `forceWebGL:false` (фолбек безкоштовний).
2. **Кастомні шейдери:** прості — переписати в TSL; складні GLSL — лишити (TSL сумісний з GLSL-матеріалами через WebGL-бекенд) або поступово.
3. **Post-processing:** TSL nodes (bloom/DOF) замість EffectComposer на WebGPU.
4. **GPGPU (R07):** тільки на WebGPU compute — головна причина мігрувати.

## PERFORMANCE + FALLBACK
- `forceWebGL:false` = авто-фолбек на WebGL2 де немає WebGPU (Firefox не-Windows, старі браузери).
- Завжди тестувати обидва бекенди.
- WebGPU: краще для compute (частинки, fluid), MRT, великих сцен.
- `antialias` лише при pixelRatio<2 (на retina не треба — дорого).

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R09_webgpu-tsl-migration.md і 00_foundations/recipe-methodology.md (стек Bruno).
Налаштуй R3F-проєкт на WebGPU з фолбеком:
- <Canvas gl={async renderer init}> з THREE.WebGPURenderer({forceWebGL:false})
- post-processing через TSL nodes (bloom на emissive MRT, як Bruno folio)
- перевір фолбек на WebGL2 (forceWebGL:true тест)
- antialias лише pixelRatio<2
Поверни конфіг + перевірку обох бекендів (Chrome WebGPU + Firefox WebGL2 fallback).
```
