# R07 · GPGPU Particles (сотні тисяч–мільйони частинок на GPU)
> 🔴 Найскладніший клас. Адаптуємо перевірені приклади (pmndrs, Wawa Sensei, Bruno). Частинки рахуються на GPU (position/velocity у текстурах або storage-буферах).

## ЩО ЦЕ / КОЛИ
Маси частинок (10k–1M+), чиї позиції оновлюються на GPU щокадру: морфінг у форми/текст/модель, реакція на курсор, потоки, галактики, «розпад на пил». Симуляція у compute (WebGPU/TSL) або ping-pong FBO (WebGL2).
**Коли:** hero-«вау» (морфінг логотипа/продукту з частинок), фон-галактика, розпад тексту (як Codrops Gommage), брендова «магія». Дорого — лише як головний акцент.

## РІВЕНЬ РИЗИКУ: 🔴
Математику симуляції НЕ пишемо з нуля. Адаптуємо перевірений приклад під свою форму/поведінку.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **WebGPU/TSL (сучасний шлях):** Wawa Sensei курс «GPGPU particles with TSL & WebGPU» (storage-буфери, `instancedArray`, compute-функції). Maxime Heckel «Field Guide to TSL and WebGPU». Three.js приклади `webgpu_compute_*`.
- **WebGL2 (фолбек):** класичний ping-pong FBO — позиція/швидкість у float-текстурах, два render-targets міняються щокадру. pmndrs приклади, Codrops GPGPU-туторіали.
- **Bruno folio-2025:** Trails.js, Confetti.js, Snow.js — instanced GPU-частинки (реальний код, github).
- **Розпад тексту:** Codrops «WebGPU Gommage Effect: Dissolving MSDF Text into Dust» (2026/01/28, TSL).

## КОД-СКЕЛЕТ (TSL/WebGPU — концепт)
```js
import * as THREE from 'three/webgpu'
import { Fn, instancedArray, instanceIndex, vec3, deltaTime, uniform } from 'three/tsl'

const COUNT = 500_000
const positions = instancedArray(COUNT, 'vec3')
const velocities = instancedArray(COUNT, 'vec3')
const uMouse = uniform(new THREE.Vector3())

// compute: оновлення позицій щокадру (на GPU)
const updateParticles = Fn(() => {
  const pos = positions.element(instanceIndex)
  const vel = velocities.element(instanceIndex)
  // притягання до курсора / морфінг до цільової форми / шум
  const toMouse = uMouse.sub(pos)
  vel.addAssign(toMouse.mul(0.001))
  vel.mulAssign(0.98)               // тертя
  pos.addAssign(vel.mul(deltaTime))
})()

renderer.computeAsync(updateParticles)  // у ticker щокадру

// рендер: instanced points/meshes читають positions
```

## КОД-СКЕЛЕТ (WebGL2 FBO ping-pong — концепт)
```
1. Дві float-текстури A,B (RGBA = xyz позиція + дані).
2. Simulation shader читає A → пише B (нова позиція = стара + швидкість + сила).
3. Swap A↔B щокадру.
4. Render shader: vertex читає позицію з текстури (gl_VertexID → uv), малює gl.POINTS.
```
Для R3F: drei + кастомний useFBO + два матеріали (sim + render). Адаптувати з pmndrs прикладу.

## ПАТЕРНИ ВИКОРИСТАННЯ
- **Морфінг у форму:** цільові позиції = семпл точок з моделі/тексту (MSDF/SDF); частинки lerp-ляться до них.
- **Курсор-репеленція/атракція:** сила від `uMouse`.
- **Розпад (Gommage):** від форми → розліт за шумом (Codrops TSL recipe).
- **Потік/галактика:** curl-noise велосіті.

## ТОЧНІ ПАРАМЕТРИ
- COUNT: 50k (безпечно desktop), 200k-500k (потужні GPU), 1M (демо/WebGPU). Mobile: 10-30k.
- Тертя velocity 0.95-0.99; сила курсора 0.0005-0.002.
- Розмір частинки 1-3px; additive blending для світіння.
- deltaTime-based (не залежить від FPS).

## PERFORMANCE + FALLBACK
- WebGPU compute набагато швидше за WebGL2 FBO для великих COUNT.
- Завжди WebGL2-фолбек (FBO) для браузерів без WebGPU.
- Mobile: різко менше частинок або статичний fallback (зображення/менша сцена).
- reduced-motion: статична форма без симуляції.
- Additive blending + малі частинки = дешевше за великі меші.
- Не поєднувати з fluid (R08) одночасно.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R07_gpgpu-particles.md.
Зроби GPGPU-частинки (R3F). НЕ пиши математику з нуля — адаптуй перевірений приклад:
- WebGPU/TSL шлях (three/webgpu, instancedArray, compute Fn) з WebGL2 FBO-фолбеком
- поведінка: морфінг 200k частинок у форму (семпл точок з тексту/моделі) + атракція до курсора
- COUNT адаптивний (desktop 200k / mobile 20k), тертя 0.98, deltaTime-based, additive blending
- reduced-motion → статична форма; WebGPU-детект з фолбеком
Орієнтир: Wawa Sensei GPGPU TSL / pmndrs FBO приклад / Bruno folio Trails.
Поверни компонент + демо (морфінг логотипа з частинок) + FPS-замір.
```
