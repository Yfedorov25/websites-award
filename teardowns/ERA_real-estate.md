# ERA.estate · Deep Teardown (Real Estate Award Benchmark)
> Triple Site of the Day (Awwwards + FWA + CSSDA). Студія Vide Infra.
> Витягнуто через Apify (повний рендерений DOM обох сторінок: / та /3d-map).
> Це ГОЛОВНИЙ еталон для нерухомості / ЖК. Розбір на рівні коду.

---

## 0. СТЕК (декодовано з DOM-атрибутів)
| Технологія | Доказ у коді |
|---|---|
| **Barba.js** (page transitions) | `data-barba="container"` `data-barba-namespace="page"` |
| **Locomotive Scroll** (smooth + sticky) | `data-scroll` `data-scroll-sticky` `data-scroll-target` `data-scroll-section` |
| **Custom parallax engine** | `data-plugin="parallax"` + `data-parallax-pattern="..."` + `data-parallax-0-30="{...}"` (keyframe-based) |
| **WebGL / Three.js** | `<canvas id="v3d-container">` (splash), `.js-map-3d-container` (3D-карта), `.webgl-deco` |
| **Frame-sequence** | `data-plugin="sequence"` (див. §2) |
| **Reveal engine** | `data-reveal="..."` `data-reveal-duration` `data-reveal-delay` |
| **Vimeo** (architect interviews) | `data-video-modal-video-embed-type="vimeo"` |
| Comagic (callback widgets) | `data-plugin="comagicWidget"` |

> Це не React/Next. Це кастомний vanilla-стек студійного рівня (Barba + Locomotive + власні plugin-движки на `data-plugin`). Наш Next.js+R3F+GSAP+Lenis дає той самий результат сучаснішим стеком.

---

## 1. АРХІТЕКТУРА LANDING (/) — порядок секцій
1. **Подвійний preloader:**
   - звичайний (`js-preloader`, градієнт-анімація);
   - **landing splash з WebGL** (`js-preloader-landing` → `<canvas id="v3d-container">`) + кнопка **Skip** (`js-splash-skip`). Показується лише якщо splash-screen увімкнено (`data-preloader-hide-if-splash-screen`).
2. **Intro-sticky hero:** sticky-контейнер (Locomotive `data-scroll-sticky`), фон з gradient-animation «flower», parallax-зображення з clip-reveal (`intro-clip-in`), заголовок "place / of / art" (split на 3 рядки), CTA-стрілка вниз. + плаваючий блок «3D-map» внизу (sticky, веде на /3d-map).
3. **Art Deco секція:** заголовок з `visualizationLines` (порядкова анімація), `webgl-deco` canvas (декоративний WebGL), parallax «section-shade», «flower» gradient.
4. **Architecture (frame-sequence!):** будівля анімується послідовністю кадрів (див. §2) + sticky «window»-кадр, що розкриває інтер'єр, parallax-shade.
5. **Architect quote + interview** (Vimeo modal), 960×960 background-відео.
6. Далі: галереї планувань, /flats, /visual-search, /3d-tour.

### Reveal-патерни ERA (точні)
- `data-reveal="title"` / `"text"` / `"title-image"` / `"flower"` / `"intro-clip-in fade-in-intro"` / `"move-up"`.
- `data-reveal-duration="3000"` (3s — повільно, кінематографічно!), `data-reveal-delay="0"`.
- `visualizationLines` plugin — заголовок проявляється по рядках (`--line-total` CSS-var).
> Урок: award-нерухомість використовує ДОВГІ reveal (3s) — це створює відчуття «розкоші, що не поспішає». Наш дефолт був 0.9s; для luxury-real-estate беремо 1.5–3s.

---

