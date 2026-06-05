# R03 · Persistent WebGL + Page Transitions + DOM-Tracked Scenes
> Як Immersive Garden (один canvas на весь сайт) та ERA (Barba persistent). Сучасний React-стек: @14islands/r3f-scroll-rig (перевірене, 937★, від студії 14islands).
> Це фундамент award-сайту: WebGL не перемонтовується між сторінками → безшовні переходи + DOM і 3D ідеально синхронізовані.

## ЩО ЦЕ / КОЛИ
Один глобальний `<Canvas>` живе постійно (поза роутером). DOM-елементи лишаються звичайним HTML/CSS, а WebGL «малює» 3D-об'єкти точно на їх місці (scale+position), відстежуючи «проксі»-елементи. Переходи між сторінками не перезавантажують сцену.
**Коли:** будь-який award-сайт з WebGL + кілька сторінок; коли треба змішати DOM-контент і 3D без розривів; коли потрібні безшовні page transitions.

## РІВЕНЬ РИЗИКУ: 🟡
Claude Code пише сам за патерном r3f-scroll-rig; ми контролюємо: матчинг DOM↔WebGL scale, cleanup при unmount, кількість WebGL-контекстів (має бути ОДИН).

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **@14islands/r3f-scroll-rig** (github.com/14islands/r3f-scroll-rig) — `<GlobalCanvas>`, `<SmoothScrollbar>` (на Lenis), `<UseCanvas>`, `<ScrollScene>`, `useTracker()`. Framework-agnostic (Next.js/Gatsby/CRA), 100% сумісне з drei/react-spring.
- Підтверджено на: Immersive Garden (один persistent canvas на Nuxt), ERA (Barba persistent container).
- Codrops туторіал: «Progressively Enhanced WebGL Lens Refraction».
- Для не-R3F (vanilla): Barba.js + persistent Three.js scene (Codrops «Seamless 3D Transitions with GSAP & Three.js»).

## ЧОМУ САМЕ ТАК (ключовий принцип)
> «Mixing WebGL with scrolling HTML is hard. One way is multiple canvases, but there is a browser limit to how many WebGL contexts can be active, and resources can't be shared between contexts.» — r3f-scroll-rig
Тому: **ОДИН `<GlobalCanvas>` на весь сайт**, що лишається між переходами. Компоненти «надсилають» свій 3D-контент у нього через `useCanvas()`/`<UseCanvas>` поки змонтовані.

## КОД-СКЕЛЕТ (Next.js + r3f-scroll-rig)
```jsx
// app/layout.jsx (або _app.jsx) — ГЛОБАЛЬНО, поза сторінками
import { GlobalCanvas, SmoothScrollbar } from '@14islands/r3f-scroll-rig'

export default function RootLayout({ children }) {
  return (
    <html><body>
      <GlobalCanvas
        // дефолтна камера 50° FoV; для контролю:
        camera={{ fov: 50 }}
        // глобальне post-processing chain можна тут
      />
      <SmoothScrollbar />   {/* Lenis smooth scroll на main thread */}
      {children}            {/* DOM-сторінки; canvas НЕ перемонтовується при навігації */}
    </body></html>
  )
}
```
```jsx
// будь-яка секція: DOM-елемент, на місці якого малюється 3D
import { UseCanvas, ScrollScene } from '@14islands/r3f-scroll-rig'
import { useRef } from 'react'

function HeroImage() {
  const el = useRef()
  return (
    <>
      {/* звичайний DOM — розмітка/доступність/SEO */}
      <div ref={el} className="hero-visual" style={{ width:'100%', aspectRatio:'16/9' }} />

      {/* тунелюємо 3D у GlobalCanvas, поки цей компонент змонтований */}
      <UseCanvas>
        <ScrollScene track={el}>
          {(props) => (                 // props.scale матчить DOM-елемент!
            <mesh {...props}>
              <planeGeometry />
              <meshBasicMaterial color="turquoise" />
              {/* або shader/відео-текстура/glb — рендериться точно поверх DOM-проксі */}
            </mesh>
          )}
        </ScrollScene>
      </UseCanvas>
    </>
  )
}
```
```jsx
// ViewportScrollScene — окрема «сцена у вікні» з власною камерою/світлом/контролами (для 3D-моделей)
import { ViewportScrollScene } from '@14islands/r3f-scroll-rig'
<UseCanvas>
  <ViewportScrollScene track={el}>
    {(props) => <Model {...props} />}  {/* власна камера, env, controls */}
  </ViewportScrollScene>
</UseCanvas>
```

## PAGE TRANSITIONS поверх persistent canvas
- Оскільки `<GlobalCanvas>` поза роутером — він НЕ зникає при навігації. DOM-сторінки міняються (Next router / Barba для vanilla), а WebGL-сцена морфить.
- Патерн переходу: на route change → GSAP timeline (fade/wipe DOM) + tween стану спільної сцени (камера, активний об'єкт). Нічого не dispose між сторінками.
- Vanilla-альтернатива: Barba.js `data-barba="container"` + один Three.js renderer (як ERA).

## ТОЧНІ ПАРАМЕТРИ
- GlobalCanvas камера: FoV 50° (дефолт; distance рахується авто під висоту екрана). Override `camera={{position:[0,0,10]}}` або `{fov:20}`.
- Завжди базуй розмір меша на `props.scale` від ScrollScene (матч DOM на всіх екранах).
- SmoothScrollbar = Lenis під капотом → lerp ~0.1 (див. R04).
- Один canvas. Ніколи не монтуй другий `<Canvas>` паралельно.
- Responsive-текстури з DOM: `useImageAsTexture` (підтримує `<picture>`, srcset, lazy).

## PERFORMANCE + FALLBACK
- Progressive enhancement: сайт працює як звичайний DOM навіть якщо WebGL вимкнено/не вантажиться — 3D накладається зверху. Це найбезпечніший підхід (StudioMeyer: 3D не має ламати базу).
- `getBoundingClientRect()` лише раз на mount, далі IntersectionObserver/ResizeObserver — дешево.
- reduced-motion: рендерити DOM без canvas-ефектів (умовний `<UseCanvas>`).
- Один контекст = немає проблеми ліміту WebGL-контекстів браузера.
- glb: Draco/KTX2 компресія.

## МАПІНГ ЕТАЛОНІВ
| Сайт | Як робить persistent WebGL |
|---|---|
| Immersive Garden | Nuxt: `<div class="webglApp"><canvas>` поза `.page`; media → спільна сцена |
| ERA | Barba `data-barba="container"` + Locomotive; WebGL splash/3d-map контейнери |
| Наш стек | r3f-scroll-rig `<GlobalCanvas>` + `<UseCanvas>`/`<ScrollScene>` |

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R03_persistent-webgl-transitions.md і teardowns/ImmersiveGarden_luxury-agency.md.
Налаштуй persistent WebGL для Next.js через @14islands/r3f-scroll-rig:
- <GlobalCanvas> + <SmoothScrollbar> у root layout (поза роутером, не перемонтовується)
- приклад секції: DOM-проксі <div ref> + <UseCanvas><ScrollScene track={ref}> з мешем, що матчить scale
- один ViewportScrollScene з glb-моделлю (власна камера/світло)
- page transition на route change: GSAP fade DOM + tween камери спільної сцени (без dispose)
- progressive enhancement: працює без WebGL; reduced-motion → без canvas
- один WebGL-контекст; responsive-текстури через useImageAsTexture
Поверни файли + перевірку: навігація між 2 сторінками без перезавантаження сцени (Playwright).
```
