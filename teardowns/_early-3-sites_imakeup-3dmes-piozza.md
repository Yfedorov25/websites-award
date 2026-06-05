# ScrollCraft PRO · Deep Code Teardown (Apify-extracted)
> Реальний рендерений DOM трьох сайтів, витягнутий через Apify rag-web-browser.
> Тут — точна механіка: Tailwind-класи, easing, durations, transform-значення.
> Це доповнює 00_foundations/philosophy.md фактами з коду, а не здогадками.

---

## СПІЛЬНА АРХІТЕКТУРА (всі три)
- **Next.js + Tailwind** (хеш-класи, `/_next/image`, `data-nimg`).
- **CSS-змінні для теми** (`var(--color-ink)`, `bg-pizzaBg`, `text-blush-rose`).
- **Hero = `height:400vh` + `sticky top-0 h-screen`** контейнер з `<canvas>` всередині. Це і є frame-sequence scrub з PDF — підтверджено в imakeup і piozza.
- **Reveal-патерн скрізь однаковий:** елемент стартує `opacity:0; transform:translateY(20–48px)` і анімується в `opacity:1; transform:none` через `transition` зі stagger-затримками (`ease-out`, 0.9s, delay 0/120/220/300...ms). Це Intersection Observer + CSS transition (не обов'язково GSAP).

---

## 1. imakeup — точна механіка

### Preloader
```html
<div class="fixed inset-0 z-50 ... bg-ink" style="opacity:1">
  <p>Preparing your glow…</p>
  <div class="h-px w-56 ... bg-white/10">
    <div class="h-full bg-gradient-to-r from-blush-rose via-blush-peach to-blush-champagne"></div>
  </div>
  <p>0%</p>  <!-- лічильник оновлюється JS -->
</div>
```
- Градієнтна полоска прогресу `rose→peach→champagne`, лічильник `0%`.

### Hero frame-sequence (підтверджено)
```html
<section style="height:400vh">              <!-- 4 екрани скролу -->
  <div class="sticky top-0 h-screen overflow-hidden bg-ink">
    <canvas class="absolute inset-0 h-full w-full"></canvas>   <!-- drawImage кадрів -->
    <div class="... bg-gradient-to-b from-ink/40 via-transparent to-ink/60"></div> <!-- vignette -->
    <!-- 4 глави, кожна opacity:0 translateY(20px), fade на своєму offset -->
  </div>
</section>
```
- **Vignette-градієнт поверх canvas** (`from-ink/40 to-ink/60`) — робить текст читабельним. Важлива деталь "дорожнечі".
- `drop-shadow-[0_2px_30px_rgba(0,0,0,0.45)]` на заголовках глав.

### Hover-swap product cards (точні значення)
```
img base:  transition-transform duration-[1100ms] ease-out group-hover:scale-[1.08]
img over:  opacity-0 transition-opacity duration-700 group-hover:opacity-100
картка:    hover:shadow-[0_40px_80px_-30px_rgba(245,181,197,0.35)]
+ radial spotlight, що йде за курсором: background:radial-gradient(420px circle at Xpx Ypx, rgba(245,181,197,0.18), transparent 70%)
```
> Ключ: **1100ms scale + 700ms crossfade** + рожевий spotlight за мишею + кольорова тінь. Ось що дає "тактильність".

### Shade swatches (CSS-only, без зображень!)
Кожен відтінок — чистий CSS radial-gradient (не фото):
```
background:radial-gradient(circle at 35% 30%, #FBE7C2 0%, #EFD9B4 35%, #B89A6E 75%, #2a2118 100%)
+ mix-blend-overlay highlight + ring-inset ring-black/40 + glow на hover (blur-2xl)
```
> Урок: дорогі "продуктові" кружечки можна робити без ассетів — самим градієнтом.

### "Collection" hover-reveal (стопка фото з ротацією)
```
заголовок: group-hover:opacity-40 (тьмяніє)
фото 1: scale-0 group-hover:scale-100, opacity-0→100, delay-100
фото 2: + group-hover:translate-x-6 translate-y-6 rotate-12, delay-150
```
> Дві фотки "вилітають" з-за тексту зі зсувом і поворотом — преміальний мікро-момент.

### Press marquee
- Використано **бібліотеку marquee з progressive blur masks** (8 шарів `backdrop-filter:blur(0..7px)` з `mask-image` градієнтами по краях). Це фірмовий "blur fade" на краях стрічки. Логотипи — inline data-URI SVG.

---

## 2. 3dmes — точна механіка

### Hero
- `<video src="/me3d.mp4" autoplay loop muted playsinline>` фоном + градієнт-маска зліва `from-cream/85 via-cream/30 to-transparent` (текст читається).
- **Dot-grid текстура** через CSS: `radial-gradient(rgba(0,0,0,0.06) 1px, transparent 1.2px); background-size:24px 24px`.
- Заголовок: `reveal-line` + `reveal-delay-1/2` — кастомні класи (рядки виїжджають по черзі).
- Декор: `animate-float` (плаваючі картки), `animate-sway`, `animate-scroll-line`, пульсуюча точка `animate-pulse`.

### Marquee (точна реалізація)
```html
<div class="flex animate-marquee whitespace-nowrap">
  <div class="flex shrink-0 ...">...Design ✦ Develop ✦...</div>  <!-- блок 1 -->
  <div class="flex shrink-0 ...">...повтор...</div>              <!-- блок 2 -->
  <div class="flex shrink-0 ...">...повтор...</div>              <!-- блок 3 -->
</div>
```
> **3 ідентичні блоки** + CSS `@keyframes animate-marquee { to { transform: translateX(-33.33%) } }` = безшовний нескінченний цикл. Ось точний рецепт seamless marquee.

### Reveal stagger (точні значення)
```
style="opacity:0; transform:translateY(28px);
       transition:opacity 0.9s ease-out 0ms, transform 0.9s ease-out 0ms"
```
Затримки по черзі: 0 → 120 → 220 → 300 → 400 → 500ms. **28px / 0.9s / ease-out** — їхній фірмовий reveal.

### Expanding hover lanes (декодовано!)
```
рядок: h-[450px] w-[60px] transition-all duration-700 ease-in-out
       (активний стає w-[400px])
текст: rotate-90 (вертикальний) коли вузький → rotate-0 коли широкий
```
> 5 "доріжок", ширина `60px → 400px` за 700ms, лейбл повертається з вертикалі в горизонталь. Це той самий патерн, що в imakeup-колекції, але через width-transition.

### Spline playground
- `<canvas style="display:none">` всередині — Spline-сцена (Whobee), drag-controls.

### Контакт
- Glow-куля: `radial-gradient(circle, rgba(232,155,106,0.45), transparent 65%); filter:blur(40px)`.
- Акцент-слово: `<em class="not-italic text-accent">making real</em>`.

---

## 3. piozza — точна механіка

### Grain overlay (фірмова фішка)
```html
<div class="grain-overlay"></div>  <!-- глобальний, поверх усього -->
```
> Окремий клас `grain-overlay` — плівкове зерно на весь сайт. Миттєвий кінематографічний шар (наш патерн #22).

### Hero (підтверджено frame-sequence)
```html
<div class="relative h-[400vh]">
  <preloader: "Heating Oven" + полоска bg-orange-500 width:0%→100%>
  <div class="sticky top-0 h-screen overflow-hidden">
    <canvas class="w-full h-full object-contain" style="opacity:0"></canvas>
    <!-- 4 текстові біти, кожен absolute inset-0, opacity:0 translateY(20px): -->
    <!-- 1. THE PERFECT SLICE (center) -->
    <!-- 2. TIME IS AN INGREDIENT (left) -->
    <!-- 3. PUREST PROVENANCE (right) -->
    <!-- 4. TASTE THE TRADITION (center) + кнопка -->
  </div>
</div>
```
> **Біти чергують вирівнювання** center→left→right→center — кінематографічний ритм, око "танцює".

### Signature pizzas — drag-scroll карусель
```
контейнер: overflow-x-auto cursor-grab active:cursor-grabbing
картка: rounded-[40px] transition-all duration-500 hover:border-white/20
        transform-style:preserve-3d (готовність до 3D-tilt)
glow: bg-pizzaRed/10 blur-[60px] rounded-full (кутове світіння)
зображення: next/image srcset до 3840w, q=75, drop-shadow-[0_20px_40px_rgba(0,0,0,0.5)]
```

### 🔥 Interactive Lab (configurator — повна механіка)
```
ROZMIR: 3 кнопки S/M/L; активна = bg-pizzaRed scale-110 shadow-[0_0_20px_rgba(185,50,51,0.4)]
ТОПІНГИ: кнопки з next/image іконкою + ціна (+$2/+$3); toggle-стан border + checkbox-кружок
ЦІНА: live "Total Build $15" (React state, перераховується)
PIZZA PREVIEW: CSS-кола (border-8, shadow inner) + glow inset-10 bg-pizzaRed/40 blur-2xl
              opacity:0 scale:0 → анімується при додаванні топінгів
КНОПКА: two-layer text reveal — "Prepare Order" (translate-y-0) ↔ "Cooking..." (translate-y-16)
        переключається transform translateY за 300ms + спінер border-t-transparent
```
> Це найдорожчий елемент. Ключ: **live state + dual-layer button (текст міняється зсувом по Y)** + спінер.

### Story cards + quote
- Картки: `opacity:0 translateY(30px)` reveal, hover `bg-white/[0.04]`, кольорові іконки-плашки.
- Цитата: `text-5xl font-light italic`, `tracking-[0.4em]` на підписі.

### Final "rotating pizza"
```
фон-зображення: opacity:0.15 scale(0.8) blur-[2px], w-[120vw]
                (обертається/масштабується на scroll — rotate linked to progress)
заголовок: ONE BITE AND YOU'RE HOOKED (text-9xl, leading-[0.85])
```
- Floating "back to top": `fixed bottom-8 right-8 w-16 h-16 bg-pizzaRed`, `opacity:0 scale:0` → з'являється на scroll.

---

## 4. НОВІ ПАТЕРНИ, ДОДАНІ ДО БІБЛІОТЕКИ (з коду)

| # | Патерн | Точний рецепт |
|---|---|---|
| 29 | **Cursor spotlight на картці** | `radial-gradient(420px circle at mouseX mouseY, accent/0.18, transparent 70%)`, opacity 0→1 на hover |
| 30 | **Vignette over canvas** | градієнт `from-ink/40 via-transparent to-ink/60` поверх hero-canvas для читабельності тексту |
| 31 | **CSS-only product swatch** | radial-gradient замість фото + mix-blend-overlay highlight + ring-inset |
| 32 | **3-block seamless marquee** | 3 ідентичні блоки + `translateX(-33.33%)` keyframe |
| 33 | **Width-expand lanes** | `w-[60px]→w-[400px]` 700ms + label `rotate-90→rotate-0` |
| 34 | **Dual-layer button** | два текст-шари, активний `translate-y-0`, прихований `translate-y-16`, свап на стан |
| 35 | **Alternating hero beats** | вирівнювання текст-бітів center→left→right→center для ритму |
| 36 | **Progressive blur marquee edges** | стопка `backdrop-filter:blur(0..7px)` з `mask-image` градієнтами |
| 37 | **Global grain-overlay** | один фіксований div на весь сайт |
| 38 | **Dot-grid atmosphere** | `radial-gradient(rgba 1px, transparent 1.2px); size:24px` |

---

## 5. ГОЛОВНІ УРОКИ ДЛЯ НАШОГО БІЛДУ

1. **Hero = `400vh` + `sticky h-screen` + canvas** — це стандарт, підтверджений 2/3 сайтів. Будуємо так само.
2. **Reveal: 28px / 0.9s / ease-out / stagger 100-120ms** — золоті значення, беремо як дефолт.
3. **Vignette/маска над медіа — обов'язкова** для читабельності (не опційна).
4. **Grain overlay глобально** — дешево додати, миттєво "дорожче".
5. **Live configurator** — найсильніший "$10k" елемент; dual-layer button + live total.
6. **CSS-градієнти замість фото** де можливо (swatches, превʼю) — швидкість + чистота.
7. **next/image srcset + q=75** скрізь — performance як частина преміальності.
8. Ці сайти **НЕ використовують важкий WebGL** (крім Spline-іграшки). Весь "вау" — на canvas frame-sequence + CSS transitions + Tailwind. Тобто наш PDF-стек достатній; WebGL/шейдери (Three.js) — це наш апгрейд НАД ними, щоб бути ще дорожче.
