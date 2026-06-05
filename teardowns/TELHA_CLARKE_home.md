# Telha Clarke · Teardown #2 — Homepage (Deep / Layer 3 verified) ✅
> URL: https://telhaclarke.com.au/ · Розбір повністю через Claude in Chrome (live DOM, токени, бібліотеки, копірайт).
> **Архітектурно-дизайнерська студія, Мельбурн.** Навмисний контраст до Zera: преміальність БЕЗ WebGL/3D.
> Головний урок цього еталона: **дорожнеча через стриманість** — типографіка + фото + grid + повільний темп, нуль cinematic-важкості.

---

## META / ПОЗИЦІОНУВАННЯ
- **title:** "Telha Clarke — Melbourne based Architecture & Interior Design studio"
- **desc:** "Melbourne based architecture and interior design studio working across diverse project types in Australia and internationally."
- Hero-маніфест: **"Driven by History, Centered on Context, Embracing Culture"**
- Філософія: *"We believe human touch must drive creativity."*

---

## СТЕК (Layer 3, точно) ★ контраст із Zera
| Технологія | Доказ |
|---|---|
| **Tailwind CSS v4** | CSS-змінні `oklch(...)`, `--color-zinc-*`, `--container-md`, `--width-col-offset-3`, `--aspect-video` — характерний відбиток Tailwind v4 `@theme` |
| **Lenis** | `<div class="lenis lenis-stopped">` — wrapper/content режим (scroll на контейнері, НЕ на body; scrollHeight 9148px) |
| **БЕЗ WebGL / three.js** | `hasCanvas: false`. Жодного 3D. |
| **БЕЗ GSAP на window** | або немає, або мінімальний bundled. Анімація — переважно CSS transitions |
| **Лише 2 `<script src>`** | гранично легкий фронт |
| **1 шрифт: Europa-Grotesk** | єдина гарнітура на весь сайт |
| Фреймворк | не Next/Nuxt/Vue/Webflow/Framer — ймовірно кастомний білд (Vite + Tailwind v4 + Lenis) або статика |

**Урок про стек:** award-преміальність досяжна на стеку Tailwind+Lenis+CSS-transitions, **без жодного WebGL**. Це прямий доказ, що наша база не має зводити «дорого» до «важкий 3D». Для утилітарних/спокійних ніш (архітектура, юридичні, консалтинг) це і є правильний легкий шлях (перегукується з Pipeline 0.0 «утилітарний → cinematic дозовано»).

---

## ПАЛІТРА — мінімалістична монохром-тріада
| Колір | Значення | Роль |
|---|---|---|
| Чорний | `rgb(0,0,0)` | основний фон / текст |
| Білий | `rgb(255,255,255)` | інверсні секції / текст на чорному |
| Шавлієво-сірий | `#cacfcb` (`rgb(202,207,203)`) | третій тон — приглушений сіро-зелений (single accent) |

Жодного яскравого кольору. Як у Zera — монохром + один приглушений акцент, але тут акцент **холодний сіро-зелений** (камінь/бетон — архітектурна асоціація), а не тепле золото. **Принцип спільний, виконання нішеве.**

---

## ТЕМП / МОУШН ★
- **`--transition-duration-smooth: .7s`** — фірмовий темп переходів. **Повільніший за Zera (0.4s).**
- Урок: темп моушну = характер ніші. Агенція = жвавий 0.4s (енергія, «дивись що ми вміємо»). Архітектура = повільний 0.7s (вага, спокій, споглядання). **Той самий принцип «reveal-темп під нішу» з нашого START_HERE #2, підтверджений на парі агенція vs архітектура.**
- Моушн зроблено на CSS-transitions + Lenis smooth-scroll, без scroll-jacking-сторітелінгу. Спокійний вертикальний скрол.

---

## GRID-СИСТЕМА — 12-колонкова з offset-утилітами ★★
Ключова знахідка. Контент позиціонується зсувами на 12-колонковій сітці:
```css
--width-col-offset-3: calc((1280px - 2rem*2) / 12 * 3);
--container-md: 28rem; --container-xs: 20rem;
```
- Базова ширина 1280px, 2rem зовнішні відступи, поділена на 12 колонок.
- `col-offset-N` = ширина N колонок — використовується для **асиметричних відступів** (текст починається не з краю, а зі зсувом на 3 колонки тощо).
- Це **той самий редакторський grid-принцип, що в Immersive Garden** (width__N/position__N), але реалізований через CSS-змінні Tailwind v4, а не утиліти-класи.
- **Урок:** асиметричні текстові блоки на 12-grid зі зсувами = редакторська «дорожнеча». Текст ніколи не на всю ширину — завжди в колонці зі зсувом. Це читається як друкований журнал.

---

## СТРУКТУРА СТОРІНКИ (section-flow)
1. **Hero:** `[Scroll down]` + маніфест "Driven by History, Centered on Context, Embracing Culture" (двічі — ймовірно дубль для анімації появи/маски).
2. **(01) STUDIO:**
   - **Acknowledgement of Country** першим блоком (!) — *"We connect, create and work on the lands of Aboriginal and Torres Strait Islander peoples... we pay our respect to Elders past and present."* — культурний/етичний жест попереду продажу. Сильний нішевий сигнал (австралійський архітектурний контекст).
   - Опис студії + типології: *"...single & multi-residential, seniors living, student accommodation, social housing, hotels, hospitality and workplace."*
