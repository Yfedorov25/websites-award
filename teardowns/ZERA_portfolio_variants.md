# Zera Studio · Teardown #3 — Екосистема портфоліо (6 варіантів, Layer 3) ✅
> Розбір 5 portfolio-піддоменів + /webdeveloper через Claude in Chrome (live DOM, libs, fonts, palette).
> **Мета:** побачити, як ОДНА студія варіює скелет під різні задачі/клієнтів/естетики.
> Головна цінність: це не 6 різних сайтів, а **6 перевдягань одного інженерного скелета**.

---

## ТАБЛИЦЯ ВАРІАНТІВ
| # | Проєкт | Тип | WebGL | Шрифти | Палітра | Lenis | Скриптів | Особливе |
|---|---|---|---|---|---|---|---|---|
| **home** | Zera Studio | агенція (себе) | three.js r162 | Cormorant+Fraunces+Space Mono+Jost+Inter | void×paper×**gold** | ✓ | 12 | 1000vh scroll-driver, horizontal-pin, Process-арка |
| **p4** | Jerry | dev-портфоліо (демо) | canvas (bundled) | **Amiamie + Amiamie-Round** | gold `#cfa355`×primary `#e5e5e0`×black | ✓ | 1 | грайливий round-гротеск; «Behind the scene, beyond the screen» |
| **p5** | Drift Field | WebGL-експеримент | three.js + **Leva** | **Bebas Neue + Barlow Condensed + IBM Plex Mono** | bg `#1a1816`×accent `#c8b89a` | ✗ | 9 | конденсована «технічна» система; debug-панель Leva |
| **p8** | Detail Driven Niagara | авто-детейлінг (клієнт) | canvas | **Instrument Serif + Inter + Outfit** | bg `#080808`×**gold `#c5a55a`** | ✓ | 2 | «Perfection, Refined.»; Process-стадії Wash/Polish; kinetic vertical letters |
| **p9** | Zera flythrough | 3D-проліт (демо) | three.js + **Leva** | **DM Serif Display + Inter** | (3D-сцена) | ✗ | 1 | повноекранний WebGL camera-flythrough по скролу |
| **/webdeveloper** | Award-Winning Web Developer | sales-лендинг курсу | немає (швидкість) | Cormorant+Fraunces+Space Mono+Jost+Inter+**Playfair** | (як home) | ✓ | 22 | продаж інфопродукту «Build $5K-$20K sites» |

---

## ГОЛОВНІ ВИСНОВКИ ★★★

### 1. Спільний інженерний скелет, різні «шкіри»
Наскрізні константи студії на ВСІХ сайтах:
- **Lenis** як дефолт smooth-scroll (крім чисто-WebGL flythrough-демо p5/p9, де камера керує скролом сама).
- **Антиква-display × гротеск × (іноді) моно** — формула типоконтрасту, але конкретні гарнітури щоразу інші.
- **Темна база + один теплий металік-акцент** (gold домінує: home, p4, p8; варіації — bronze/sage).
- **Process/stages-секція з нумерацією** (home: Clarity→Reduction→Reconstruction→Refinement; p8: Wash→Polish→...). Повторюваний модуль.
- **WebGL опційний** — додається коли треба «вау» (home, p5, p8, p9), прибирається коли треба конверсія/швидкість (/webdeveloper).

