# Zera Studio · Teardown #1 — Homepage (Deep / Layer 1+2+3) ✅ VERIFIED
> URL: https://zerasoftwarestudio.com/ · Apify (screenshot+рендернутий DOM) + rag-web-browser (meta) + **Claude in Chrome (Layer 3: live DOM, токени, шрифти, бібліотеки, повний копірайт)**.
> Це **головний еталон для нашої власної ніші** — студія, що продає веб/моушн/3D-послуги. Те, чим ми хочемо стати.
> Усі токени/шрифти/бібліотеки звірені на живій сторінці — це верифікована версія.

---

## META / ПОЗИЦІОНУВАННЯ
- **title:** "Zera Studio — Branding, software development & design"
- **description:** "Zera Studio is a design-led studio for branding, custom software development, and immersive web experiences. We build products, sites, and systems with clarity, craft, and performance."
- **author:** Zera Studio
- Самопозиціонування на сайті: *"Zera Studio / Web & Digital Arts"* (вертикальний бічний підпис).
- Бренд-обіцянка (hero): **"BUILT TO BE FELT. NOT JUST SEEN."** + мікропідпис *"A new frontier in web design. We build emotional interfaces."*

---

## СТЕК (декодовано з DOM)
| Технологія | Доказ у DOM |
|---|---|
| **Next.js (App Router, RSC)** | `<next-route-announcer>`, `/_next/image?url=...&w=...&q=75` для всіх зображень, `BAILOUT_TO_CLIENT_SIDE_RENDERING` у до-гідраційному HTML |
| **React (client-side hydration)** | контент рендериться на клієнті (порожній каркас до гідрації) |
| **Tailwind CSS** | utility-класи `fixed inset-0 z-[-2]`, `md:hidden`, `grid-cols-1 md:grid-cols-[1.1fr,0.9fr]`, `flex-shrink-0` |
| **Three.js r162** | `<canvas data-engine="three.js r162">` — фіксований fullscreen WebGL фон (`fixed inset-0 z-[1]`) |
| **GSAP ScrollTrigger (pin)** | `.pin-spacer` з `height: 6232px; padding-bottom: 5032px` + inline `translate/rotate/scale: none; transform: translate(0,0)` — фірмовий слід GSAP-pin + horizontal-scroll tween |
| **Lenis (ймовірно)** | плавний скрол + scroll-jacking сторітелінг (підтвердити в Layer 3) |
| **next/image** | `data-nimg="fill"`, `srcset` на 256→1920w, `loading="lazy" decoding="async"`, формат `.webp`/`.png` |

**Висновок про стек:** це майже точно **наш цільовий стек** (Next.js + R3F/Three + GSAP + Lenis + Tailwind). Тобто референс не лише естетичний, а й технічно повторюваний нами 1:1.

---

## ТИПОГРАФІЧНА СИСТЕМА (3 шрифти, чіткі ролі) ★ ключовий урок
Три гарнітури, кожна з власною роллю — це і є «дорогий» типографічний контраст:

1. **Cormorant Garamond** (`--font-cormorant`) — **display / заголовки**.
   - `font-weight: 500`, часто `font-style: italic`, `line-height: 0.88–0.95`, `letter-spacing: -0.02em`.
   - Розмір hero: `clamp(2.4rem, 10vw, 9rem)`; H2 секцій: `clamp(3rem, 7.5vw, 7rem)`.
   - Високонтрастна антиква-курсив = розкіш/редакторськість. **Антитеза Inter.**
2. **Space Mono** (`--font-space-mono`) — **підписи / body / лейбли / списки**.
   - `font-weight: 400`, `text-transform: uppercase`, `letter-spacing: 0.12–0.4em` (велике трекування — фірмова деталь), `line-height: 1.7–2`.
   - Розміри крихітні: `clamp(0.5rem, 0.65vw, 0.6rem)`. Моноширинний + uppercase + широкий трекінг = «технічний/інженерний» регістр.
3. **Jost** (`--font-jost`) — **лого / навігація (chrome UI)**.
   - `font-weight: 300`, `letter-spacing: 0.22em` (лого) / `0.55em` (вертикальна нав), uppercase.

**Формула контрасту:** антиква-курсив (емоція) × моно-uppercase (точність) × тонкий гротеск (службовий UI). Три регістри = три голоси на одній сторінці.

