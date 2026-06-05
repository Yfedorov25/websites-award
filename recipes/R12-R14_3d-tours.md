# R12-R14 · 3D-Тури / Віртуальні Прогулянки (всі типи, всі через Claude Code)
> Твоя вимога: всі варіанти 3D-прогулянки по ЖК/простору, різні рівні складності, усі реалізовувані через Claude (+ зовнішні сервіси Kling/Luma). Меню для вибору під проєкт.

## ОГЛЯД: 6 ТИПІВ 3D-ТУРУ (від простого до складного)
| # | Тип | Складність | Реалізм | Що треба | Recipe |
|---|---|---|---|---|---|
| 1 | Frame-sequence flythrough | 🟢 низька | висока (рендер) | Kling-відео польоту | R01 |
| 2 | 3D-карта району | 🟡 середня | стилізована | модель кварталу | R10 |
| 3 | 360° панорами | 🟢 низька | фото-реалізм | панорамні фото | **R13** |
| 4 | Gaussian Splatting | 🟡 середня | ФОТО-реалізм | відео/фото обходу | **R12** |
| 5 | Real-time walkthrough | 🔴 висока | модельна | 3D-модель ЖК | **R14** |
| 6 | Matterport-style dollhouse | 🟡 середня | фото | 360-зйомка + план | R13+ |

