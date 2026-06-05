# Lusion · Teardown #5 — lusion.co (Layer 3, live DOM + network) ✅
> Еталон #1 award-WebGL індустрії. Розбір через Claude in Chrome: DOM-зонд + WebGL-probe + network asset-трейс.
> **Категорія:** MOTION/WebGL/3D (чистий спектакль). Awwwards Site of the Year 2023.

---

## ГОЛОВНЕ (суть одразу)
1. **Власний WebGL2-движок, БЕЗ Three.js і БЕЗ фреймворку.** Probe: THREE=null, gsap=null, Lenis=false, React/Next/Vue=false. Лише **2 скрипти + 3 canvas + WebGL2**. Lusion пишуть свій рушій — це їхня головна відмінність від 99% студій.
2. **Майже весь контент мальований у canvas/шейдерах**, не в DOM (innerText бідний). Сайт — це 3D-сцена, не сторінка.
3. **Asset-пайплайн продакшн-рівня** (з network): власний бінарний формат геометрії `.buf` + LOD + packed-PBR текстури + matcap. Це інженерія рівня геймдеву, не «сайт».

---

## СТЕК (probe)
- Рендер: **WebGL2**, 3 canvas-шари, власний движок (не Three/Babylon/PIXI).
- Фреймворк: **відсутній** (vanilla JS, 2 скрипти). Не React/Next/Nuxt/Vue.
- Scroll: кастомний (Lenis НЕ використовують), `--vh` обчислюється в JS (власний viewport-менеджмент).
- Assets з окремого CDN: **lusion.dev** (відділений від основного домену).

## ТИПОСИСТЕМА
3 гарнітури:
- **Aeonik** — преміальний нейтральний гротеск (основний; швейцарська школа, як у Linear/багатьох tech-брендів).
- **IBM Plex Mono** — технічний моно для лейблів/деталей.
- **LusionMono** — ВЛАСНИЙ моно-шрифт (фірмова деталь, як LusionMono — кастомна айдентика).
Формула: 1 нейтральний гротеск + 2 моно (один кастомний). Контраст до Zera (антиква+гротеск) — тут технічно-нейтральна система, бо акцент на 3D, не на типографіці.

## ПАЛІТРА (21 токен) — «цифрова веселка на ч/б»
База: `--color-white #fff` / `--color-black #000` / `--color-off-white #f0f1fa` / `--color-grey-blue #2b2e3a`.
Акценти (насичені, чисті): electric blue `#0016ec` (header) + `#1a2ffb` + dark-blue `#071bdf` · red `#ff4c41` · **lime green `#c1ff00`** · purple `#8832f7` · magenta `#f0f`.
**Контраст до інших:** Zera/Telha — приглушені warm-нейтралі. Lusion — чисті насичені «цифрові» кольори (RGB-первинні), що світяться на ч/б базі. Це WebGL-естетика: кольори як емісія шейдера.

## МОУШН / WebGL-механіка
- 3 canvas-шари накладаються (multi-layer compositing).
- Кастомний scroll драйвить камеру по 3D-сцені (scroll = час/позиція в сцені, не прокрутка DOM).
- Matcap-освітлення (дешева імітація світла без real-time lights).
- Скелетна анімація моделей, тригериться скролом (`_in`/`_out_animation`).

## ASSET-ПАЙПЛАЙН (з network — найцінніше для нас) ★★★
Весь конвеєр видно у мережевих запитах до lusion.dev/assets:
- **Геометрія: `.buf`** — ВЛАСНИЙ бінарний формат (не glTF/.glb). Власна серіалізація вершин → менший розмір, швидший парсинг.
- **LOD-система:** `grid_base_ld.buf` vs `grid_base_hd.buf` (low/high detail) — підвантаження деталізації за потребою (продуктивність).
- **PBR-текстури packed:** `_arm` (AO+Roughness+Metallic в одному файлі — економія запитів), `_base` (albedo), `_nor` (normal). Формат **.webp** (легкі).
- **HDR:** `matcap.exr` (.exr = HDR), `white_matcap.jpg`.
- Контент-сцена: astronaut helmet/glove/wearpack (модульна модель), tunnels, broken_glass (+animation руйнування), earth, diamond, lines (line_reel/line_goal).
**Урок:** преміальний WebGL = не «додати Three.js», а інженерія ассетів: бінарна геометрія, LOD, packed-PBR, matcap, окремий CDN. Це найбільший технічний розрив між generic-сайтом і Lusion-рівнем.

## АРХІТЕКТУРА / КОПІРАЙТ
- Title: «Lusion — Award Winning 3D and Interactive Web Studio».
- Desc: «We design and produce 3D visual storytelling, immersive websites, and interactive digital experiences that help brands stand out online.»
- Видимий текст мінімальний: «LET'S TALK», «BACK» — навігація поверх 3D. Контент-сторітелінг живе в сцені (tunnels → astronaut → проєкти-картки в 3D-просторі).
- Нав: мінімальна, поверх canvas.

## ЧОГО ВЧИМОСЯ / ЩО БРАТИ В БАЗУ
- **Принцип «scene-as-site»:** на верхньому рівні cinematic-проєктів сторінка = 3D-сцена, DOM лише для SEO/нав. (наш максимальний tier «спектакль»).
- **Asset-оптимізація як окрема дисципліна** → toolbox: packed-PBR (_arm), LOD (_ld/_hd), .webp текстури, matcap замість real lights, окремий asset-CDN.
- **Кастомний моно-шрифт** як дешева айдентика-деталь → patterns.
- **Палітра-стратегія «насичені RGB на ч/б»** для WebGL-проєктів (емісійні кольори) — альтернатива warm-нейтралям → philosophy (вибір палітри під рендер-тип).
- **Realість для нас:** власний движок як Lusion — недосяжно/непотрібно. НАШ еквівалент = Three.js/R3F + ці ж принципи (LOD, packed текстури, matcap, scroll-driven камера). Беремо філософію, не реалізацію.

## TODO / порівняння
- Зіставити з Active Theory (#2) — теж власний тулсет, цікаво порівняти підхід.
- Lusion = верхня планка «scene-as-site». Більшість наших проєктів будуть гібридом (DOM-контент + WebGL-острівці), не повний canvas.
