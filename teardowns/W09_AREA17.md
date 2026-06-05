# AREA 17 · Teardown #W9 — area17.com (Layer 3 + MOTION) ✅
> WEB-блок #9. Париж + NY. «Brand, experience, and technology transformation.» Клієнти: Saint Laurent, ElevenLabs, Fondation Cartier, OpenAI, IEA.
> **Категорія:** WEB/brand/editorial + technology consultancy.

## СТЕК (probe)
- **Vanilla + GSAP, БЕЗ WebGL** (canvas=0; gsapVersions=true; не React/Vue/Next/Nuxt). 5 скриптів (компактно). Lenis=false (свій/GSAP скрол).
- Шрифти: **Suisse Int'l** (швейц. гротеск) + **Suisse Works** (його антиква-компаньйон, Swiss Typefaces). Гротеск+антиква однієї суперродини = цілісність+контраст. Біла база.

## АНАТОМІЯ РУХУ (виміряно)
- **Двополюсний темп:** домінанта **1s** (71×!) для контентних появ + швидкі 0.3s (41×) і 0.15s (14×) для UI. Чіткий поділ: повільні драми / швидкі мікровзаємодії.
- Easing: **`cubic-bezier(0.33,1,0.68,1)`** (71× = easeOutCubic-варіація, плавне гальмо) для появ + **Material `cubic-bezier(0.4,0,0.2,1)`** (50×) для UI/кольору + `cubic-bezier(0.61,1,0.88,1)` (12×).
- Властивості: переважно `color/background/border` переходи (22×+) — багато hover-станів на тексті/лінках (типове для контентного сайту) + opacity+transform появи.

### Емоція:
1s появи + плавний easeOutCubic + Suisse гротеск/антиква + білий = впевнений, консультантський, «дорослий» преміум. Не спектакль, а авторитет. Темп говорить «ми серйозні партнери великих організацій».

## СЕНСИ / КОПІРАЙТ
- «We partner with the world's most **influential organizations** to realize their vision and achieve their greatest impact.»
- **«We're consultants and craftspeople»** — подвійна ідентичність (стратегія + ремесло). Сильний якір.
- «integrate brand, experience, and technology to create **lasting value**».
- Індустрії перелічені явно (AI, ClimateTech, Health, Fintech, Fashion/luxury, Hospitality, E-commerce) — показ галузевої експертизи.

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **«Consultants and craftspeople»** — позиція стратегія+ремесло → copywriting (подвійна цінність).
- **Двополюсний темп** (1s драми + 0.15-0.3s UI, без середини) → motion-system (чіткий поділ ролей швидкостей).
- **Suisse Int'l + Suisse Works** (гротеск+антиква суперродини) → toolbox типосистем (цілісна пара від одного словолитника).
- **Перелік індустрій явно** → patterns (показ галузевої експертизи для B2B-довіри).
- **Vanilla+GSAP без WebGL/фреймворку на топ-консалтингу** → ще одне підтвердження «стек під задачу» (B2B-консалтинг не потребує WebGL-спектаклю).

## TODO
- Далі #11 Build in Amsterdam.
