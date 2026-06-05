# ScrollCraft PRO · Toolbox & Arsenal
> Усе, що встановлюємо в Claude Code, усі open-source ресурси, студії та техніки.
> Перевірено резьорчем (станом на 2026). Це наш "магазин зброї".

---

## 1. CLAUDE CODE: ПЛАГІНИ, СКІЛИ, АГЕНТИ

### 1.1 Головний 3D/анімаційний маркетплейс — `freshtechbro/claudedesignskills`
Найважливіша знахідка: повна колекція скілів саме під наш стек.

```bash
# Додати маркетплейс
/plugin marketplace add freshtechbro/claudedesignskills

# Ключові бандли (рекомендований мінімум для ScrollCraft PRO)
/plugin install core-3d-animation     # Three.js, GSAP, R3F, Motion, Babylon (5 skills, 6 agents)
/plugin install extended-3d-scroll     # A-Frame, Vanta, PlayCanvas, PixiJS, Locomotive, Barba (6 skills, 7 agents)
/plugin install animation-components    # React Spring, Magic UI, AOS, Anime.js, Lottie
/plugin install authoring-motion        # Blender, Spline, Rive, Substance 3D
/plugin install meta-skills             # Integration patterns + Modern design

# Або точково під hero-послідовність
/plugin install gsap-scrolltrigger
/plugin install react-three-fiber
/plugin install threejs-webgl
/plugin install locomotive-scroll
```
**Чому:** дає Claude Code реальне знання API GSAP timelines, ScrollTrigger pin/scrub, R3F хуків — не вгадування.

### 1.2 Frontend Design Toolkit — `wilwaldon/Claude-Code-Frontend-Design-Toolkit`
Робить вивід "красивим" замість "AI-slop". 23 анімаційні скіли + аудит руху.
```bash
/plugin marketplace add wilwaldon/Claude-Code-Frontend-Design-Toolkit
```
Має тюнери: **design variance** (safe→experimental), **motion intensity** (subtle→cinematic), **visual density**. Для ScrollCraft ставимо motion = **cinematic**, variance = **experimental** (але контрольовано).

### 1.3 Офіційні GSAP скіли — GreenSock
```bash
# через awesome-agent-skills (VoltAgent/awesome-agent-skills)
# greensock/gsap-frameworks — Vue/Svelte/React lifecycle, cleanup, scoping patterns
```
**Чому:** правильний cleanup GSAP у React/Next (gsap.context / useGSAP) — критично, інакше memory leaks.