---

## ПАЛІТРА (CSS-токени дизайн-системи) ★
| Токен | Значення | Роль |
|---|---|---|
| `--studio-bg-void` | `#0a0a0a` | глобальний чорний фон (фікс-шар `z-[-2]`) |
| `--studio-bg-paper` | теплий світлий (≈`#f4efe6`/`#ece6d9`, з градієнтів) | фон секцій-панелей (світлі картки на темному) |
| `--studio-ink-inverse` | `#f5f5f0` | теплий білий текст на темному |
| `--studio-ink-primary` | темний (≈`#0f0f0f`) | текст на світлих панелях |
| `--studio-accent-gold` | `#b8a07a` | акцент-золото (крапки, лінії, «.» у заголовках, лейбли tier) |
| `--studio-accent-chrome` | (хром/сірий) | бордюри focus-станів |
| hero-текст | `rgb(240, 230, 211)` | кремовий, теплий (не чистий білий) |

**Урок:** не «чорно-біле», а **тепла монохромна** гама (void-чорний × paper-кремовий × золото). Золото — єдиний акцент, дозований (крапка в кінці заголовка, мікролінії в списках). Жодних яскравих кольорів. Це і є люкс-стриманість.

---

## МАКРО-АРХІТЕКТУРА СТОРІНКИ (шарова) ★★ головний урок
Сторінка побудована як **стек фіксованих шарів + один довгий scroll-драйвер**:

```
z-[-2]  Чорний фон (--studio-bg-void), fixed inset-0
z-[-1]  Hero-типографіка (fixed): "BUILT TO / BE FELT. / NOT JUST / SEEN." — розкидана по кутах,
        pointer-events:none, керується скролом (parallax translateY/translateX), will-change:opacity
z-[1]   WebGL-canvas (three.js r162), fixed inset-0, pointer-events:auto — 3D-сцена-герой
z-[10]  <main>: контент поверх
        ├─ #studio-story — ПОРОЖНІЙ div height:1000vh (scroll-драйвер для WebGL+типо-параласту)
        └─ studio-epilogue — реальний DOM-контент, що "наїжджає" знизу (margin-top:-4vh, rounded-top 24px)
z-50    Бічні фіксовані aside-навігації (зліва identity, справа меню) — vertical-rl текст
z-[60]  Mobile лого + бургер
```

**Ключова механіка `#studio-story` (height:1000vh):**
- Це **порожній високий блок** — він не містить контенту, лише задає 1000vh висоти прокрутки.
- Поки юзер скролить ці 10 екранів, **WebGL-сцена (canvas) і hero-типографіка анімуються** (scroll-driven), а DOM-контенту ще немає.
- Тобто перші ~10 екранів скролу = чистий cinematic WebGL-сторітелінг героя, типографіка літає параласт-шарами.
- Лише після цього знизу «наїжджає» `studio-epilogue` із заокругленим верхом (rounded-top-24, margin-top:-4vh) — класичний reveal «контент випливає з-під сцени».

**Це і є рецепт «BUILT TO BE FELT»:** довга беззмістовна (навмисно) cinematic-преамбула на WebGL, перш ніж показати послуги. R03 (persistent canvas) + R04 (scroll choreography) у чистому вигляді.

---

## ГОРИЗОНТАЛЬНА ПАНЕЛЬНА ГАЛЕРЕЯ (#work) ★★
Після сторітелінгу йде секція `#work` — **horizontal scroll, пінований GSAP-ом**:
- `.pin-spacer height: 6232px, padding-bottom: 5032px` → секція пінується на ~1200px viewport, поки прокручується ~5000px вертикалі = горизонтальний рух 3 панелей.
- Контейнер: `display:flex; width:max-content; column-gap: --panel-gap` — панелі в ряд, рухаються по X через GSAP transform.
- 3 `<section class="h-panel">`, кожна `width: calc(100vw - shell-gutter - 2*panel-gap)`, `border-radius: clamp(20px,2.4vw,32px)`, фон `--studio-bg-paper` (світла картка).
- Це класичний **vertical-scroll → horizontal-translate** патерн (як ERA/IG, але тут на послугах, не на проєктах).

