# Immersive Garden · Teardown #3 — Layer 3 ДОПОВНЕННЯ (live probe)
> Доповнення до існуючого ImmersiveGarden_luxury-agency.md (той знятий через Apify-DOM). Тут — live WebGL-probe + network через Claude in Chrome. Об'єднати при перебудові бази.

## НОВЕ / ПІДТВЕРДЖЕНЕ live-probe
- **WebGL2** (не просто WebGL) — підтверджено context-probe.
- **8 canvas-шарів** одночасно (як Active Theory; multi-layer compositing). Старий teardown казав «єдиний persistent canvas» — насправді на головній 8.
- **Nuxt + Vue** — підтверджено (`nuxt:true, vue:true`). Узгоджується зі старим (Nuxt 3). На відміну від Lusion/Active Theory (vanilla, без фреймворку) — IG будує WebGL ВСЕРЕДИНІ Vue-додатку. Це для нас найреалістичніший патерн (фреймворк + WebGL), ближчий за власний движок.
- **THREE=null, gsap=null, Lenis=false на window** — bundled у 2 скрипти (Nuxt-бандл ховає глобали). Старий teardown фіксував GSAP/кастомний скрол — вони в бандлі.
- Лише **2 скрипти** — все зібрано в Nuxt build.

## ТИПОСИСТЕМА (повна, live)
- **PSTimes** — антиква-display (ефектний H1 «Transcend anything seen or felt before...»).
- **Helvetica Neue Regular + Light** — нейтральний гротеск (тіло/UI).
- Формула: антиква-display × гротеск-тіло (як Zera). Контраст до Lusion/AT (чисто гротеск). → антиква присутня там, де luxury/editorial-позиціонування.

## ПАЛІТРА (live)
- База: **`#e8e8e8`** (світло-сіра, галерейна) — не чорна (AT) і не біла (Lusion). Преміально-нейтральна, «музейна».
- (Детальних токенів на :root немає — кольори в бандлі/інлайн.)

## МЕДІА-ПАЙПЛАЙН (network, нове)
- Усі медіа подаються як **blob: URLs** (fetch→ObjectURL, ~12 активних). Контроль декодування/кешу + ускладнює пряме витягування ассетів. Старий teardown зафіксував CDN DigitalOcean Spaces — тобто: завантаження з DO Spaces → blob у пам'яті → у WebGL/відео.

## КОПІРАЙТ (live, нове формулювання H1)
- H1: «Transcend anything seen or felt before by crafting unparalleled experiences for ambitious brands.»
- Інтро: «Innovative / digital / experiences / studio / Scroll down».
- Проєкти-якорі: Louis Vuitton VIA (Web3 digital trunk), Cartier End of Year, David Whyte poetry journey. Усе luxury/editorial.

## ПІДСУМОК ПОРІВНЯННЯ (3 еталони WebGL)
| | Lusion | Active Theory | Immersive Garden |
|---|---|---|---|
| Движок | власний WebGL2 | власний «Hydra» (8 workers) | **WebGL2 в Nuxt/Vue** |
| Фреймворк | немає | немає | **Nuxt 3** ← найреалістичніше для нас |
| Canvas | 3 | 0 видимих (Hydra) | 8 |
| База | біла + RGB | чорна | сіра #e8e8e8 |
| Шрифти | Aeonik+2моно | nbarchitekt | PSTimes+Helvetica (антиква+гротеск) |
| Текст | в сцені | CMS-JSON | **WebGL SDF-заголовки** (data-webgl-title) |
**Висновок:** з трьох еталонів IG — найближчий до нашого реалістичного шляху: фреймворк (Nuxt) + WebGL2-шар поверх, а не власний vanilla-движок. У нас аналог = Next.js + R3F.
