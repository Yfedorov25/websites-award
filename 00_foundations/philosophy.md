# ScrollCraft PRO · Reference Analysis & Pattern Library
> Глибокий розбір топових сайтів + патерни, що створюють відчуття сайту на $10k+
> Цей файл — частина "мозку" проекту. Claude Code читає його перед побудовою будь-якої секції.

---

## 0. ФІЛОСОФІЯ: чому одні сайти "коштують" $500, а інші — $10k

Різниця **не в технології** (всі юзають GSAP + Three.js), а в **диригуванні**:

1. **Один сильний момент важливіший за 10 слабких.** Один ідеально поставлений scroll-reveal героя > 20 дрібних мікроанімацій всюди.
2. **Темп (pacing) — це все.** Дорогі сайти "дихають": момент напруги → момент спокою. Дешеві — анімують усе одночасно і втомлюють.
3. **Континуальність.** Перехід між секціями ніколи не "рветься" — об'єкт/камера/світло перетікають. Lenis (smooth scroll) — фундамент цього відчуття.
4. **Матеріальність.** Світло, тіні, grain, depth-of-field, bloom на правильних об'єктах. Це те, що мозок читає як "дорого".
5. **Стриманість тексту.** Менше слів, більше повітря. Типографіка — головний актор, а не декорація.
6. **Завантаження як перформанс.** Preloader (0→100%) — це не "очікування", це перша емоція й обіцянка якості.

> Правило ScrollCraft PRO: якщо анімація не викликає мікро-вдиху ("вау"), вона або підсилюється до кінематографічної, або видаляється. Середини не існує.

---

## 1. imakeup.netlify.app — "Blushlighters" (beauty/product)

**Жанр:** преміум б'юті-продукт, темна кінематографічна естетика (`#050505`).

### Структура (декодована)
- **Preloader:** `Preparing your glow… 0%` — лічильник, що тримає увагу й маскує завантаження ассетів.
- **Chaptered hero:** 4 "глави" (Chapter 01–04) — це scroll-controlled frame sequence у дії: "Blur, perfected" → "Micro-milled pigments" → "Filter-like diffusion" → "Your skin, but softer". Кожна глава = один кадр/стан анімації, текст фейдиться синхронно зі скролом.
- **Product line (hover-to-feel):** картки з двома зображеннями (`Highlighter1.jpg` / `Highlighter2.jpg`) — hover свопить текстуру. Класичний crossfade/displacement на hover.
- **Shades swatch:** 5 гармонізованих відтінків, кожен з мікроописом — interactive hover reveal.
- **"Hover to peek" ритуал:** ще один hover-reveal патерн (двошарові зображення).
- **Editorial press logos:** inline SVG логотипи (Vogue, Elle...) — соціальний доказ як data-URI SVG (швидко, без HTTP-запитів).
- **Final CTA:** "A new ritual for soft-focus skin" + "Shop the palette".

### Патерни, які крадемо
| Патерн | Як працює | Цінність |
|---|---|---|
| **Numbered chapters в hero** | Скрол-контрольована послідовність кадрів, текст міняється на ключових offset | Перетворює hero у міні-фільм |
| **Hover-swap product cards** | 2 зображення, друге `opacity:0→1` + scale на hover | Тактильність, "хочеться торкнутись" |
| **Lichильник preloader** | `0%→100%` поки вантажаться WebP-послідовності | Перша емоція + технічна необхідність |
| **Inline SVG логотипи** | data-URI, нуль запитів | Performance + редакторський лоск |

---

## 2. 3dmes.netlify.app — "ME" (3D портфоліо / personal brand)

**Жанр:** дизайнер-портфоліо з 3D-аватаром (Spline), теплий, "живий".

