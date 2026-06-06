# R22 · Glass Refraction (MeshTransmissionMaterial / Fluid Glass)
> Хвиля 1 (легка). Реальний код: react-bits FluidGlass (github.com/DavidHDev/react-bits). Скляна лінза/куб/панель, що заломлює сцену під собою й слідує за курсором.

## ЩО ЦЕ / КОЛИ
3D-скляний об'єкт (лінза, куб, панель), крізь який видно решту сайту з фізичним заломленням (IOR, товщина, хроматична аберація). Класичний «fluid glass» cursor/nav-ефект award-сайтів. Дешевший за справжню fluid-симуляцію, але дає «дороге» відчуття преміального скла.
**Коли:** glass-навігація (bar-режим), cursor-лінза поверх контенту, hero зі скляним об'єктом, продуктові сцени (флакон/годинник зі склом).

## РІВЕНЬ РИЗИКУ: 🟢 (drop-in drei)
`MeshTransmissionMaterial` з drei — готовий. Claude Code робить за хвилини.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **react-bits FluidGlass** (github.com/DavidHDev/react-bits, `src/content/Components/FluidGlass/FluidGlass.jsx`) — реальний код нижче. Режими lens/bar/cube.
- **drei MeshTransmissionMaterial** (поверх MeshPhysicalMaterial) — IOR, thickness, chromaticAberration, anisotropy, distortion.
- Codrops explainer: Glass Torus / MeshTransmissionMaterial (13 Mar 2025).
- maath (pmndrs) — `easing.damp3` для плавного слідування.

## КЛЮЧОВИЙ ПРИНЦИП (з реального коду)
1. **FBO-буфер** захоплює всю сцену (контент під склом): `useFBO()`.
2. Сцена рендериться у буфер: `gl.setRenderTarget(buffer); gl.render(scene, camera)`.
3. Контент показується площиною з `meshBasicMaterial map={buffer.texture}`.
4. **Скляна геометрія** (з .glb: lens=Cylinder / cube=Cube) отримує `MeshTransmissionMaterial buffer={buffer.texture}` — заломлює захоплену сцену.
5. Скло слідує за курсором через `easing.damp3(ref.position, [pointer.x*w/2, pointer.y*h/2, 15], 0.15, delta)`.

## КОД-СКЕЛЕТ (реальний, адаптований з react-bits)
```jsx
import * as THREE from 'three'
import { useRef, useState, useEffect } from 'react'
import { Canvas, createPortal, useFrame, useThree } from '@react-three/fiber'
import { useFBO, useGLTF, MeshTransmissionMaterial } from '@react-three/drei'
import { easing } from 'maath'

function GlassLens({ glb = '/assets/3d/lens.glb', geometryKey = 'Cylinder',
                     ior = 1.15, thickness = 5, chromaticAberration = 0.1, anisotropy = 0.01 }) {
  const ref = useRef()
  const { nodes } = useGLTF(glb)
  const buffer = useFBO()                         // FBO для захоплення сцени
  const { viewport: vp } = useThree()
  const [scene] = useState(() => new THREE.Scene())

  useFrame((state, delta) => {
    const { gl, viewport, pointer, camera } = state
    const v = viewport.getCurrentViewport(camera, [0, 0, 15])
    // скло слідує за курсором (плавно)
    easing.damp3(ref.current.position,
      [(pointer.x * v.width) / 2, (pointer.y * v.height) / 2, 15], 0.15, delta)
    // рендеримо контент-сцену у буфер
    gl.setRenderTarget(buffer); gl.render(scene, camera); gl.setRenderTarget(null)
  })

  return (
    <>
      {createPortal(/* ВЕСЬ контент під склом */ <Content/>, scene)}
      {/* площина показує захоплену сцену */}
      <mesh scale={[vp.width, vp.height, 1]}>
        <planeGeometry />
        <meshBasicMaterial map={buffer.texture} transparent />
      </mesh>
      {/* скляна геометрія заломлює буфер */}
      <mesh ref={ref} scale={0.15} rotation-x={Math.PI/2} geometry={nodes[geometryKey].geometry}>
        <MeshTransmissionMaterial buffer={buffer.texture}
          ior={ior} thickness={thickness}
          anisotropy={anisotropy} chromaticAberration={chromaticAberration} />
      </mesh>
    </>
  )
}

export default function FluidGlass() {
  return (
    <Canvas camera={{ position:[0,0,20], fov:15 }} gl={{ alpha:true }}>
      <GlassLens />
    </Canvas>
  )
}
```

## ТОЧНІ ПАРАМЕТРИ (з реального коду + tuning)
- `ior` 1.15 (скло ~1.5, нижче = м'якше заломлення), `thickness` 5-10.
- `chromaticAberration` 0.1 (RGB-розщеплення на краях; 0.05-0.15).
- `anisotropy` 0.01, `roughness` 0 (чисте скло), `distortion` 0-0.5.
- Слідування: `easing.damp3 ... 0.15` (lag; менше = чіткіше).
- Камера fov 15, position z=20 (вузький fov → менше perspective-спотворення скла).
- Режими: **lens** (Cylinder, cursor-лінза), **cube** (Cube), **bar** (навігаційна панель, lockToBottom).
- Геометрія з .glb (lens.glb/cube.glb) — або проста drei-геометрія (sphereGeometry/torusGeometry) без моделі.

## PERFORMANCE + FALLBACK
- `MeshTransmissionMaterial` рендерить сцену у буфер щокадру (як дзеркало) — помірно дорого. `samples` нижче (4-6) на слабких GPU.
- `resolution` буфера менше на mobile (512 замість 1024).
- Тільки pointer:fine для cursor-режиму; на touch — статичне скло або вимкнути.
- reduced-motion: статичне скло без слідування.
- Не поєднувати з importkeHeavy post-processing одночасно.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R22_glass-refraction.md.
Зроби <FluidGlass/> (R3F + drei MeshTransmissionMaterial) за реальним кодом react-bits:
- FBO захоплює контент-сцену (createPortal), площина показує buffer.texture
- скляна геометрія (drei sphereGeometry або .glb lens/cube) з MeshTransmissionMaterial buffer=buffer.texture
- режими: lens (слідує за курсором через maath easing.damp3 0.15), bar (nav lockToBottom)
- params: ior 1.15, thickness 5, chromaticAberration 0.1, anisotropy 0.01, roughness 0
- камера fov 15 z=20; pointer:fine only; mobile/reduced-motion → статичне скло, samples↓ resolution↓
Поверни компонент + демо (cursor-лінза поверх тексту/зображень).
```
