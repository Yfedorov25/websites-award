# Cuberto · Teardown #13 — cuberto.com (Layer 3 + MOTION ANATOMY) ✅
> Еталон #11. UI/UX product-агенція (засн. 2010). Відома кастомними курсорами + насиченим DOM-моушном.
> **Категорія:** UI/UX + motion (БЕЗ WebGL — весь рух на GSAP/CSS). Ніша: digital products, Web3, super-apps.

## СТЕК (probe)
- **Vanilla + GSAP + Lenis** (gsapVersions=true, Lenis=true). **canvas=0 — НЕ WebGL!** Весь моушн = DOM/CSS/GSAP transforms. 4 скрипти (легкий).
- Доказ: можна бути award-моушн-студією БЕЗ WebGL — лише майстерний GSAP+CSS. Важливий контраст до WebGL-еталонів.
- Шрифти: **Suisse Intl** (преміальний швейцарський гротеск) + **Manrope**. Біла база.
- **Кастомний курсор:** `.cb-cursor` (фірмова фішка — анімований курсор, що реагує/морфить).

## АНАТОМІЯ РУХУ (виміряно) ★★★
- **transform домінує:** 65× окремо + 30× `opacity+transform` + 143 активні matrix-трансформації. Рух = зсуви/масштаб (transform), НЕ clip-path (контраст до Tendril).
- **Тривалості:** домінанта **1.2s** (50×! — розкішно повільно) + пара **`0.4s, 0.6s`** (30× — РІЗНІ затримки для двох властивостей = вбудований stagger/каскад в одному елементі) + 0.8s (8×) + 0.3s (UI).
- **Easing:** **`cubic-bezier(0.16, 1, 0.3, 1)`** (53× — домінує) = **easeOutExpo** (миттєвий старт → дуже довге м'яке гальмо) + `cubic-bezier(0.19,1,0.22,1)` (експо-варіація).
- Патерн входу: **fade-up з easeOutExpo @1.2s** (opacity+transform). Елемент «вилітає» швидко й довго елегантно осідає.

### Емоція цього руху (протилежність Tendril):
- Tendril: симетрична S (easeInOut), clip-path шторка → вагоме, споглядальне.
- **Cuberto: easeOutExpo (асиметрична), transform-зсув → енергійне + елегантне.** Швидкий старт = жвавість/відгук; довге гальмо = преміальність/контроль. Доречно для UI/UX-агенції: демонструють «живий, чуйний інтерфейс».
- `0.4s,0.6s` різні затримки на властивостях = елемент рухається не «єдиним блоком», а розшаровано (позиція приходить раніше за прозорість) → органічність.

## КАСТОМНИЙ КУРСОР як емоційний прийом
- `.cb-cursor` реагує на ховери (морфінг, magnetic, текст усередині). Створює відчуття «сайт реагує на мене особисто» → залученість, гра. Фірмова деталь Cuberto, скопійована всією індустрією.

## СЕНСИ / КОПІРАЙТ
- Акцент на ВИКОНАННІ/точності: «no short cuts or simplifications», «exactly as during the design phase», «thoughtful design systems and carefully crafted development».
- «memorable websites» — емоційний результат.
- Ніша: scalable digital products, design systems, Web3 (Magma), super-apps (Punto Pago).

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **Award-моушн БЕЗ WebGL можливий** — майстерний GSAP+CSS+Lenis достатньо → philosophy (не кожен топ-сайт потребує 3D; контраст «спектакль через WebGL» vs «спектакль через бездоганний DOM-моушн»).
- **easeOutExpo `cubic-bezier(0.16,1,0.3,1)` @1.2s** — рецепт «енергійно+елегантно» → easing-бібліотека (парний до Tendril easeInOutCubic).
- **Розшарований stagger в одному елементі** (різні затримки на властивостях `0.4s,0.6s`) → motion-system (органічність руху).
- **Кастомний курсор** (.cb-cursor) як прийом залученості → components/emotion.
- **fade-up (opacity+transform) @expo** — найуніверсальніший entrance → toolbox (базовий рецепт).
- Suisse Intl + Manrope → toolbox типосистем (преміальний product-grotesk).

## ПОРІВНЯННЯ EASING (накопичуємо бібліотеку):
| Студія | Крива | Тип | Емоція |
|---|---|---|---|
| Tendril | cubic-bezier(0.65,0,0.35,1) | easeInOutCubic (симетрична S) | вагоме, споглядальне, театральне |
| Cuberto | cubic-bezier(0.16,1,0.3,1) | easeOutExpo (асиметрична) | енергійне старт + елегантне осідання |

## TODO
- Далі #12 Dogstudio.
