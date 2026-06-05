# R20 · Infinite Pan-Anywhere Canvas (Spatial Media Grid)
> Хвиля 2 (R3F). Реальний код: github.com/edoardolunardi/infinite-canvas (Codrops, R19 React + R3F + Three 0.182). Безкінечний 3D-простір медіа: drag-to-pan, zoom, нескінченне повторення.

## ЩО ЦЕ / КОЛИ
Нескінченна сітка зображень/відео у 3D-просторі, по якій можна літати (drag миша / touch, scroll-zoom, WASD). Chunk-based rendering + distance-culling тримають 120fps. Медіа повторюються безшовно.
**Коли:** портфоліо-«всесвіт», галерея проєктів як простір, креативні агенції, інтерактивний lookbook, «explore»-секція замість сітки карток.

## РІВЕНЬ РИЗИКУ: 🟡
Готове репо клонується; складність — інтеграція в Next.js + керування пам'яттю текстур.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **github.com/edoardolunardi/infinite-canvas** (Codrops 7 Jan 2026, MIT). Стек: React 19, Three 0.182, R3F 9.4, drei 10.7.
- Demo: tympanus.net/Tutorials/InfiniteCanvas/

## КЛЮЧОВІ ФІЧІ (з README)
- Infinite 3D space — нескінченна сітка медіа.
- Performance: **chunk-based rendering + distance-based culling**.
- Touch & mouse: drag-pan, pinch/scroll-zoom.
- Keyboard: WASD рух, QE вгору/вниз.
- Progressive loading: текстури вантажаться on-demand з прогресом.

## ПРИНЦИП (архітектура)
1. Простір розбитий на **chunks** (комірки сітки). Рендеряться лише chunks у радіусі від камери (culling).
2. При русі камери — chunks за межами радіуса вивантажуються, нові вантажаться (object pooling).
3. Медіа повторюється по модулю позиції (нескінченність = `position % gridSize`).
4. Текстури — lazy, з прогресом; placeholder поки вантажиться.
5. Інерція drag через damping (lerp до цільової позиції).

## КОД-СКЕЛЕТ (концепт, адаптувати з репо)
```jsx
import { Canvas, useFrame, useThree } from '@react-three/fiber'
import { useRef, useMemo, useState } from 'react'

const CHUNK = 4, RADIUS = 3, GAP = 3  // комірок у chunk, радіус видимості, відстань між медіа

function InfiniteGrid({ items }) {
  const group = useRef()
  const target = useRef({ x:0, y:0 })   // ціль drag
  const { camera } = useThree()

  // drag-pan з інерцією
  useFrame((_, delta) => {
    group.current.position.x += (target.current.x - group.current.position.x) * 0.08
    group.current.position.y += (target.current.y - group.current.position.y) * 0.08
    // culling: показуємо тільки tiles у радіусі від центру камери
  })

  // tiles навколо камери (chunk-based)
  const tiles = useMemo(() => {
    const t = []
    for (let x=-RADIUS; x<=RADIUS; x++)
      for (let y=-RADIUS; y<=RADIUS; y++) {
        const item = items[Math.abs((x*7+y*13)) % items.length]  // безшовне повторення
        t.push({ key:`${x}_${y}`, pos:[x*GAP, y*GAP, 0], item })
      }
    return t
  }, [items])

  return (
    <group ref={group}
      onPointerDown={e => /* старт drag */ null}
      onPointerMove={e => /* оновити target.current при drag */ null}>
      {tiles.map(t => <MediaTile key={t.key} position={t.pos} item={t.item} />)}
    </group>
  )
}

function MediaTile({ position, item }) {
  // lazy-текстура з placeholder; drei <Image> або useTexture
  return (
    <mesh position={position}>
      <planeGeometry args={[2.5, 2.5*9/16]} />
      <meshBasicMaterial /* map={lazyTexture} */ />
    </mesh>
  )
}

export default function InfiniteCanvas({ items }) {
  return (
    <Canvas camera={{ position:[0,0,8], fov:45 }} dpr={[1,2]}>
      <InfiniteGrid items={items} />
    </Canvas>
  )
}
```
> Для production — клонувати репо й адаптувати (там готові chunk-manager, culling, touch/pinch, прогрес-лоадер).

## NEXT.JS ІНТЕГРАЦІЯ
- `dynamic(() => import('./InfiniteCanvas'), { ssr:false })` — Canvas не рендериться на сервері.
- Текстури з `/public`, next/image для прелоадів.

## ТОЧНІ ПАРАМЕТРИ
- Радіус видимості 3-4 chunks (баланс «нескінченність» / пам'ять).
- Drag-інерція lerp 0.06-0.1.
- Zoom: камера z або fov, clamp min/max.
- DPR `[1,2]` (не вище — дорого).
- Текстури: WebP, on-demand, max ~30-50 одночасно у пам'яті.

## PERFORMANCE + FALLBACK
- Culling — ключ; ніколи не рендерити всю сітку.
- Object pooling tiles (не створювати/знищувати щокадру).
- Vivload текстур з прогресом; dispose поза радіусом.
- Mobile: менший радіус (2), DPR 1, менші текстури.
- reduced-motion / no-WebGL: статична CSS-сітка-галерея fallback.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R20_infinite-canvas.md.
Клонуй/адаптуй github.com/edoardolunardi/infinite-canvas під Next.js (R3F + drei):
- нескінченна 3D-сітка медіа, chunk-based rendering + distance culling
- drag-pan з інерцією (lerp 0.08), scroll/pinch-zoom, WASD
- безшовне повторення (position % grid), lazy-текстури з прогресом + dispose поза радіусом
- dynamic import ssr:false; DPR [1,2]; radius 3-4 chunks
- mobile → radius 2, DPR 1; no-WebGL → CSS-сітка fallback
Поверни компонент + демо (12 зображень портфоліо у нескінченному просторі).
```