**3 панелі = 3 tiers послуг** (паралель до ScrollCraft PDF tiers!):
1. `(01) — Foundation` → **WEBSITE & DESIGN.** — мармуровий бюст, підпис *"Where craft meets clarity."*
2. `(02)` → **3D Experiences** — 3D-скульптура (місяць-сфера в кільці на постаменті), список: 3D Website Elements / Interactive Scroll Animations / GSAP Advanced Motion / Experimental Web Interactions. Стрічка-діаграма Strategy→(Z Impact)→Execution.
3. `(03 / 03) — Flagship` → **GROWTH SYSTEM.** — композиція з каменів, *"Everything in Website & Design — plus the engine."*

**Декоративні номери панелей:** `(01) — Foundation`, `(01 / 03)` — моно-uppercase лейбли в кутах кожної панелі = «редакторська пагінація». Дешева на вигляд деталь, що додає дорожнечі.

---

## МІКРО-ПАТЕРНИ (деталі, що роблять «дорого») ★
1. **Заголовок із золотою крапкою:** `WEBSITE &<br>DESIGN<span color:gold>.</span>` — крапка акцентним золотом. Повторюється: `System.`, `Growth/System.`
2. **Список із золотою рискою-маркером:** замість буліту — `<span>` 0.55rem×1px золота горизонтальна лінія зліва (`top:0.55em`). На мобільному — ромб 4×4px rotate(45deg).
3. **Роздільник «лейбл — лінія — крапка»:** `Our Services ———————— •` (моно-лейбл + flex-1 hairline + крапка). Повторюваний секційний хедер.
4. **Footer кожної панелі:** `(01/03) — Foundation` | курсивна антиква-цитата по центру | `Continue →`. Тричастинний.
5. **Бічні vertical-rl aside:** ліворуч `Zera Studio / Web & Digital Arts` (rotate 180), праворуч меню `Work / Studio / Portal / Inquire` — вертикальний текст замість горизонтального топ-меню.
6. **Анімована вертикальна лінія-індикатор** у лівому aside: `<div height:100% scaleY(0) will-change:transform>` — росте по скролу (scroll-progress бар, вертикальний).
7. **Радіальні золоті світлові плями** на панелях: `radial-gradient(80% 60% at 70% 55%, rgba(184,160,122,0.06), transparent)` — ледь помітне тепле підсвічування.
8. **Класичні скульптури як візуальний мотив:** мармуровий бюст, 3D-сфера-місяць, кам'яні композиції — «класика × діджитал». Оброблені фільтрами `contrast(1.3) sepia(0.18) mix-blend-mode:multiply`.
9. **easing-підпис:** `cubic-bezier(0.22, 1, 0.36, 1)` (easeOutQuint) на ВСІХ transition, `0.4s`. Фірмовий «дорогий» вихід.
10. **Дзеркальна типографіка:** hero "BE FELT." має `transform: scale(-1,-1)` — перевернутий рядок (художній прийом).

---

## КОПІРАЙТ (повний розбір, антислоп) ★
**Hero:**
- `BUILT TO / BE FELT. / NOT JUST / SEEN.` — короткі ударні рядки, антитеза (felt vs seen).
- `A new frontier in web design.` / `We build emotional interfaces.` — позиціонування через концепт «emotional interfaces».
- `Scroll to explore` — мікрокопі-запрошення.

**Секції послуг:**
- `WEBSITE & DESIGN.` → *"A custom-crafted website designed to establish your brand, build credibility, and give your business a professional digital foundation that stands out."*
- `3D Experiences` → *"3D digital experiences that elevate brands through unforgettable visual storytelling."*
- `GROWTH SYSTEM.` → *"A complete digital growth engine that combines website, landing pages, paid advertising, SEO, and ongoing optimization to generate leads and turn your online presence into a revenue machine."*

**Курсивні «брейк»-фрази (антиква):**
- *"Where craft meets clarity."*
- *"Design-first. Everything else follows."*
- *"Everything in Website & Design — plus the engine."*

**Лейбли-пагінація (моно):** `(01) — Foundation`, `(02)`, `(03 / 03) — Flagship`, `(Three Tiers) — Pick Your Path`, `Our Services`, `Strategy / Execution`.

