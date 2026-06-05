# Immersive Garden · Teardown (Luxury Agency Benchmark)
> Agency of the Year 2025 (Awwwards). Cartier, Dior, Louis Vuitton, Omega, Longines.
> Витягнуто через Apify (повний Nuxt-DOM головної). Інший підхід ніж ERA — еталон luxury-портфоліо + persistent WebGL.

## СТЕК (декодовано)
| Технологія | Доказ |
|---|---|
| **Nuxt 3 (Vue)** | `#__nuxt`, `data-v-*` scoped-hash на кожному елементі, `<!--[-->` Vue fragment-маркери |
| **Єдиний persistent WebGL canvas** | `<div class="webglApp"><canvas></canvas></div>` — ПОЗА `.page`, на рівні `.main` |
| **WebGL-заголовки** | `data-webgl-title` на `<h1>` — текст рендериться у WebGL (SDF), не DOM |
| **DigitalOcean Spaces CDN** | усі медіа з `ig-medias-prod.ams3.digitaloceanspaces.com` |
| **Lottie** | `lottie-animation-container` (іконки кнопок) |
| Lazy media | `data-src` (не `src`) + кастомний lazy-loader |

## КЛЮЧОВА АРХІТЕКТУРА — persistent WebGL
```html
<div class="main">
  <div class="webglApp"><canvas></canvas></div>   <!-- ОДИН canvas на весь сайт -->
  <div><div class="page homePage">...</div></div>  <!-- DOM-контент поверх -->
</div>
```
**Урок:** на відміну від звичайних сайтів, де кожна секція має свій canvas, тут ОДНА WebGL-сцена живе постійно. DOM-контент — це «проксі»-розмітка, а реальний рендер (зображення, відео-текстури, 3D, переходи) — у спільному canvas. Саме тому переходи між сторінками безшовні (нічого не перемонтовується). Це той самий принцип, що ми кладемо в R03 (persistent canvas + R3F).

## GRID-СИСТЕМА КОНТЕНТУ (майстерність luxury-лейауту)
Кожен медіа-блок позиціонується утилітами на 12-колонковій сітці:
```html
<img class="mediaBlock__image image width__6 position__2 offsetY__center offsetX__inherit" .../>
<img class="... width__4 position__8 portrait" .../>
<img class="... width__3 position__3" data-src=".../CARTIER_EOY_v3.glb" .../>
```
- `width__N` — скільки колонок займає (3-6 типово).
- `position__N` — з якої колонки починається.
- `portrait` — вертикальний формат.
- `offsetY__center` / `offsetX__inherit` — вирівнювання.
**Урок:** асиметричні композиції (6+4, 5+3, зсуви) створюють «редакторський»/галерейний ритм — НЕ нудна сітка карток. Кожен проєкт = унікальна композиція з 2-3 медіа різних розмірів і позицій.

## МЕДІА-СТРАТЕГІЯ
- **Відео — основний формат** (не зображення!): `.mp4` для кожного кейсу (Louis Vuitton, Cartier, Dior, Omega...). Автоплей, muted, loop, lazy через `data-src`.
- **`.glb` 3D-моделі просто в стрічці**: `CARTIER_EOY_v3.glb`, `CARTIER_WW_24_v3.glb` (`width__3/4`, 800×800) — інтерактивні 3D-об'єкти між відео-блоками, рендеряться у спільний canvas.
- Змішування форматів: відео 16:9 + відео portrait 9:16 + статичне jpg + glb — у межах одного проєкту.

## КАСТОМНИЙ SCROLL-CURSOR (velocity dots)
```html
<div class="scrollCursor">
  <div class="dots dots__fast">
    <!-- ~120 <div class="dot"> -->
  </div>
</div>
```
- Шлейф зі ~120 крапок, що тягнуться за курсором; при швидкому скролі (`dots__fast`) розтягуються. Velocity-based particle trail. Додаємо в R02 як рівень C+.

## INTRO-LOADER
```html
<div class="introLoader">
  <div class="introLoader__logo"></div>
  <div class="introLoader__progressBar"></div>
  <div class="introLoader__baseline">
    <div>Innovative </div><div>digital </div><div>experiences </div><div>studio </div>
  </div>
  <div class="introLoader__scrollDown"><div>Scroll down</div></div>
</div>
```
- Прогрес-бар + слова по одному (stagger reveal) + «Scroll down». Чистіше за ERA (без WebGL splash), але той самий принцип «лоадер = частина оповіді».

## КОПІРАЙТ (luxury-tone, антислоп)
Приклади з сайту:
- Hero: "Transcend anything seen or felt before by crafting unparalleled experiences for ambitious brands."
- Проєкти: "Step into David Whyte's poetry in a digital journey, capturing life's profound moments with artistic depth."
- "Dive into a journey with Jake Gyllenhaal, unveiling Cartier's timeless bond with time."
**Урок:** дієслова дії на старті ("Transcend", "Step into", "Dive into", "Discover", "Explore"), емоція + конкретика, без AI-кліше. 1 речення на проєкт.

## ЩО РОБИТЬ AWARD-РІВНЕМ (уроки)
1. **Один persistent WebGL canvas** — безшовні переходи, єдина сцена.
2. **Асиметрична grid-композиція** (width/position утиліти) — редакторський ритм замість карток.
3. **Відео-first** — рух у кожному блоці, не статика.
4. **3D .glb просто в стрічці** — інтерактивні об'єкти між відео.
5. **WebGL-текст** (SDF) — типографіка в тій же сцені, можна деформувати/анімувати шейдером.
6. **Velocity dot-cursor** — фірмова інтерактивна деталь.

## МАПІНГ НА НАШ СТЕК (як повторити через Claude Code)
| IG робить | Ми (Next.js + R3F) | Recipe |
|---|---|---|
| Persistent WebGL canvas (Nuxt) | R3F `<Canvas>` на рівні layout + drei `<View>` / scroll-rig | R03 |
| width/position grid | CSS Grid 12-col + утиліти width-N/pos-N | новий: grid-system у 00_foundations/toolbox.md |
| WebGL-заголовки (SDF) | troika-three-text / MSDF (drei `<Text>`) | R-text (новий) |
| .glb у стрічці | drei `useGLTF` + `<View>` inline | R03/R10 |
| Velocity dots cursor | R02 рівень C+ (particle trail на scroll-velocity) | R02 |
| Відео-текстури у WebGL | drei `useVideoTexture` на площинах | R-media (новий) |
| Lazy data-src медіа | next/image + IntersectionObserver | — |

## НОВІ ЕЛЕМЕНТИ ДЛЯ БАЗИ
- **Grid-композиція 12-col з width__N/position__N** — додати в 00_foundations/toolbox.md як патерн luxury-лейауту.
- **R02 рівень C+**: velocity dot-trail cursor (шлейф частинок за курсором, реагує на швидкість скролу).
- **Відео-текстури у WebGL** (`useVideoTexture`) — рух у спільній сцені.
- **WebGL/SDF-заголовки** — для деформованої типографіки.
- Підтвердження R03 (persistent canvas) — на реальному award-сайті.