### Структура (декодована)
- **Video hero:** `/me3d.mp4` фоном — це готовий рендер (наш Kling-output), зашитий у hero.
- **Split headline:** "ME: Crafting **Digital Wonders**." — частина тексту акцентна.
- **3D Avatar** + "Scroll" індикатор.
- **Marquee стрічка:** `Design ✦ Develop ✦ Animate ✦ Ship ✦ Iterate ✦` — нескінченний горизонтальний рух (infinite marquee, GSAP/CSS). Повторюється 3 рази для безшовності.
- **About зі статистикою:** `08+ Years / 60+ Projects / ∞ Coffee` — count-up на scroll-into-view.
- **"What I do" lanes (hover to peek):** 5 горизонтальних "доріжок" (Brand / Web / 3D / Product / Creative Code), hover розкриває зображення з Unsplash. Це **expanding rows / accordion-on-hover** патерн.
- **Selected Work:** список проєктів з тегами (`Web · Motion`, `Product · UX`) + hover-reveal "→".
- **Spline playground:** "Meet Whobee — drag him around" — інтерактивна 3D-сцена (drag-controls).
- **Big CTA:** "Got an idea worth *making real*?" — курсив-акцент.

### Патерни, які крадемо
| Патерн | Як працює | Цінність |
|---|---|---|
| **Infinite marquee** | Дубльована стрічка, `translateX` лінійно, seamless loop | "Живий" ритм, заповнює простір елегантно |
| **Count-up on scroll** | ScrollTrigger → tween числа 0→target | Робить статистику кінематографічною |
| **Expanding hover rows** | Рядки тексту, hover → розкриває зображення/висоту | Інтерактивний, "магазинний" feeling |
| **Spline drag-toy** | Вбудована інтерактивна 3D-сцена | Запам'ятовується, "грається" |
| **Italic accent words** | Окремі слова курсивом/іншим вагою | Редакторський, дорогий тон |

---

## 3. piozza.netlify.app — "PIOZZA" (food / e-commerce, Next.js)

**Жанр:** артизанальна піца, теплий преміум, Next.js (`/_next/image`).

### Структура (декодована)
- **Preloader:** "Heating Oven" + "Scroll to Taste" — тематичний лоадер (не generic "Loading").
- **Hero:** "THE PERFECT SLICE / Crafted by hand. Baked in seconds." — рядки з `<br>`, staggered reveal.
- **3-step scroll story:** "TIME IS AN INGREDIENT" → "PUREST PROVENANCE" → "TASTE THE TRADITION" — scroll-pinned секції зі зміною контенту (pin + scrub).
- **Signature pizzas:** картки продуктів з Next/Image (q=75, оптимізовані), ціна, "Add to Cart".
- **🔥 Interactive Lab (killer feature):** "BUILD YOUR MASTERPIECE" — конфігуратор: вибір розміру (S/M/L) → топінги (з цінами) → live total ($15→...) → "Prepare Order / Cooking...". Це **stateful interactive configurator** — найдорожчий за відчуттям елемент.
- **Feature grid:** Lightning Fast / 900° Wood-Fired / 100% Organic / Heritage Craft.
- **Quote block:** цитата шефа, великий курсив.
- **Rotating pizza:** зображення з обертанням на scroll (rotate linked to scroll progress).
- **Final CTA:** "ONE BITE AND YOU'RE HOOKED."

### Патерни, які крадемо
| Патерн | Як працює | Цінність |
|---|---|---|
| **Тематичний preloader** | Лоадер у мові бренду ("Heating Oven") | Бренд з першої секунди |
| **Pinned scroll-story (3 beats)** | `ScrollTrigger pin + scrub`, контент міняється | Структура "кіно", контроль темпу |
| **Live configurator** | React state, live total, мікроанімації вибору | Найсильніше "$10k" відчуття — інтерактив |
| **Scroll-linked rotation** | `rotate = scrollProgress * 360` | Продукт "живе" в руках юзера |
| **Next/Image optimization** | автоматичні WebP, responsive sizes | Швидкість = преміум |

---

## 4. ОБ'ЄДНАНА БІБЛІОТЕКА ПАТЕРНІВ (ScrollCraft PRO Pattern Deck)

Це наш "інвентар". Будуючи сайт, ми обираємо 3–5 патернів і виконуємо їх ідеально, а не пхаємо всі.

