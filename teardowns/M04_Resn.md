# Resn · Teardown #7 — resn.co.nz (Layer 3) ✅
> Еталон #4. Розбір через Claude in Chrome. Засн. 2004, Веллінгтон NZ + Амстердам. Експериментальні «gooey» WebGL-досвіди.
> **Категорія:** MOTION/WebGL/3D. Клієнти: Google, Adobe, Converse, Droga5.

---

## ГОЛОВНЕ
1. **Перший еталон на СПРАВЖНЬОМУ Three.js** (не власний движок) — `THREE r84`. Але стара версія (~2017).
2. **GSAP старого покоління** — `TweenMax / TimelineMax / TweenLite` (GSAP 2, до gsap 3). Перевірений legacy-стек.
3. **Свідомий «застиглий» стек 2017-2018**, який досі дає award-роботи. Доказ, що нові версії не обов'язкові для топ-рівня.
4. Контент мальований у WebGL (DOM-текст майже порожній). 2 canvas, чорна база.

## СТЕК (probe)
- 3D: **Three.js r84** (2017). Не власний движок (контраст до Lusion/AT).
- Анімація: **GSAP 2** (TweenMax/TimelineMax/TweenLite/TimelineLite/TweenPlugin — глобали на window).
- Scroll: Lenis=false (свій/GSAP-based, бо стек до-Lenis епохи).
- Фреймворк: немає (vanilla, **14 скриптів** — стара модульна архітектура з багатьма файлами, не bundled в 1-2).
- Аналітика: GA4 (G-RWMTZ1D17T).

## ТИПОСИСТЕМА (багата на ваги)
- **Fort** (Sharp Type) — преміальний гротеск, 5 ваг: Extralight / Light / Book / Medium / Bold.
- **Work Sans** — 4 ваги: Thin / ExtraLight / Light / Regular.
- Дуже багато легких ваг (Thin/Extralight/Light домінують) → тонка, повітряна типографіка на чорному. Формула: 2 гротески з широким діапазоном ваг (без антикви).

## ПАЛІТРА
- База: чорна (`#000`). Акцентів у DOM не видно (усе в WebGL/шейдерах — кольори емісійні в canvas, як у Lusion).

## АРХІТЕКТУРНИЙ УРОК (найцінніше) ★
**«Застиглий перевірений стек».** Resn — award-студія з 20-річною історією — досі на Three.js r84 + GSAP 2 (2017). Не женуться за версіями. Урок: **award-рівень дає майстерність і концепція, а не свіжість бібліотек.** Для нас: не обов'язково найновіший R3F/WebGPU — стабільний Three+GSAP достатній, якщо виконання сильне. (Контраст до Zera, що на свіжому Next 15 + three r162.)

## КОПІРАЙТ / ПОЗИЦІОНУВАННЯ
- Title: «Resn — Creative Digital Agency | Ideation, Design, and Development».
- Desc: «...bringing brand, content and digital experiences to life through stories shaped with global brands and emerging innovators.»
- Видимий DOM-текст мінімальний: «View All Projects» (решта — WebGL).

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **Принцип «стабільний стек > свіжий стек»** → philosophy (виконання важливіше за версії).
- **Багатовагова типосистема на чорному** (Thin→Bold одного гротеску) → toolbox типосистем.
- Resn = доказ, що Three.js (навіть старий) + GSAP достатньо для award — наш реалістичний шлях (на відміну від власних движків Lusion/AT).

## TODO
- DOM не віддав секції (WebGL). Технічна суть (Three r84 + GSAP2 + типосистема) зрозуміла.
- Далі #5 Merci-Michel.