## 2. FRAME-SEQUENCE (точна формула, підтверджена!)
Анімація будівлі в секції Architecture:
```html
<div class="architecture__canvas"
     data-plugin="sequence parallax"
     data-sequence-image-source="/assets/architecture/sequence-reverse/ANIMA_6_00"
     data-sequence-frame-count="149"
     data-sequence-image-postfix=".png.webp"
     data-parallax-pattern="architecture-building"></div>
```
**Розшифровка:**
- **149 кадрів**, шлях `ANIMA_6_00{0001..0149}.png.webp`.
- Формат **`.png.webp`** — PNG-рендер (з альфа-каналом, бо будівля на прозорому фоні!) конвертований у WebP. Це ключ: будівля «вирізана», лягає поверх фонового зображення.
- `sequence-reverse` — кадри програються у зворотному порядку при скролі вгору.
- Поєднано з `parallax` — кадри ще й паралаксяться.
- **Тільки desktop** (`is-hidden--sm-down`); на mobile — статична `<picture>` (`architecture__building is-hidden--md-up`).

> Це РІВНО наш recipe R01, підтверджений на award-сайті. Цифри-еталон: **~149 кадрів, WebP з альфою, desktop-only, fallback-картинка на mobile.** Наш медіа-пайплайн: рендер будівлі в Blender/Kling з alpha → FFmpeg → `.webp` з alpha → sequence-плеєр.

### Sticky «window reveal» (архітектурний прийом)
`architecture__frame-sticky` + `data-scroll-sticky` + `architecture__window` (parallax) — «вікно», що розширюється при скролі й відкриває інтер'єрне зображення. Це патерн **mask-reveal через scroll** (clip/scale маски). Додаємо в бібліотеку як новий патерн.

---

## 3. 3D-КАРТА РАЙОНУ (/3d-map) — killer feature для нерухомості
Це найцінніше для ЖК. Повна декодована структура:

### Контейнер і движок
```html
<div class="page-3d-map" data-plugin="cursor map3D" data-cursor-is-clickable="false">
  <div class="js-map-3d-container"></div>   <!-- сюди рендериться Three.js scene 3D-моделі району -->
  ...
</div>
```
- `data-plugin="map3D"` — кастомний Three.js-плеєр 3D-моделі району (будівлі кварталу + ЖК у центрі).
- `cursor` plugin — кастомний курсор (`page-3d-map-cursor`, `js-cursor-button`).

### Спеціальний 3D-map preloader (вертикальний роллер!)
```html
<div class="preloader__percent-number">
  <div class="preloader__percent-item">100</div>
  ... 90, 80, ... 10
  <div class="preloader__percent-item">0</div>
</div>
<div class="preloader__percent-postfix">%</div>
```
- Лічильник іде **100→0** (роллер цифр, translateY-анімація стопки) — нестандартно й преміально (зворотний відлік замість 0→100).

### Компас
```html
<div class="page-3d-map-compass">
  <span>C</span>
  <div class="page-3d-map-compass__arrow js-page-3d-map-compass"></div>
</div>
```
- Стрілка обертається синхронно з обертанням 3D-камери навколо моделі. Дає відчуття орієнтації в просторі.

### POI-модалки (точки інтересу) — data-driven
9 точок інтересу (театр, церква, стадіон, ринок, дім музики, плаза, монастирі), кожна:
```html
<div class="page-3d-map-modals__item" data-id="0">
  <div class="...__time h2">7</div>  <span>minutes ride</span>
  <p class="...__uptitle">Pavlovskaya str., 6</p>          <!-- адреса -->
  <p class="...__title text-lead">Teresa Durova Theater</p> <!-- назва -->
  <picture>...image-1.webp...</picture>                      <!-- фото 720×900..1282×1602 -->
  <p>To immerse oneself in the unique world of art...</p>    <!-- опис -->
</div>
```
- На 3D-моделі району розставлені інтерактивні маркери; клік → відкривається ця модалка з фото + час їзди + опис.
- `page-3d-map__dim` — затемнення фону при відкритій модалці (parallax opacity 0→1).
- Це **продаж локації через досвід** — не «поруч є інфраструктура», а кінематографічна подорож по кварталу.