**Урок для нас:** треба будувати не «сайти», а **один модульний скелет** (Next + Lenis + R3F-опційно + типосистема-на-CSS-vars + Process-модуль + tier-модуль), який перевдягається зміною шрифтів/палітри/контенту. Саме так студія масштабується. Це прямо лягає в наш принцип «кожна техніка модульна» (START_HERE #10).

### 2. Кожен проєкт = власна типосистема (НЕ нав'язана студійна)
6 сайтів = 6 різних шрифтових наборів (Amiamie / Bebas+Barlow / Instrument / DM Serif / Cormorant+Fraunces...). Студія НЕ ставить свій шрифт усім. **Айдентика проєкту > айдентика студії.** Урок: у нашій базі типосистема має бути параметром проєкту, а не глобальною константою.

### 3. WebGL — це інструмент, а не самоціль (підтвердження дихотомії з teardown #2)
- p5/p9 — чистий WebGL-спектакль (демо можливостей).
- p8 — WebGL дозовано (клієнтський бізнес, акцент на конверсію).
- /webdeveloper — 0 WebGL (sales-лендинг, бо тут гроші роблять текст і швидкість, не 3D).
Це третє підтвердження: **рівень cinematic диктує задача сторінки**, не «крутість». Sales-сторінка курсу про дорогі сайти — сама БЕЗ 3D. Промовисто.

### 4. /webdeveloper — дзеркало нашої бізнес-моделі ★
Zera продає курс «Build $5K to $20K+ Websites Without Coding» — це **та сама модель, що ScrollCraft PDF**. Структура їхньої sales-воронки (бери як шаблон для власного позиціонування/лендингу):
hero-offer → **Proof this actually works** → **What You Learn** → CTA → **Why this works (why most stay stuck)** → «The difference isn't tools. It's execution.» → **module by module** → **Why this isn't another course** → CTA.
Меседж-якір: *«The difference isn't tools, it's execution»* — точно як ScrollCraft *«The difference isn't the AI, it's the craft»*. Однакова теза.

---

## КОПІРАЙТ-ЗНАХІДКИ (по варіантах)
- **p4 Jerry:** «ZERA STUDIO HELPS GROWING BRANDS SHIP PREMIUM, RESULTS-DRIVEN WEB EXPERIENCES WITH CLARITY, SPEED, AND CRAFT» / «Behind the scene, beyond the screen».
- **p8 Detail Driven:** «Perfection, Refined.» / Process-тріади: «Strip. Cleanse. Reveal.» / «Every contaminant dissolved. Every pore opened. The surface speaks only when it's truly bare.» — короткі ударні фрази, дієслова-команди.
- **/webdeveloper:** «Build $5K to $20K+ Websites Without Coding» / «The difference isn't tools. It's execution.» / «Why this isn't another course».
**Спільне:** короткі ударні рядки + тріади команд-дієслів + якірний контраст («не X, а Y»). Це фірмовий копірайт-почерк студії.

---

## НОВІ ЕЛЕМЕНТИ ДЛЯ БАЗИ (приріст із teardown #3)
- **Концепт «модульний скелет + змінна шкіра»** → philosophy/patterns: будуємо переодягуваний скелет, не разові сайти. Параметри шкіри: типосистема, палітра, наявність WebGL, темп.
- **Модуль «Process/Stages-секція»** (нумеровані кроки з мікрокопі) → components-каталог. Повторюється на home+p8.
- **Модуль «Tier-секція»** (3 рівні пропозиції) → components.
- **Принцип «типосистема = параметр проєкту»** → toolbox (не глобальний шрифт).
- **Принцип «WebGL опційний шар»** — вмикається/вимикається залежно від задачі сторінки (спектакль vs конверсія) → philosophy дихотомія (3-тє підтвердження).
- **Leva** як dev-tool для R3F-debug → toolbox (інструмент розробки 3D-сцен).
- **Sales-воронка інфопродукту** (структура /webdeveloper) → business/ або окремий шаблон лендингу, якщо робитимемо власний продаж.
- **Бібліотека шрифтових наборів Zera** (6 готових пар/тріо) → toolbox як референс-палітра типосистем:
  - Round-грайливий: Amiamie
  - Технічно-спортивний: Bebas Neue + Barlow Condensed + IBM Plex Mono
  - Преміум-класика: Instrument Serif / Cormorant+Fraunces / DM Serif / Playfair + Inter/Outfit

## TODO
- p8 Detail Driven — найближчий до реального клієнтського білда; вартий окремого глибокого розбору механіки (kinetic letters, Process-scroll).
- (Крок Б) Розширити вибірку поза Zera — award-агенції зі світу для балансу датасету.
