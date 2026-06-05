# Research Report #3 — Award-Сайти 2026 (Поглиблене дослідження → R15-R24)
> Третє дослідження. Джерело технік R15-R24. 25+ нових сайтів по всіх 10 нішах + 11 Codrops-демо + GitHub-репо.
> Це повний текст дослідження, на основі якого створено recipe R15-R24. Тримаємо у файлі, щоб не загубити деталі/посилання.

## TL;DR
- **2025-2026 пік award-галерей** = WebGPU + TSL (Three Shading Language, що компілюється і в WebGL, і в WebGPU), персистентні scroll-driven сцени з безшовними scene-transitions, парадигма «світ замість сторінки» (Messenger SOTY 2025, threejs.paris).
- Знайдено 25+ нових сайтів + 11 Codrops-туторіалів з GitHub-репо. Більшість мапиться на R01-R14, але виявлено 10 НОВИХ технік → R15-R24.
- **Реалізовно через Claude Code:** усі DOM/GSAP/Lenis/R3F техніки — так, з готових репо. WebGPU/TSL compute-heavy (False Earth >1M травинок, threejs.paris SSGI) — частково (ручний тюнінг шейдерів + WebGL fallback).

## Ключові висновки
1. **TSL — головний технологічний зсув.** Node-based шейдери (`three/tsl` + `THREE.WebGPURenderer` з `three/webgpu`), компілюються і у WebGL, і у WebGPU. Домінують у топ-кейсах: Cartier W&W 2026 (Immersive Garden), Shader.se pipeline, Gommage (Codrops 28 Jan 2026), Fluid X-Ray (Codrops 23 Mar 2026, репо cullenwebber/three-skull), False Earth (Codrops 21 Apr 2026).
2. **Естетики на піку:** tactile brutalism / neo-brutalism (anti-design), editorial/типографіка-як-архітектура (vw-typography), retro-tech/80s, обмежений glassmorphism 2.0, dark mode як мова. WebGL/3D — НЕ універсальний тренд: лише там, де «бренд = досвід», бо вбиває CWV на мобільних.
3. **Awwwards Annual 2025:** Lando Norris (SOTY), Messenger (Developer SOTY), Scout Motors (E-commerce of Year), Immersive Garden (Agency of Year), Malvah (Studio of Year). Lando Norris і Scout Motors — обидва від OFF+BRAND; Lando Norris на Webflow + WebGL + Rive.
4. **Connective stack:** GSAP (ScrollTrigger/ScrollSmoother/Flip/CustomEase/Observer) + Lenis + Three.js/R3F + drei. Webflow+GSAP усе ще частий для «легших» award-сайтів.

## По нішах (нові сайти, не дубль уже-розібраних)

### 1. Real Estate
- **Springs** (springs.estate) — Awwwards SOTD 8 Mar 2026 + Developer Award. Quadro Room × UNIQ. Muted-палітра, арочні портали, smooth transitions. Мапінг: R03+R04+R11.
- **Elyse Residence** — HM 11 Apr 2026, Phenomenon Studio. Dark-on-light, floor-plan overlays, page transitions. → R04+R23.
- **FIND Real Estate** — HM 30 Mar 2026. PropTech discovery. → R10.

### 2. E-commerce / Product
- **Scout Motors** (scoutmotors.com) — **E-commerce of Year 2025** + SOTD, OFF+BRAND. EV-вантажівки, scroll-driven 3D reveal. → R03+R04+R05.
- **Miu Miu – The Holiday Diary** — HM, MONOGRID. Стек: Contentful, GSAP, Vue.js, TS. → R04.
- **Ray-Ban | Meta** — HM, EssilorLuxottica. Parallax + 3D product.

### 3. Luxury / Fashion
- **Cartier Watches & Wonders 2026** — SOTD 25 May 2026 + Dev Award, Immersive Garden + Mooders (audio). Jury: Design 7.73 / Usability 7.05 / Creativity 7.84 / Content 7.6. Стек (credits IG): **Three.js + GLSL + Blender + GSAP + Lenis + Web Audio API**. Шість 3D «alcoves» з gesture-інтеракціями + аудіо-наратив. → R03+R04+R07/R08+spatial-audio.
- **The Camel Fabric Game** (Max Mara) — HM 25 Feb 2026, Adoratorio. WebGL/Three.js + GSAP. (Adoratorio також: Untamed Heroine SOTD, Catch The Lantern SOTD.)
- **Ray-Ban.EXE** — HM 8 Feb 2026. Webtoon/dystopian cinematic.

### 4. Hospitality
- **Amrit Palace** (amritpalace.com) — SOTD + CSSDA, Artemii Lebedev. Стек: **Webflow + GSAP**. → R04+R11 (найлегша точка входу).
- **Explore Primland** — SOTD 4 Feb 2026 + Dev Award + CSSDA. Blue Ridge resort, heavy WebGL.
- **Studio Dado** — SOTD. Hospitality interior-design портфоліо.

