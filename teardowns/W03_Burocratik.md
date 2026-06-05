# Bürocratik · Teardown #W3 — burocratik.com (Layer 3) ✅
> WEB-блок #3. Порту/Коїмбра, Португалія. CSSDA Best Studio, #1 Digital Design Studio Europe (EDA League). Засн. 2005.
> **Категорія:** WEB/brand/editorial. Проєкти: Floema, Jeton, Remote.

## СТЕК (probe)
- **Vanilla + GSAP + Lenis** (Lenis=true, gsapVersions=true). **10 canvas** (багато canvas-острівців для ефектів). **29 скриптів** (важкий, багатофункціональний).
- Шрифт: **Commercial Gräphik Web** ЄДИНИЙ (Commercial Type — преміум швейц. гротеск). Дисципліна одного шрифту. Прозора база.

## АНАТОМІЯ РУХУ (виміряно)
- Швидкий темп: 0.3s (11×), 0.2s (11×), 0.175s (4×), 0.4s (4×). Близько до японського (швидко-точно), хоч студія європейська.
- Easing-мікс: **`cubic-bezier(0.2,0,0,1)`** (10× — кастомний агресивний easeOut, різке гальмо) + **`cubic-bezier(0.77,0,0.175,1)`** (easeInOutQuart — драматичний симетричний) + easeOutCubic/easeInOutCubic.
- Багато складених тривалостей (0.3s,0.3s,0.3s) — синхронні групи.

### Емоція:
Швидкий (0.2-0.3s) + агресивний easeOut + 1 гротеск = чітко, енергійно, професійно-стримано. «Adaptable, technical» (зі слогану). Не драматична повільність, а вправна швидкість.

## СЕНСИ
- «**transdisciplinary**, branding and digital practice», «adaptable approach to good design and technology».
- «We are Büro» — колективна ідентичність (ми, не я).
- Акцент: адаптивність + технологія + критичне визнання.

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **Швидкий темп у європейській brand-студії** (0.2-0.3s) → motion-system (швидко ≠ лише японці; швидкість = професійна вправність).
- **Кастомний агресивний easeOut `cubic-bezier(0.2,0,0,1)`** (різке гальмо) → easing-бібліотека.
- **easeInOutQuart `cubic-bezier(0.77,0,0.175,1)`** (драматичний симетричний) → easing-бібліотека.
- **Один преміум-гротеск** (Commercial Gräphik) → toolbox.
- **10 canvas-острівців** (точкові ефекти, не повний WebGL) → patterns (WebGL дозовано, як острівці).

## TODO
- Далі #4 Robin Noguier.