**Урок копірайту:** короткі обіцянки-концепти (емоція) у display-антикві + конкретні буліти послуг у моно-uppercase. Дієслова-результати: *establish, build, elevate, generate, turn into revenue*. Концепт-якорі: «emotional interfaces», «growth engine», «revenue machine». Жодного AI-кліше («unlock», «seamless», «empower»). Структура tier = ScrollCraft tier (Foundation/Flagship).

---

## ЩО РОБИТЬ AWARD-РІВНЕМ (уроки для нашого датасету)
1. **Шарова fixed-архітектура:** void-фон / параласт-типо / WebGL-canvas / контент — 4 z-шари, не «секції одна під одною».
2. **Порожній 1000vh scroll-драйвер** (`#studio-story`) — cinematic-преамбула героя перед контентом. Сміливо віддати 10 екранів чистому WebGL.
3. **Контент «наїжджає» знизу** з rounded-top — reveal з-під сцени.
4. **Horizontal-pin галерея послуг** (vertical→horizontal), 3 панелі = 3 tiers.
5. **3-шрифтова система ролей:** Cormorant (емоція) / Space Mono (точність) / Jost (UI).
6. **Тепла монохромна палітра** void×paper×gold, золото дозоване.
7. **Класичні скульптури × діджитал** — фірмовий візуальний мотив (мармур під фільтрами).
8. **Мікро-деталі-пагінація** ((01/03), hairline-роздільники, vertical-rl нав) — редакторська «дорожнеча».
9. **easeOutQuint 0.4s скрізь** — єдиний моушн-почерк.
10. **Mobile-форк:** окремий `md:hidden` блок зі статичними картками tiers замість horizontal-pin (важкий моушн — desktop-only). Точно як наш принцип «mobile fallback».

---

## МАПІНГ НА НАШ СТЕК / RECIPES
| Zera робить | Ми (Next.js+R3F) | Recipe |
|---|---|---|
| Fixed WebGL canvas (three r162) | R3F `<Canvas>` на рівні layout | R03 |
| Порожній 1000vh scroll-драйвер | ScrollTrigger pin + scrub на dummy-висоту | R04 |
| Типо-параласт по кутах | GSAP scrub на translateY/X шарів | R04 |
| Horizontal-pin галерея 3 панелі | ScrollTrigger pin + horizontal tween (x: -100%*n) | R04 (+R19 для grid) |
| Контент наїжджає з rounded-top | GSAP reveal + margin-top negative | R04/R11 |
| 3-шрифтова система | next/font (Cormorant+Space Mono+Jost) + CSS-vars | новий: type-system-3voice у toolbox |
| Vertical-rl бічна нав | CSS writing-mode + scaleY scroll-progress bar | новий мікро-патерн |
| Тепла монохром-палітра + gold | CSS-токени studio-* | новий: palette-warm-mono у toolbox |
| Класика×діджитал (мармур+фільтри) | Nano Banana Pro генерація скульптур + CSS filter | медіа-пайплайн |

---