### A. ВХІД / ЗАВАНТАЖЕННЯ
1. **Counter Preloader** (0→100%) — маскує preload WebP-послідовності, перша емоція.
2. **Thematic Loader** — текст у мові бренду ("Heating Oven", "Preparing your glow").
3. **Curtain Reveal** — після 100% "завіса" розсувається й відкриває hero (clip-path / translateY).

### B. HERO / СКРОЛ-ПОСЛІДОВНІСТЬ (ядро ScrollCraft)
4. **Frame-Sequence Scrub** — Kling-відео → WebP-кадри → canvas, scrubbed скролом (база PDF).
5. **Chaptered Hero** — текстові "глави" фейдяться на ключових offset поверх послідовності.
6. **Video Hero** — готовий рендер фоном (простіший за canvas, для легких проєктів).
7. **Split / Accent Headline** — частина заголовка іншим кольором/вагою/курсивом.

### C. ТЕКСТ / ТИПОГРАФІКА
8. **Staggered Line Reveal** — рядки заголовка виїжджають знизу з clip-path + stagger (SplitText / маски).
9. **Kinetic Marquee** — нескінченна стрічка-стрічка (seamless loop).
10. **Count-Up Stats** — числа 0→target на scroll-into-view.
11. **Text Mask / Clip Reveal** — текст з'являється з-під маски на scroll.

### D. ІНТЕРАКТИВ / HOVER
12. **Hover-Swap Cards** — два зображення, crossfade/scale на hover.
13. **Expanding Rows** — рядки розкриваються в зображення на hover.
14. **Live Configurator** — stateful вибір + live підрахунок (killer feature).
15. **Magnetic Buttons** — кнопка "притягується" до курсора (GSAP quickTo).
16. **Custom Cursor** — кастомний курсор з режимами (lerp follow + scale on hover).

### E. WebGL / ШЕЙДЕРИ (рівень "вау")
17. **Cursor Fluid / Displacement** — рідинне спотворення за курсором (RGB shift).
18. **Image Displacement Transition** — перехід між зображеннями через displacement map.
19. **Scroll Velocity Distortion** — зображення "розтягується" від швидкості скролу.
20. **Selective Bloom** — світіння лише на обраних матеріалах (luminanceThreshold).
21. **Depth-of-Field** — кінематографічне розмиття переднього/заднього плану.
22. **Grain / Noise Overlay** — плівкове зерно поверх усього (миттєвий "film" feeling).

### F. ПЕРЕХОДИ МІЖ СЕКЦІЯМИ
23. **Pinned Scroll-Story** — секція "пінується", контент змінюється під час скролу (Piozza beats).
24. **Parallax Layers** — шари рухаються з різною швидкістю (Lenis-driven).
25. **Scroll-Linked Rotation/Scale** — об'єкт обертається/масштабується від прогресу.
26. **Color Theme Shift** — фон/акцент плавно міняється між секціями на scroll.

### G. ВИХІД / ФУТЕР
27. **Big Statement CTA** — один великий заклик ("ONE BITE AND YOU'RE HOOKED").
28. **Reveal Footer** — футер "виїжджає" з-під контенту (sticky reveal).

---

## 5. ПРАВИЛА ВИКОРИСТАННЯ (щоб не вийшло "дешево")

- **Бюджет анімацій:** не більше **1 "героїчного" моменту на секцію**. Решта — стримані.
- **Easing:** ніколи `linear` для UI (тільки для marquee). Дефолт — `power3.out` / `expo.out`. Кінематографічні settle — `power4.out` з тривалістю 1.2–1.8s.
- **Lenis lerp:** `0.08–0.12` для важких 3D, `0.1` дефолт. Ніколи не "гумовий" (>0.2).
- **Performance budget:** hero frame sequence 120–180 кадрів, WebP, lazy-load below-fold, desktop 1920w / mobile 1080w.
- **prefers-reduced-motion:** завжди є fallback (статичний кадр замість послідовності).
- **Grain + DOF + Bloom** — максимум 2 одночасно (інакше FPS падає вдвічі).
- **Текст завжди читабельний** — WebGL під текстом, не навпаки.