### 1.4 Workflow-оркестрація — `obra/superpowers`
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```
Структурує роботу: **brainstorm → design spec → plan → subagent execution → review → merge**. Кожен крок у свіжому subagent-контексті (немає context drift на довгих білдах). Це наш "процес".

### 1.5 Plan-review агенти (Garry Tan / YC) — `garrytan/*`
```bash
# garrytan/plan-design-review — Senior Designer review, оцінка 0-10 + AI-slop detection
# garrytan/plan-eng-review     — Eng Manager: архітектура, data flow, edge cases
# garrytan/design-consultation — повна дизайн-система з нуля
```
**Чому:** `plan-design-review` — це твій "council" момент: AI-критик ставить оцінку дизайну до релізу й ловить "слоп".

### 1.6 Тестування/верифікація UI
```bash
# Playwright skill — скріншоти, перевірка анімацій, browser logs
# (з claude-plugins.dev / awesome-skills.com)
```
**Чому:** Claude Code сам відкриває білд, скролить, робить скрін і перевіряє, чи анімація працює.

### 1.7 Multi-model "council" (опційно)
Multi-model debate skill — шле спеку на адверсаріальний рев'ю кільком моделям. Збігається з твоїм "council llm".
> ⚠️ Безпека: шле контент у зовнішні API. Не вмикати для приватних/клієнтських даних.

### Підсумок мапінгу на твої згадані плагіни
| Ти назвав | Реальний відповідник |
|---|---|
| gstack | core bundle stack (R3F/Three/GSAP) — `core-3d-animation` |
| gsap | `gsap-scrolltrigger` + `greensock/gsap-frameworks` |
| uiux pro | `wilwaldon/...Toolkit` + системний `frontend-design` skill |
| design-engineer | `garrytan/plan-design-review` + `design-consultation` |
| council llm | multi-model debate skill (обережно з даними) |

---

## 2. OPEN-SOURCE РЕПОЗИТОРІЇ (вчимось / форкаємо код)

### Smooth scroll (фундамент усього)
- **`darkroomengineering/lenis`** — №1 smooth scroll. Не хайджекає нативний скрол, віддає правильну позицію. Синк із ScrollTrigger через `gsap.ticker`.
  ```js
  const lenis = new Lenis({ lerp: 0.1 })
  lenis.on('scroll', ScrollTrigger.update)
  gsap.ticker.add((t) => lenis.raf(t * 1000))
  gsap.ticker.lagSmoothing(0)
  ```
- **`scroll.locomotive.ca`** (Locomotive v5) — тонка обгортка над Lenis + detection-шар (in-view, parallax), 9.4kB.

### WebGL ↔ DOM синхронізація
- **`webgl-scroll-sync.lusion.co`** — демо Lusion: як синхронізувати WebGL-візуал з DOM-елементами під час скролу. Золотий стандарт.

### Codrops (tympanus.net/codrops) — скарбниця ефектів
- **`codrops/LiquidDistortion`** — рідинні переходи слайдшоу (PixiJS + GSAP).
- **`codrops/TextDistortionEffects`** — спотворення тексту (Blotter.js).
- Туторіали 2025: pixel/grid displacement + GPGPU з RGB-shift на курсор; distortion+grain на scroll із синком HTML↔WebGL; нескінченна 3D-карусель у R3F зі scroll-distortion; bubble-spheres GLSL; infinite procedural landscape (WebGPU).
- Тег `glsl` та `webgl` на Codrops — читати перед кожним шейдерним завданням.

### React Three екосистема (pmndrs)
- **`pmndrs/drei`** — хелпери R3F: `ScrollControls` + `useScroll` (range/curve/visible), `View`, `Text`, `Image`, `MeshTransmissionMaterial`, `Environment`.
- **`pmndrs/react-postprocessing`** — Bloom, DepthOfField, Noise, Vignette, ChromaticAberration. Bloom селективний (керується через luminance матеріалів).
  ```jsx
  <EffectComposer>
    <Bloom luminanceThreshold={1} luminanceSmoothing={0.9} intensity={0.8} />
    <DepthOfField focusDistance={0} focalLength={0.02} bokehScale={2} />
    <Noise opacity={0.025} />
    <Vignette offset={0.1} darkness={0.9} />
  </EffectComposer>
  ```

### Навчальні бази (як читати)
- **Three.js Journey** (threejs-journey.com) — пост-процесинг з R3F, шейдери.
- **Wawa Sensei** (wawasensei.dev) — R3F курс: Scroll, Post-processing, Particles, Trails, VFX Engine, Fireworks, Shader transitions.
- **Codrops "Building Efficient Three.js Scenes"** (2025) — оптимізація: LOD, KTX2 текстури, instancing.

---

## 3. СТУДІЇ-ОРІЄНТИРИ (від кого вчимось мисленню)

| Студія | Чим знаменита | Що крадемо |
|---|---|---|
| **Lusion** (lusion.co) | WebGL↔DOM scroll sync, фізичні матеріали | Синхронізація візуалу зі скролом, depth |
| **Active Theory** | Великі імерсивні WebGL-продакшни | Масштаб, "світ" замість "сторінки" |
| **Resn** (KPR — Awwwards Site of the Year) | Інтерактивний сторітелінг, click&hold | Шарова інтерактивність, наратив |
| **Chipsa** | 6 років WebGL: pixelation shader, Fresnel, KTX2, gyroscope mobile | Шейдер як "мова ідей", не декор |
| **Igloo Inc / Lusion / Apple AirPods** (з PDF) | Еталони з методички | База ScrollCraft |
| **-99 / Moxy / Ember House** (Awwwards) | Scroll-triggered, 3D-навігація | Свіжі патерни скрол-руху |

**Awwwards-ресурси для постійного підживлення:**
- awwwards.com колекції: `webgl`, `scrolling`, `scroll-animation`.
- Codrops стрічка — нові демо щотижня.

---

## 4. КОНКРЕТНІ ШЕЙДЕРНІ/МОУШН ТЕХНІКИ (рецепти "вау")

1. **Cursor Fluid (RGB displacement):** рендер сцени в render target → displacement shader на вихідну "картинку" сцени, зміщення = функція відстані до миші. (Codrops "Interactive WebGL Hover Effects").
2. **Scroll-velocity distortion:** передаємо velocity з Lenis у uniform → вертексний шейдер вигинає площину пропорційно швидкості.
3. **Image displacement transition:** дві текстури + displacement map, `mix()` керований progress (GSAP tween 0→1).
4. **Selective bloom:** матеріали "героя" виводимо за межі 0–1 діапазону кольору, `luminanceThreshold:1` → світяться лише вони.
5. **Film grain:** post Noise `opacity 0.02–0.03` + легка vignette — миттєвий кінематографічний шар.
6. **KTX2 / texture compression:** для швидкості й пам'яті на важких сценах (Chipsa, навіть на Safari).
7. **Frame-sequence canvas scrub (ядро PDF):** FFmpeg → WebP кадри → `drawImage` на canvas, індекс кадру = ScrollTrigger progress.

---

## 5. APIFY / NANO BANANA PRO / KLING — наш пайплайн даних і ассетів

- **Apify конектор** — скрапимо реальні дані (товари, ціни, тексти, зображення) для наповнення сайту замість lorem ipsum. Робить демо "живим" для клієнта.
- **Nano Banana Pro API** — генеруємо start/end frames (16:9 desktop + 9:16 mobile) прямо через API, без ручного Flow (PDF Pro Tip: інколи краще за Flow).
- **Kling 3 API** — image→video, 6s кінематографічний рендер, інтерполяція start↔end frame. Вихід → FFmpeg → WebP-послідовність.

**Пайплайн ассетів:**
```
Pinterest board → концепт
   → ChatGPT-стиль промпт (тепер пишемо в Claude як senior++ prompt engineer)
   → Nano Banana Pro API → start.webp + end.webp (16:9 + 9:16)
   → Kling 3 API → hero-desktop.mp4 + hero-mobile.mp4
   → FFmpeg → /public/frames/*.webp
   → Claude Code (canvas + ScrollTrigger) → жива секція
Apify → реальні дані → наповнення секцій
```

---

## 6. ЦІЛЬОВИЙ ТЕХ-СТЕК ScrollCraft PRO

```
Framework:     Next.js 15 (App Router) — SSR shell + client canvas
Smooth scroll: Lenis (+ синк зі ScrollTrigger)
Анімація:      GSAP + ScrollTrigger (+ SplitText для тексту)
3D:            Three.js / React-Three-Fiber + drei
Пост-процес:   @react-three/postprocessing (Bloom, DoF, Noise, Vignette)
Hero:          Canvas frame-sequence (WebP) ABO Video hero
Стилі:         Tailwind + CSS variables (теми) + кастомні шейдери (GLSL)
Зображення:    next/image (WebP, responsive, q≈75)
Шрифти:        next/font, distinctive display + refined body (НЕ Inter/Roboto)
Деплой:        Vercel
```