## НОВІ ЕЛЕМЕНТИ ДЛЯ БАЗИ (приріст датасету з teardown #1)
- **Патерн «shell-layer architecture»**: void-bg / parallax-type / webgl-canvas / content-epilogue (4 z-шари) → у patterns/.
- **Патерн «empty scroll-driver»** (1000vh dummy для cinematic-преамбули) → recipes/R04 рівень B.
- **Патерн «3-voice type system»** (display-serif × mono-caps × thin-grotesk) → 00_foundations/toolbox.md.
- **Патерн «warm-mono palette + single gold accent»** → toolbox.md (палітра з характером, принцип #1).
- **Мікро-патерни**: gold-dot заголовок, hairline-роздільник «лейбл—лінія—•», vertical-rl нав зі scroll-progress, editorial-пагінація (01/03), gold-line list marker.
- **Копірайт-формула студії**: концепт-якір в display-serif + конкретні буліти в mono-caps, tier-структура Foundation/Flagship.
- **Підтвердження**: easeOutQuint(0.22,1,0.36,1)@0.4s як «дорогий» дефолт; Cormorant/Space Mono як анти-Inter вибір; mobile-форк важкого моушну.

## LAYER 3 — ВЕРИФІКОВАНО НА ЖИВІЙ СТОРІНЦІ (Claude in Chrome) ✅

### Точні токени (звірені на `[data-studio-immersive]`)
| Токен | Значення |
|---|---|
| `--studio-bg-void` | `#0a0a0a` |
| `--studio-bg-paper` | `#f4f1ec` |
| `--studio-ink-inverse` | `#f5f5f0` |
| `--studio-ink-primary` | `#0f0f0f` |
| `--studio-accent-gold` | `#b8a07a` |
| `--studio-accent-chrome` | `#c8c4bc` |

### Бібліотеки (точно)
- **Lenis** — підтверджено (`window.lenis` існує). Smooth-scroll = Lenis, не власний RAF. Опції не експонуються глобально.
- **three.js r162** (`canvas[data-engine]`). `window.THREE` = null → Three імпортований як ESM-модуль (bundled), canvas рендериться 1920×990 (DPR-scaled).
- **GSAP + ScrollTrigger** — bundled як ESM (не на window). Доказ: **2 `.pin-spacer`** у DOM (horizontal-галерея + Process-секція).
- **Next.js** (App Router) підтверджено. Лише **12 `<script src>`** (Next code-splitting).
- **5 шрифтів через next/font:** Cormorant Garamond (300/400/600/700 × normal+italic) · **Fraunces** (400/500/600 × normal+italic — друга display-антиква, варіативна!) · Space Mono · Jost · Inter (UI/body fallback).
  - ⚠️ Уточнення до типосистеми: не 3, а **2 display-антикви** (Cormorant + Fraunces) + mono (Space Mono) + grotesk (Jost) + Inter (службовий). Fraunces дає другий «голос» антикви.

### Геометрія скролу (точно)
- `body.scrollHeight` = **14893px** при `vh≈660–990` → сторінка ≈ **15–22 екрани**.
- `#studio-story` scroll-driver = **~10 екранів (1000vh)** cinematic-преамбули. Підтверджено.
- 3 `.h-panel` у horizontal-pin галереї. 2 пінованих блоки всього.

### ПОВНИЙ section-flow (фінальний, з Process + services-grid, яких не було в Apify-HTML)
1. **Hero** (fixed): "BUILT TO / BE FELT. / NOT JUST / SEEN." + WebGL r162 + 1000vh преамбула.
2. **Horizontal-pin галерея** (3 панелі = tiers): (01) Website & Design · (02) 3D Experiences · (03) Growth System.
3. **«Design-first. Everything else follows.»** — повноекранний типо-маніфест між блоками.
4. **PROCESS** (4 кроки, філософія «прибирати зайве» — нова знахідка):
   - `01 Clarity` — *"We strip the brief down to its core. What does this brand actually need to say — and to whom?"*
   - `02 Reduction` — *"Everything nonessential falls away. We remove until only what serves the experience remains."*
   - `03 Reconstruction` — *"From the refined foundation, the design takes shape — structure, rhythm, spatial logic."*
   - `04 Refinement` — *"Every surface is polished. Every transition considered. The final artifact emerges."*
5. **Services-grid (7 послуг)** — кожна: лейбл + 2-рядкова обіцянка caps:
   - UX Design — *"Crafting immersive interfaces that move people."*
   - Art Direction — *"Weaving art & design for visual storytelling."*
   - Marketing — *"Strategic campaigns that resonate and convert."*
   - AI Automations — *"Intelligent workflows that work while you sleep."*
   - Brand Identity — *"Creating meaningful brand worlds."*
   - Full Stack Development — *"Engineering for sleek, scalable experiences."*
   - SEO & GEO — *"Visibility engineering for search and discovery."* (зауваж «GEO» — Generative Engine Optimization, свіжий термін 2025-26)
6. **Footer-нав:** WORK | STUDIO | PORTAL | INQUIRE.

### Урок Process-секції (важливо для нашого копірайту)
Process = не «discovery→design→launch» (кліше), а концептуальна арка **Clarity→Reduction→Reconstruction→Refinement** — філософія мінімалізму («прибираємо, поки не лишиться суть»). Дієслово-наратив, не список послуг. Це і є антислоп-рівень process-секції.

## TODO (наступні кроки розбору Zera)
- /work, /about, /contact, /clientportal + 5 portfolio-піддоменів (як варіюється патерн під проєкти).
- Що саме в three.js canvas (модель/шейдер/частинки) — дістати через інспекцію сцени.
- Точні GSAP scrub-тайминги (читати ScrollTrigger.getAll() якщо вдасться дістати інстанс).