3. **(02) SELECTED WORKS `17-25'`:** список проєктів у форматі `[Назва] / Типологія / Рік`:
   - `[Loller]` Multi-residential 2025 · `[Penthouse Vivace]` Residential 2025 · `[Southbank Tower]` Multi-... (32 зображення проєктів).
4. **Process / Studio / Contact** (нав: Work / Process / Studio / Contact + Instagram / Linkedin).

---

## КОПІРАЙТ — архітектурний регістр (антислоп, інша ніша) ★
- **Hero — тріада через коми:** "Driven by History, Centered on Context, Embracing Culture" — три герундії, паралельна конструкція, абстрактні цінності (історія/контекст/культура). Жодної згадки про послугу — чиста філософія.
- **Acknowledgement of Country** — етичний жест перед бізнесом (нішево-локальне, австралійський стандарт).
- **Назви проєктів у квадратних дужках:** `[Loller]`, `[Penthouse Vivace]`, `[Southbank Tower]` — типографічний прийом (дужки = «технічна каталогізація», редакторськість).
- **Лейбли-каталог:** `SELECTED WORKS`, `17-25'`, `(01) STUDIO`, `(02)` — мінімалістична нумерація (як у Zera (01/03), але сухіша).
- Тон: спокійний, тяжкий, культурно-вкорінений. Дієслова стану/цінності (driven, centered, embracing, believe), не дієслова-продажу (generate, convert). **Протилежність агенційному «revenue machine».**

**Урок копірайту:** регістр ніші = регістр цінностей клієнта. Агенція продає РЕЗУЛЬТАТ (convert, revenue, leads). Архітектура продає ФІЛОСОФІЮ (history, context, human touch). Обидва антислоп, але різні словники.

---

## ZERA vs TELHA CLARKE — таблиця контрастів ★★ (головна цінність teardown #2)
| Вимір | Zera (агенція) | Telha Clarke (архітектура) |
|---|---|---|
| WebGL/3D | three.js r162, fixed canvas, 1000vh преамбула | **немає взагалі** |
| Стек | Next.js + R3F + GSAP + Lenis | Tailwind v4 + Lenis + CSS-transitions |
| Скрипти | 12 | **2** |
| Шрифти | 5 (2 антикви + mono + grotesk + Inter) | **1** (Europa-Grotesk) |
| Темп | 0.4s (easeOutQuint, жваво) | **0.7s** (повільно, споглядально) |
| Палітра | void-чорний × paper-кремовий × **тепле золото** | чорний × білий × **холодний шавлієвий** |
| Скрол | scroll-jacking сторітелінг, horizontal-pin | спокійний вертикальний (Lenis) |
| Grid | панелі h-panel | 12-col offset (редакторський) |
| Копірайт | «emotional interfaces», «revenue machine» | «history, context, culture», «human touch» |
| Стратегія дорожнечі | **cinematic спектакль** | **стримана типографіка** |

**ГОЛОВНИЙ УРОК:** є щонайменше **дві ортогональні стратегії award-преміальності**:
- **A. Спектакль** (Zera/Lusion/IG): WebGL, 3D, scroll-сторітелінг, багата типосистема. Для емоційних ніш, де треба «вау».
- **B. Стриманість** (Telha Clarke): один шрифт, монохром, повільний темп, ідеальний grid, фото. Для спокійних/культурних ніш, де «вау» = несмак.
Наша база мусить тримати ОБИДВІ як рівноправні, а не вважати, що «дорого = більше ефектів». Вибір A/B диктує **тип ніші** (Pipeline 0.0 емоційний/утилітарний), а не рівень майстерності.

---

## НОВІ ЕЛЕМЕНТИ ДЛЯ БАЗИ (приріст із teardown #2)
- **Дихотомія стратегій «Спектакль ↔ Стриманість»** → у philosophy.md як рамка вибору (поряд з емоційний/утилітарний).
- **Патерн «12-col offset grid через CSS-vars»** (Tailwind v4 `calc((W - pad)/12 * N)`) → toolbox.md (редакторський лейаут без важких залежностей).
- **Підтвердження «темп під нішу»**: 0.4s агенція ↔ 0.7s архітектура — конкретні числа в START_HERE #2 / philosophy.
- **Патерн «Lenis на wrapper-контейнері»** (не на body) → recipes (варіант інтеграції Lenis).
- **Single-font преміальність** як легітимний напрям (анти-надмірність) → toolbox типосистема: не лише «3-5 voices», а й «1 voice, ідеально набраний».
- **Копірайт-регістр архітектури**: цінності-герундії, дужки-каталог, Acknowledgement-жест → niche-profiles (нова ніша: architecture/interior).
- **Легкий award-стек** (Tailwind v4 + Lenis + CSS-transitions, 2 скрипти) → доказ для утилітарних ніш у Pipeline 0.0.

## TODO
- Telha: розбір /work (як подають проєкти — галерея), /process.
- Перевірити: чи є на проєктних сторінках hover-reveal / image-displacement (тоді GSAP таки є локально).