> Часто комбінують: R10 (район) + R01 (флайт будівлі) + R13/R12 (інтер'єри квартир).

---

## R12 · GAUSSIAN SPLATTING (фотореалістичний тур з фото/відео)
### Що це / коли
Найреалістичніший тур: знімаєш простір на телефон (відео обходу) → Luma AI генерує 3D Gaussian Splat → рендериш у браузері через Spark. Виглядає як справжнє фото, але можна літати камерою в 3D. Ідеально для готових квартир/інтер'єрів/локацій.

### Рівень ризику: 🟡 (рендер готовий, складність — у захопленні/оптимізації)

### Перевірене джерело
- **Spark** (`@sparkjsdev/spark`, MIT, World Labs) — рендерер 3DGS для Three.js. 98%+ WebGL2, працює на mobile, формати .PLY/.SPZ/.SPLAT/.KSPLAT/.SOG, splat+mesh разом, LoD (Spark 2.0), real-time edit.
- **Luma AI** — безкоштовне захоплення (відео/фото з телефона → splat за 15-30 хв, експорт .ply).
- Альтернативи захоплення: Polycam, Postshot. Рендер: GaussianSplats3D (mkkellogg), luma-web (@lumaai/luma-web).

### Код-скелет (Spark + Three.js)
```js
import { SplatMesh } from '@sparkjsdev/spark'
import * as THREE from 'three'

const splat = new SplatMesh({ url: '/tours/apartment.spz' })  // з Luma-захоплення
scene.add(splat)
// камера літає (GSAP path або OrbitControls обмежений) — як R10 flyTo
```
R3F: обгорнути SplatMesh у `<primitive>`, камера-тур по waypoints (GSAP).

### Параметри/перформанс
- Формат: .SPZ або .SOG (стиснені) замість .PLY (важкий — може бути 100MB+).
- LoD (Spark 2.0) для великих сцен.
- Mobile: працює, але стрімінг — слідкуй за розміром.
- Fallback: статичні фото-рендери, якщо WebGL2 нема.

### Промпт для Claude Code
```
Прочитай recipes/R12-R14_3d-tours.md (R12).
Зроби фото-реалістичний тур через @sparkjsdev/spark (R3F):
- SplatMesh з /tours/*.spz (захоплення Luma)
- камера-тур по waypoints (GSAP path) + обмежений OrbitControls
- стиснений формат SOG/SPZ, LoD, mobile fallback (фото)
Поверни компонент + інструкцію як зняти простір на телефон → Luma → .spz.
```

---

## R13 · 360° ПАНОРАМНІ ТУРИ (photo-based, найдешевше)
### Що це / коли
Панорамні фото (360°) з hotspot-навігацією між кімнатами. Найдешевший фотореалістичний тур — потрібні лише 360-фото (телефон з панорамою / Insta360). Класика нерухомості.

### Рівень ризику: 🟢

### Перевірене джерело
- **Pannellum** (mpetroff/pannellum, MIT, ~21KB) — найлегший, hotspots, multi-scene.
- **Marzipano** (Google, ~55KB) — tour-builder, потужніший.
- **Photo Sphere Viewer** (TS rewrite 2025) — найбагатша marker-система, React-wrapper, Map plugin (floor-plan).
- **Panolens.js** — на Three.js.

### Код-скелет (Pannellum)
```js
pannellum.viewer('tour', {
  default: { firstScene: 'living', sceneFadeDuration: 1000 },
  scenes: {
    living: {
      type: 'equirectangular', panorama: '/360/living.webp',
      hotSpots: [{ pitch:-2, yaw:130, type:'scene', text:'Kitchen', sceneId:'kitchen' }]
    },
    kitchen: { type:'equirectangular', panorama:'/360/kitchen.webp', hotSpots:[...] }
  }
})
```

### Параметри/перформанс
- Фото: equirectangular WebP, 4096-8192px ширина.
- Hotspots: перехід між сценами (scene) + інфо-точки (info).
- Photo Sphere Viewer Map plugin → floor-plan з позицією (Matterport-відчуття = R13+).
- Дуже легкий, працює всюди.

### Промпт для Claude Code
```
Прочитай recipes/R12-R14_3d-tours.md (R13).
Зроби 360°-тур через Pannellum (або Photo Sphere Viewer для React):
- multi-scene з /360/*.webp, hotspots-переходи між кімнатами + info-точки
- (опц.) floor-plan map plugin з позицією
- sceneFadeDuration 1000, mobile-friendly
Поверни компонент + структуру даних сцен/hotspots.
```

---

## R14 · REAL-TIME WALKTHROUGH (інтерактивна 3D-модель ЖК)
### Що це / коли
Вільне переміщення камерою по 3D-моделі ЖК (від першої особи або орбіта). Потрібна 3D-модель (від забудовника / Blender / арх-віз). Найскладніше, але дає повний контроль і інтерактив (клік на квартиру → інфо).

### Рівень ризику: 🔴

### Перевірене джерело
- R3F + drei: `useGLTF`, `OrbitControls`/`PointerLockControls`, `Bounds`.
- Draco/KTX2 компресія моделі (як Active Theory/Bruno).
- camera-controls (yomotsu) — кінематографічна камера (як Bruno).

### Код-скелет (R3F)
```jsx
import { useGLTF, OrbitControls, Bounds, Html } from '@react-three/drei'
function Walkthrough() {
  const { scene } = useGLTF('/models/complex.glb')  // Draco-стиснена
  return (
    <Canvas camera={{ position:[20,15,20], fov:45 }}>
      <Environment preset="city"/>
      <primitive object={scene}/>
      {/* клікабельні квартири з інфо */}
      <Html position={[5,10,2]}><div className="unit-tag">Flat 12 · 2BR</div></Html>
      <OrbitControls enableDamping minDistance={8} maxDistance={50}
        maxPolarAngle={Math.PI/2.2}/>
    </Canvas>
  )
}
```
First-person: `PointerLockControls` + WASD (як гра).

### Параметри/перформанс
- Модель: Draco+KTX2, LoD, instancing для повторюваних елементів.
- Камера: OrbitControls (огляд) або PointerLock (від першої особи).
- Клік на юніт → raycaster → інфо-панель (як R10 POI).
- Mobile: спрощена модель або фолбек на R01/R13.

### Промпт для Claude Code
```
Прочитай recipes/R12-R14_3d-tours.md (R14).
Зроби real-time walkthrough ЖК (R3F):
- useGLTF Draco-модель /models/complex.glb + Environment
- OrbitControls (damping, обмежений polar, min/max 8/50) АБО PointerLock+WASD
- клікабельні квартири (raycaster → Html інфо-панель: поверх, к-сть кімнат, ціна)
- Draco/KTX2, LoD, instancing; mobile → спрощена модель або фолбек R01/R13
Поверни компонент + як підготувати/стиснути модель (gltf-transform).
```

---

## ЯК ОБИРАТИ ТИП ТУРУ (рішення в pipeline)
- **Є тільки рендери/картинки** → R01 (Kling-флайт) + R10 (карта району).
- **Є готовий простір для зйомки** → R13 (360, дешево) або R12 (Splatting, фотореалізм).
- **Є 3D-модель від забудовника** → R14 (walkthrough) + R10 (район).
- **Максимальний «вау» + бюджет** → R12 (Splatting інтер'єри) + R14 (модель) + R10 (район) + R01 (cinematic флайт фасаду).
- **Mobile-перформанс критичний** → R13 (найлегше) або R01 (frame-seq).