### Структурний висновок 3D-карти
**Це НЕ Google Maps embed.** Це власна 3D-сцена (Three.js): стилізована модель району з будівлями, ЖК підсвічений у центрі, обертова камера, клікабельні POI з багатими модалками, компас, кастомний курсор, зворотний preloader. Саме це робить відчуття «$10k+».

---

## 4. ЩО РОБИТЬ ERA AWARD-РІВНЕМ (уроки для нашої бази)
1. **Повільний кінематографічний темп** — reveal 3s, не 0.9s. Розкіш = час.
2. **Frame-sequence з альфа-каналом** — будівля «вирізана» й лягає на parallax-фон (не просто відео на весь екран). `.png.webp`.
3. **3D-карта району як окремий досвід** — продає не квартиру, а життя в локації. POI з часом їзди + фото + наратив.
4. **Подвійний/спеціалізований preloader** — splash WebGL на головній, зворотний 100→0 роллер на 3D-карті. Лоадер = частина шоу.
5. **Sticky window-reveal** — масковане «вікно», що відкриває інтер'єр на скролі.
6. **Persistent transitions (Barba)** — між сторінками сцена не перезавантажується.
7. **Mobile = статичні fallback** — важкий WebGL/sequence лише desktop, на mobile `<picture>`. Перформанс-дисципліна.
8. **Емоційний копірайт** — «place of art», «art de vivre», цитати архітектора. Не «1-3 кімнатні від $X».

---

## 5. ЯК ПОВТОРИТИ 1-В-1 ЧЕРЕЗ CLAUDE CODE (мапінг на наш стек)
| ERA робить | Ми робимо (Next.js + R3F) | Recipe |
|---|---|---|
| Barba page transitions | R3F persistent canvas + GSAP transitions (R03) | R03 |
| Locomotive sticky/scroll | Lenis + GSAP ScrollTrigger pin | R04 |
| Frame-sequence будівлі (149 .png.webp alpha) | canvas/WebGL frame-seq з alpha (R01) + Kling/Blender рендер з прозорістю | R01 |
| Custom parallax keyframes | GSAP ScrollTrigger scrub + data-driven keyframes | R04 |
| 3D-карта району (map3D) | R3F-сцена: low-poly модель кварталу + InstancedMesh POI-маркери + raycaster + OrbitControls (обмежений) + HTML-overlay модалки (drei Html) | **R10 (новий)** |
| Зворотний preloader-роллер | GSAP stagger translateY стопки цифр | R-preloader |
| Sticky window-reveal | clip-path/scale маска на ScrollTrigger | **R11 (новий)** |
| Custom cursor | R02 | R02 |
| POI-модалки data-driven | React-компонент з JSON даних POI | R10 |

**Медіа-пайплайн для ЖК (наш):**
```
3D-модель будівлі (Blender / готова від забудовника / Kling-рендер)
  → рендер обертання з alpha-каналом (PNG sequence, ~120-150 кадрів)
  → FFmpeg → .webp з alpha
  → R01 frame-sequence плеєр (scroll-scrubbed)
Модель району → low-poly Three.js scene АБО стилізований 3D-рендер
  → R10 інтерактивна 3D-карта з POI
Реальні фото локації (Apify-скрап / від забудовника) → POI-модалки
```

---

## 6. НОВІ RECIPE, ЯКІ ДОДАЄМО (з ERA)
- **R10 · Interactive 3D District Map** — Three.js модель кварталу, клікабельні POI-маркери (raycaster), HTML-overlay модалки з фото+наратив, компас, обмежений orbit. Killer feature нерухомості.
- **R11 · Sticky Window/Mask Reveal** — масковане «вікно», що розкриває зображення/інтер'єр на scroll (clip-path scale).
- **R-preloader варіації** — splash WebGL, зворотний роллер 100→0, counter 0→100.
- Оновлення **R01** — додати alpha-channel sequence (`.png.webp` з прозорістю + parallax-фон під будівлею) як «ERA-mode».
- Оновлення правил темпу — для luxury/real-estate reveal 1.5–3s (не 0.9s).
