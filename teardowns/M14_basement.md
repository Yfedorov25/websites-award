# basement studio · Teardown #17 — basement.studio (Layer 3 + MOTION) ✅
> Еталон #15. Аргентина/NY. «We make cool shit that performs.» Хостить Studio Freight (автори Lenis).
> **Категорія:** MOTION/WebGL + branding. Performance-орієнтований почерк.

## СТЕК (probe)
- **WebGL2, 3 canvas** (як Lusion — multi-layer). 10 скриптів. React/Next probe=false (ймовірно Next з кастомним рендером, глобали сховані). Lenis bundled (вони автори, на window не видно).
- Шрифти: **Geist + Geist Mono** (шрифт Vercel — сучасний техно-гротеск) + **flauta** (дисплейний). Чорна база.

## АНАТОМІЯ РУХУ (виміряно) — НАЙШВИДША З УСІХ ★★★
- Домінанта **0.3s** (43×) + надзвичайно швидкі **0.06s** (35×!) + 0.1s/0.2s. Майже немає повільних (0 × 1.2s!).
- Easing: **`cubic-bezier(0.4, 0, 0.2, 1)`** (52× = **Material Design standard easing**) + **linear** (35× — миттєві/механічні зміни, hover-стани).
- Властивості: opacity(40×)+transform(9×)+mask-position(1× — маскові ефекти).

### Емоція (performance-почерк):
0.06-0.3s + Material easing + linear = **snappy, реактивний, «нічого не гальмує»**. Інженерний почерк: рух підкреслює швидкість/чіткість продукту, не милується собою. Відповідає слогану «cool shit that **performs**». Протилежність повільним європейцям (Cuberto 1.2s) і близько до японців (GE 0.3-0.6s), але ще різкіше.

### Нове розуміння — рух як заява про цінності:
- Tendril (1s, вагомо) = «ми робимо дороге кіно».
- Cuberto (1.2s expo) = «ми робимо елегантні продукти».
- basement (0.06-0.3s, Material+linear) = «ми робимо швидке, що працює».
→ Темп руху = декларація позиціонування студії. Рух «говорить» цінності ще до тексту.

## СЕНСИ / КОПІРАЙТ
- «We make **cool shit that performs**» — зухвалий тон + performance-акцент (рідкісне поєднання грайливості й інженерності). «branding powerhouse».
- Тон: зухвалий/прямий (контраст до серйозного Phantom, врочистого Tendril).

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **Material easing `cubic-bezier(0.4,0,0.2,1)` + linear для мікро** — snappy-почерк → easing-бібліотека.
- **Ультрашвидкий темп 0.06-0.3s** = реактивність/performance → motion-system (найшвидший полюс).
- **Рух як декларація цінностей** (темп говорить позиціонування) → philosophy (вибір темпу = стратегічне рішення, не естетичне).
- **Geist (Vercel font)** → toolbox типосистем (сучасний техно-default).
- **linear для мікро-станів** (hover) — інженерна деталь → motion-system.

## TODO
- Далі #16 Antinomy Studio.
