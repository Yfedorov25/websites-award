# R10 · Interactive 3D District Map (Real Estate Killer Feature)
> Декодовано з ERA.estate /3d-map (Triple SOTD). Найсильніший «$10k+» елемент для нерухомості/ЖК.
> Продає не квартиру, а ЖИТТЯ В ЛОКАЦІЇ через кінематографічну 3D-подорож кварталом.

## ЩО ЦЕ / КОЛИ
Власна 3D-сцена (Three.js/R3F): стилізована модель району, ЖК підсвічений у центрі, обертова камера, клікабельні POI-маркери (театр, школа, парк, метро...). Клік на маркер → багата модалка: фото + час їзди + наратив. Компас показує орієнтацію. **Це НЕ Google Maps embed.**
**Коли:** нерухомість, ЖК, готелі, курорти, кампуси, будь-що, де «локація = цінність».

## РІВЕНЬ РИЗИКУ: 🟡 (з 3D-моделлю) / 🟢 (зі стилізованою площиною)
Дві версії складності:
- **Lite (🟢):** стилізована 3D-площина (texture карти) + 3D-маркери + raycaster. Claude Code робить сам.
- **Full (🟡):** low-poly 3D-модель кварталу (будівлі) + підсвічений ЖК. Потрібна модель (Blender/готова від забудовника/Gaussian Splatting).

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- ERA.estate /3d-map (еталон структури — наш teardown).
- drei: `<Html>` (HTML-overlay на 3D), `OrbitControls`, `Bounds`, `useGLTF`, `Instances`.
- raycaster для кліку по маркерах (як Bruno RayCursor.js).

## АРХІТЕКТУРА ДАНИХ (data-driven, як в ERA)
```js
// data/poi.js — точки інтересу (з реальних даних / Apify-скрапу)
export const POI = [
  { id:0, position:[12,0,-8], time:7, unit:'min ride', address:'Soborna str., 6',
    title:'Central Theater', image:'/poi/theater.webp',
    text:'Immerse in the world of art where drama meets performance.' },
  { id:1, position:[-15,0,5], time:5, unit:'min walk', address:'Park alley',
    title:'City Park', image:'/poi/park.webp', text:'...' },
  // ...
];
export const COMPLEX = { position:[0,0,0], name:'Your ЖК' }; // підсвічений центр
```

## КОД-СКЕЛЕТ (R3F + drei)
```jsx
// DistrictMap.jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, Html, useGLTF, Environment } from '@react-three/drei'
import { useState, useRef } from 'react'
import { POI, COMPLEX } from '@/data/poi'

function Marker({ poi, onClick, active }) {
  return (
    <group position={poi.position}>
      <mesh onClick={(e)=>{e.stopPropagation(); onClick(poi)}}
            onPointerOver={()=>document.body.style.cursor='pointer'}
            onPointerOut={()=>document.body.style.cursor='auto'}>
        <sphereGeometry args={[0.4,16,16]} />
        <meshStandardMaterial color={active?'#e0a96d':'#fff'} emissive={active?'#e0a96d':'#000'} emissiveIntensity={active?2:0}/>
      </mesh>
      {/* пульсуюче кільце */}
      <mesh rotation={[-Math.PI/2,0,0]}>
        <ringGeometry args={[0.5,0.7,32]} />
        <meshBasicMaterial color="#e0a96d" transparent opacity={0.5}/>
      </mesh>
      {/* час їзди — завжди видно як HTML-label */}
      <Html center distanceFactor={30} position={[0,1.2,0]}>
        <div className="poi-label">{poi.time} {poi.unit}</div>
      </Html>
    </group>
  )
}

function District() {
  // Full-режим: const { scene } = useGLTF('/models/district.glb')
  // Lite-режим: площина з текстурою карти
  return (
    <group>
      {/* Lite: <mesh rotation={[-Math.PI/2,0,0]}><planeGeometry args={[60,60]}/>
                <meshStandardMaterial map={mapTexture}/></mesh> */}
      {/* Full: <primitive object={scene} /> */}
      {/* ЖК у центрі — підсвічений */}
      <mesh position={COMPLEX.position}>
        <boxGeometry args={[3,8,3]} />
        <meshStandardMaterial color="#c8794a" emissive="#9a4f2b" emissiveIntensity={0.4}/>
      </mesh>
    </group>
  )
}

export function DistrictMap() {
  const [active, setActive] = useState(null)
  const controls = useRef()
  return (
    <div className="district-map">
      <Canvas camera={{ position:[0,28,28], fov:42 }} shadows>
        <ambientLight intensity={0.6}/>
        <directionalLight position={[10,20,10]} intensity={1.2} castShadow/>
        <Environment preset="city"/>
        <District/>
        {POI.map(p=> <Marker key={p.id} poi={p} active={active?.id===p.id}
                             onClick={(poi)=>{ setActive(poi); flyTo(controls,poi) }}/>)}
        <OrbitControls ref={controls} enablePan={false}
          minPolarAngle={Math.PI/6} maxPolarAngle={Math.PI/2.4}
          minDistance={18} maxDistance={45} enableDamping dampingFactor={0.08}
          onChange={()=>updateCompass(controls)} />
      </Canvas>

      {/* HTML-overlay модалка (як ERA page-3d-map-modals) */}
      {active && (
        <div className="poi-modal" onClick={()=>setActive(null)}>
          <div className="poi-modal__card" onClick={e=>e.stopPropagation()}>
            <button className="poi-modal__close" onClick={()=>setActive(null)}>×</button>
            <div className="poi-modal__time"><b>{active.time}</b> {active.unit}</div>
            <div className="poi-modal__addr">{active.address}</div>
            <h3 className="poi-modal__title">{active.title}</h3>
            <img src={active.image} alt={active.title}/>
            <p>{active.text}</p>
          </div>
        </div>
      )}

      {/* Компас (стрілка обертається з камерою) */}
      <div className="compass"><span>N</span><div className="compass__arrow" id="compass-arrow"/></div>

      {/* Зворотний preloader 100→0 (ERA-style) — окремий компонент */}
    </div>
  )
}

function flyTo(controls, poi){ /* GSAP tween camera.position + target до poi.position */ }
function updateCompass(controls){ /* compass-arrow rotate = -azimuthalAngle камери */ }
```

