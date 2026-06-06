# R08 · Fluid Simulation (Cursor-Reactive WebGL Fluid)
> 🔴 НЕ пишемо solver з нуля. Адаптуємо перевірений код (Pavel Dobryakov Navier-Stokes). Для R3F є готова обгортка з повним API.

## ЩО ЦЕ / КОЛИ
Рідина, що реагує на курсор/рух — кольорові розводи, що течуть і змішуються (Navier-Stokes на GPU, ping-pong FBO). Преміальний інтерактивний фон / hero / cursor-ефект.
**Коли:** арт-бренди, агенції, hero-фон з «магією», cursor-distortion поверх сцени. Обережно з нішами (нерухомість/корпоратив — рідко доречно).

## РІВЕНЬ РИЗИКУ: 🔴 → стає 🟢 завдяки готовій обгортці
Математику solver'а (advection, divergence, pressure, curl) ми НЕ пишемо. Беремо перевірену обгортку й конфігуруємо параметрами.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **@whatisjery/react-fluid-distortion** (github.com/whatisjery/react-fluid-distortion) — post-processing fluid для R3F, на шейдерах Pavel Dobryakov. Готовий `<Fluid>` ефект у EffectComposer.
- **PavelDoGreat/WebGL-Fluid-Simulation** — оригінал (vanilla, для не-R3F).
- **x8BitRain/react-webgl-fluid** — порт Pavel як React-компонент.
- WebGPU-версія: three-fluid-fx (TSL/WGSL compute) для R09.

## КОД-СКЕЛЕТ (R3F — готова обгортка)
```bash
npm install @whatisjery/react-fluid-distortion @react-three/drei @react-three/postprocessing postprocessing
```
```jsx
import { Fluid } from '@whatisjery/react-fluid-distortion'
import { EffectComposer } from '@react-three/postprocessing'
import { Canvas } from '@react-three/fiber'

function FluidBackground() {
  return (
    <Canvas style={{ position:'fixed', inset:0, width:'100vw', height:'100vh' }}>
      {/* інша сцена за бажанням */}
      <EffectComposer>
        <Fluid
          fluidColor="#c8794a"        // бурштин (наш бренд); rainbow=false щоб діяв
          rainbow={false}
          intensity={8}               // 0-10
          force={2}                   // множник швидкості миші 0-20
          distortion={1.4}            // 0-2
          radius={0.3}                // 0.01-1
          curl={10}                   // завихрення 0-50
          swirl={18}                  // 0-20
          velocityDissipation={0.98}  // як швидко згасає рух
          densityDissipation={0.95}   // як швидко зникає колір
          pressure={0.8}
          showBackground={false}      // прозорий фон (накладається на контент)
        />
      </EffectComposer>
    </Canvas>
  )
}
```

## ТОЧНІ ПАРАМЕТРИ (пресети)
**Тонкий брендовий cursor-trail (преміум):**
- intensity 6-8, force 1.5-2, distortion 1-1.4, rainbow false, fluidColor = бренд, showBackground false.
- velocityDissipation 0.98, densityDissipation 0.94 (швидко зникає → не захаращує).

**Соковитий арт-фон:**
- intensity 10, force 4-6, curl 20-30, swirl 20, rainbow true, showBackground true.

## PERFORMANCE + FALLBACK
- Fluid дорогий (ping-pong FBO щокадру). На mobile — вимкнути або знизити sim resolution.
- Тільки pointer:fine (на touch немає курсора для splat).
- reduced-motion: вимкнути.
- Не поверх важкої 3D-сцени одночасно (подвійне GPU-навантаження) — або фон, або сцена.
- Adaptive: детект FPS, знизити dissipation/resolution.

## VANILLA-АЛЬТЕРНАТИВА (не-R3F)
PavelDoGreat/WebGL-Fluid-Simulation — один JS-файл, dat.gui-конфіг. Вбудувати як фоновий canvas. Для проєктів без React.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R08_fluid-simulation.md.
Додай cursor-reactive fluid через @whatisjery/react-fluid-distortion (НЕ пиши solver):
- <Fluid> у EffectComposer, пресет "тонкий брендовий": intensity 8, force 2, distortion 1.4,
  rainbow false, fluidColor=бренд, showBackground false, velocityDissipation 0.98
- тільки pointer:fine; mobile/reduced-motion → вимкнути
- не поверх важкої 3D-сцени; adaptive FPS
Поверни компонент + демо-фон. Якщо не-React — використай PavelDoGreat vanilla як фоновий canvas.
```