### 5. Automotive
- **Project Cars** — HM. Стек: **Three.js + WebGI + Webflow**. Real-time конфігуратор. → R-configurator.
- **smart 3D Car Configurator** — HM. Performant 3D-конфігуратор.
- **Meyers Manx** — HM 16 Feb 2026. Electric heritage buggy.
- Open-source: github.com/theshanergy/4x4builder, github.com/rendercodeninja/automotive-configurator (Three.js + glTF).

### 6. SaaS / Fintech
- **Razorpay Sprint 26** — SOTD 26 May 2026 + Dev Award. Стек: **WebGL + Three.js + Webflow**. Scroll-one-pager з гігантським 3D-кросівком + 100+ продуктів. → R03+R04.
- **Cleo AI** — SOTD 23 May 2026 + Dev Award. AI-fintech, ультра-гладка motion.
- **Jeton** — SOTD + Dev + FWA + CSSDA + 2× CCP Gold, Bürocratik. Стек: **Vue/Nuxt + Sanity + Vercel + Lenis + GSAP + Rive + Matter.js**. Coin-motion 3D, морфінг desktop↔mobile. → R04 + Matter.js physics.

### 7. Agency / Portfolio
- **Malvah.Studio** — **Studio of Year 2025** + SOTD + Dev. Мінімалістичне 2-кольорове (#000/#fff). Проєкти: Seventeen (SOTD), KODE Immersive (WebGL SOTD).
- **Obys** (obys.agency) — кілька SOTD, Obys' Design Books (SOTD). Storytelling/motion.
- **Shader / shader.se** — SOTD. Real-time graphics studio. Codrops case study про scroll-driven WebGPU pipeline.

### 8. Beauty
- **Deep Beauty** — SOTD + Dev Award, Ogilvy Italia для KIKO Milano. Immersive art-exhibition. Heavy WebGL.
- **DG Beauty – My Lip Stylo** — SOTD 24 Dec 2024 + Dev (D&G Beauty). Immersive 3D memory-game. → R03 + gamified 3D.
- **OMR Beauty** — HM 3 Feb 2026. Shopify replatform, performance-focused.

### 9. F&B / Drinks
- **IVRESS Spin A Tale** — HM 25 Apr 2026 + CSSDA + FWA of Month May 2026, Laugh Mind Co. (Японія). Narrative spin.
- **Done Drinks** — HM. Playful motion + vibrant + storytelling.
- **KOJI** — beverage (award-статус не підтверджено).

### 10. Культура / Музика / Arts
- **threejs.paris** — FWA + Codrops case study 28 Feb 2026. David Ronai (Makio64) × Hervé Studio. Стек: **Three.js WebGPURenderer + SSGI + custom physics (Web Worker)**. Кожен відвідувач = глянцева фізична сфера («community as UI»). Деталі: ціль 120 FPS, KTX2 ETC1S/UASTC атласи, обхід «8 vertex buffers» через bit-packing у 4×vec4, avatar = 10-символьний base62 mixed-radix codec; патчили Three.js під iOS/Android. → R07 + R15 (WebGPU physics).
- **OceanX 2025** — SOTD 23 Feb 2026, Unseen Studio × PROPAGANDE. Стек: **WebGL + Three.js + Blender**. Horizontal-layout scroll-driven year-in-review. → horizontal scroll + R04.
- **Valorant 2025 Flashback** — HM 19 Dec 2025, Thinkingbox для Riot. Social-first video/motion.

## Нові техніки → recipe (мапінг)
- **R15** TSL Dual-Pipeline (WebGL/WebGPU). Libs: three/webgpu, three/tsl. Expert.
- **R16** Ping-Pong Fluid X-Ray Reveal. Репо: github.com/cullenwebber/three-skull (WebGL fallback гілка). Libs: three/webgpu, TSL, maath. Advanced.
- **R17** MSDF Dissolve/Gommage. Libs: Three MSDF Text (Léo Mouraire), msdf-bmfont, GSAP. Codrops 28 Jan 2026. Advanced.
- **R18** SVG Mask Scroll Transitions. Codrops 11 Mar 2026. GSAP+ScrollTrigger+Lenis. Intermediate.
- **R19** GSAP Flip Grid Morphs. Репо: codrops/MenuToGrid, GridLayoutAnimation, OneElementScroll. Codrops 20 Jan 2026. Intermediate.
- **R20** Infinite Pan-Anywhere Canvas. Репо: github.com/edoardolunardi/infinite-canvas. R3F+Three+TS. Intermediate-Advanced.
- **R21** Dithering/ASCII/Halftone. Репо: github.com/damarberlari/visualizing-dithering-codrops; @react-three/postprocessing Dithering; emilwidlund/ASCII. Intermediate.
- **R22** MeshTransmissionMaterial Glass. React Bits (github.com/DavidHDev/react-bits, modes lens/bar/cube). Codrops Glass Torus 13 Mar 2025. Beginner-Intermediate.
- **R23** Kinetic-Type Page Transitions. Репо: codrops/KineticTypePageTransition. Intermediate.
- **R24** Scroll-Driven SVG Path Draw. GSAP DrawSVG/MotionPath. Intermediate.
- Додатково: horizontal scroll (OceanX), Matter.js physics (Jeton), 3D configurator (Project Cars/smart), WebGPU compute particles/grass (False Earth, Codrops 21 Apr 2026, Expert, без публічного репо), scroll-driven WebGPU scene transitions (Shader.se, open-source обіцяний але ще не випущений).

## Codrops демо 2025-2026 (з GitHub)
1. Cinematic 3D Scroll with GSAP (19 Nov 2025) — github.com/JosephASG/codrops-cinematic-scroll-animations (CustomEase camera, GLTF, OGL).
2. Wavy Infinite Carousels in R3F + GLSL (26 Nov 2025) — vertex sine displacement, uProgress, CoverUV.
3. Animate WebGL Shaders with GSAP: Ripples/Reveals/Blur (8 Oct 2025, Adoratorio) — Draggable, click-ripple noise mask.
4. Responsive SEO-friendly WebGL Text (5 Jun 2025) — github.com/ehaakana/codrops-text-demo.
5. Infinite Canvas (7 Jan 2026) — github.com/edoardolunardi/infinite-canvas.
6. WebGPU Gommage (28 Jan 2026).
7. SVG Mask Transitions on Scroll (11 Mar 2026).
8. Dual-Scene Fluid X-Ray Reveal (23 Mar 2026) — github.com/cullenwebber/three-skull.
9. Animating 160k Cubes / Dithering (1 Apr 2026) — github.com/damarberlari/visualizing-dithering-codrops.
10. Scroll-Revealed WebGL Gallery + Astro + Barba (2 Feb 2026) — github.com/J0SUKE/gsap-threejs-codrops (Astro, Barba, GSAP Flip/SplitText/ScrollSmoother).
11. Shader.se WebGPU Pipeline case study (19 May 2026).

## GitHub репо (опорні)
- **codrops org** (github.com/codrops, 344 репо): KineticTypePageTransition, 3DCarousel, RepeatingImageTransition, ImageTrailEffects, MenuToGrid, GridLayoutAnimation, OneElementScroll, AnimatedCustomCursor, ElasticGridScroll, TileScroll, SmoothScrollAnimations.
- 14islands/codrops-scroll-rig-tutorial — progressive WebGL + Lens Refraction (R3F).
- cullenwebber/three-skull — WebGPU fluid X-ray + WebGL гілка.
- edoardolunardi/infinite-canvas, J0SUKE/gsap-threejs-codrops, damarberlari/visualizing-dithering-codrops, ehaakana/codrops-text-demo, JosephASG/codrops-cinematic-scroll-animations.
- DavidHDev/react-bits — Fluid Glass + інші R3F-компоненти.
- theshanergy/4x4builder, rendercodeninja/automotive-configurator (configurators).

## Тренди 2026
- Anti-design / neo-brutalism / tactile brutalism — для агенцій/fashion/арту; провал для fintech/healthcare/B2B.
- Типографіка як архітектура — vw-typography, variable fonts, kinetic type (kinetic type майже не йде в продакшн — б'ється зі screen readers + CWV, переважно demo).
- Retro-tech / 80s editorial — film grain, vignettes, Pantone Cloud Dancer 2026.
- Glassmorphism 2.0 — стриманіший.
- Bento grids + dark mode (CSS Subgrid).
- WebGPU/TSL — технічний пік award-галерей, production-обережно через performance budget.
- «Світ замість сторінки» — ігрові WebGL-світи (Messenger SOTY, threejs.paris, Bruno Simon).

## Три хвилі інтеграції
1. **Хвиля 1 (низький ризик):** R18, R19, R22, R24 — DOM/GSAP/drei, готові репо.
2. **Хвиля 2 (R3F):** R20, R16, wavy carousel — клонувати репо, адаптувати під Next.js (dynamic import ssr:false).
3. **Хвиля 3 (frontier):** R15, R17, R21 — лише desktop-first з WebGL fallback. Поріг: mobile-трафік <40% і LCP-бюджет дозволяє.

## Per-niche default
- Real estate/hospitality → Webflow+GSAP легка (Amrit Palace) або R04+R11.
- Fashion/luxury → Immersive Garden модель (Three.js+GLSL+Lenis+Web Audio, R03+R04).
- Automotive/e-commerce → 3D configurator.
- SaaS → scroll-one-pager (Razorpay, R03+R04).
- Culture → world/horizontal scroll.

## Caveats (важливо)
- Стеки кількох сайтів НЕ підтверджені першоджерелом детально (Scout Motors libs, Cleo AI, Deep Beauty, Explore Primland, Camel Fabric, Ray-Ban.EXE, Valorant) — Awwwards «tech» теги часто категорії, не реальний стек. Підтверджені: Lando Norris (Webflow+WebGL+Rive), Cartier (Three.js+GLSL+Blender+GSAP+Lenis+Web Audio).
- «Fluid Glass» named-компонент = React Bits, НЕ Codrops-туторіал.
- False Earth і Shader.se — case-study статті, не клоновані туторіали; Shader.se open-source обіцяний але ще не випущений.
- Точні repo-slug для Wavy Carousel, Gommage, SVG Mask — перевіряти прямим fetch live-статей Codrops.