## ТОЧНІ ПАРАМЕТРИ (з ERA + best practice)
- Камера: ізометрична-ish, fov 40-45, нахил `maxPolarAngle ~Math.PI/2.4` (не дає «лягти» на землю).
- OrbitControls: `enablePan=false` (тільки обертання+зум), damping 0.08, minDistance 18 / maxDistance 45.
- POI-маркери: пульсуюче emissive-кільце + час їзди як завжди-видимий label (distanceFactor).
- flyTo: GSAP tween камери до POI при кліку (1.2s power3.out) — кінематографічно.
- Модалка: фото 720×900→1282×1602 (як ERA), dim-фон, наратив (не суха інфа).
- Preloader: зворотний 100→0 роллер (translateY стопки цифр).
- POI з реальними даними: час їзди + адреса + фото + 1-2 речення емоційного опису.

## PERFORMANCE + FALLBACK
- Full-модель: Draco/KTX2 компресія glTF (як Active Theory), LOD для далеких будівель.
- Lite-режим — дефолт для мобільних (площина+маркери замість важкої моделі).
- reduced-motion / mobile low-end: статична 2D-карта-зображення з клікабельними HTML-маркерами (без WebGL), як ERA робить `<picture>` fallback.
- Lazy-load моделі; preloader поки вантажиться.
- POI-фото — next/image WebP, lazy.

## АЛЬТЕРНАТИВНІ РІВНІ 3D-ТУРУ (меню для бази)
Для «прогулянки по ЖК» окрім R10 (карта району):
1. **Frame-sequence flythrough** (R01) — Kling-рендер польоту крізь ЖК → scroll-scrubbed. Найпростіше.
2. **Gaussian Splatting** (R12, окремий recipe) — фотореалістичний тур з фото/відео через Luma + Spark. Найреалістичніше.
3. **360° панорами** (R13) — Pannellum/Marzipano, photo-based тури по квартирах. Найдешевше.
4. **Real-time R3F walkthrough** (R14) — інтерактивна модель з вільною камерою. Найскладніше.
> Для одного ЖК часто комбінують: R10 (район) + R01 (флайт будівлі) + R13 (360 квартир).

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай teardowns/ERA_real-estate.md і recipes/R10_3d-district-map.md.
Побудуй <DistrictMap/> (Next.js + R3F + drei) за скелетом R10:
- дані POI з /data/poi.js (id, position, time, unit, address, title, image, text)
- Lite-режим: стилізована площина-карта + підсвічений ЖК-бокс у центрі
- POI-маркери: пульсуюче emissive-кільце + час їзди як Html-label
- клік на маркер → GSAP flyTo камери + HTML-модалка (фото+час+адреса+наратив, dim-фон)
- OrbitControls: enablePan=false, damping 0.08, обмежений polar angle, min/maxDistance 18/45
- компас (стрілка = -azimuthalAngle камери)
- зворотний preloader 100→0 (роллер цифр)
- mobile/reduced-motion fallback: статична 2D-карта з HTML-маркерами (без WebGL)
- next/image для POI-фото; ціль 60fps desktop
Поверни файли + демо з 5 тестовими POI + як перевірити через Playwright (клік на маркер → модалка).
```
