# 🚀 ScrollCraft PRO — START HERE
> Операційна система виробництва award-winning cinematic / 3D / scroll-driven вебсайтів через Claude Code.
> **Це єдина точка входу.** Claude читає цей файл першим у кожному новому чаті, щоб зорієнтуватись у базі.

---

## ⚡ ШВИДКИЙ СТАРТ (сценарії)

**Сценарій 0 — запускаю НОВИЙ проєкт (головний шлях):**
→ Відкрий `META_PROMPT.md`, скопіюй блок у новий чат. Це двигун: веде від ніші →
дослідження бренду (скрап) → сенси й копірайт за Федорівим → структура → візуалізація
емоції рухом → інструменти/3D/Higgsfield MCP → ТЗ для Claude Code. СЕНС веде, техніка слідує.

**Сценарій A — хочу деталі операційних фаз:**
→ Читай `PIPELINE.md` (8 фаз від ідеї до деплою) і веди мене по них.

**Сценарій B — потрібна конкретна техніка (анімація/ефект):**
→ Відкрий `recipes/_INDEX.md` (шпаргалка техніка→коли) → візьми потрібний `recipes/RXX`.

**Сценарій C — хочу зрозуміти підхід / філософію / стек:**
→ `00_foundations/philosophy.md` (що робить сайт «$10k») + `00_foundations/toolbox.md` (інструменти).

**Сценарій D — треба написати копірайт (не AI-slop):**
→ `00_foundations/meaning-copy-system.md` (метод Федоріва + UA-регістри + emotion→copy) →
`copywriting/_RESEARCH_fedoriv_method.md` (піраміда) → `copywriting/_ANTISLOP_ukrainian_hard.md`
(жорсткий банлист, em-dash=0) → `niche-profiles.md` (голос ніші).

**Сценарій E — як рух має викликати емоцію:**
→ `00_foundations/motion-system.md` (easing-бібліотека з замірів, шкала темпу, класи руху,
stagger, emotion→motion). Розбори студій → `teardowns/` (M01-M20, W01-W09, UA01-UA06).

---

## 🗂 КАРТА БАЗИ (де що лежить)

```
scrollcraft-pro/
├── START_HERE.md              ← ти тут (точка входу)
├── META_PROMPT.md             ← 🧭 ГОЛОВНИЙ двигун: веде новий проєкт крок за кроком
├── PIPELINE.md                ← 🎯 операційна система: 8 фаз запуску проєкту
│
├── 00_foundations/            ← фундамент (читати на старті проєкту)
│   ├── meaning-copy-system.md ← 🧠 СЕНСИ+КОПІРАЙТ: метод Федоріва, UA-регістри, emotion→copy
│   ├── motion-system.md       → 🎬 АНАТОМІЯ РУХУ: easing-бібліотека, темп, stagger, emotion→motion
│   ├── philosophy.md          → «$10k feeling», правила «щоб не дешево», 28 патернів
│   ├── recipe-methodology.md  → як влаштований recipe, ризики 🟢🟡🔴, стек-еталон
│   ├── toolbox.md             → плагіни Claude Code, репо, студії, медіа-пайплайн
│   └── _accumulator_source.md → сирий масив замірів руху (джерело motion-system)
│
├── recipes/                   ← 🍳 24 техніки (R01-R24), кожна з кодом+промптом
│
├── teardowns/                 ← 🔬 44 розбори award-сайтів (стек+рух+сенси+копірайт)
│   ├── M01-M20                → 20 motion/WebGL студій (Lusion, Active Theory, Cuberto...)
│   ├── W01-W09                → 9 web/brand студій (Obys, Order, Gretel, AREA17...)
│   ├── UA01-UA06              → 6 укр. брендів (monobank, Нова Пошта, Дія, Taryan...)
│   ├── _RETROFIT_motion_meaning.md → догін Zera/Telha/ERA/IG новими вимірами
│   └── ERA/IG/Lusion/early-3  → ранні детальні еталони
│
├── copywriting/               ← ✍️ копірайт рівня Fedoriv (не AI-slop)
│   ├── _RESEARCH_fedoriv_method.md → 🔑 піраміда Федоріва з першоджерел
│   ├── _ANTISLOP_ukrainian_hard.md → 🚫 жорсткий банлист (em-dash=0, кальки, кліше)
│   ├── voice-foundations.md   → теорія + 12 принципів + brand-idea→copy
│   ├── ukrainian-school.md    → формула UA-школи, 2 регістри, фірмові слова
│   ├── niche-profiles.md      → профілі ніш: голос, лексикон, before/after
│   ├── real-estate-ua.md, microcopy-ux.md, world-masters.md, anti-slop-detector.md
│
├── business/                  ← 💰 конверсія, цінність+ціни, аналітика
├── research/                  ← 📚 повні звіти RESEARCH_03-10
├── templates/                 ← 📝 ТЗ, CLAUDE.md, KICKOFF-промпт
├── patterns/ · components/ · demos/
```

---

## 🍳 RECIPES — карта 22 технік

**Фундамент (майже завжди):** R03 persistent WebGL · R04 scroll choreography · R05 post-processing
**Hero/reveal:** R01 frame-sequence · R11 window-reveal · R18 SVG-mask · R16 fluid X-ray · R17 MSDF dissolve
**Типографіка:** R23 kinetic-type transitions · R17 dissolve
**Галереї/портфоліо:** R06 displacement · R19 GSAP Flip · R20 infinite canvas
**Інтерактив/курсор:** R02 cursor · R22 glass refraction · R08 fluid
**3D/частинки:** R07 GPGPU · R10 3D-карта району · R12-R14 3D-тури (Splatting/360/walkthrough)
**Стилізація:** R21 dithering/ASCII · R24 SVG path draw
**Рендер-інфраструктура:** R09 WebGPU/TSL міграція · R15 TSL dual-pipeline

> Деталі кожного + рівень ризику + вибір під нішу → `recipes/_INDEX.md`

---

## 🌊 ТРИ ХВИЛІ ВПРОВАДЖЕННЯ НОВИХ ТЕХНІК (R15-R24, з research 2026)
1. **Хвиля 1 (🟢 низький ризик):** R18, R19, R22, R24 — DOM/GSAP/drei, готові репо. Почати з них.
2. **Хвиля 2 (🟡 R3F):** R20, R16 — клонувати репо, адаптувати під Next.js (dynamic import ssr:false).
3. **Хвиля 3 (🔴 frontier):** R15, R17, R21 — лише desktop-first + WebGL fallback. Поріг: mobile <40% і LCP-бюджет дозволяє.

---

## 🛠 GSTACK (інженерна команда в Claude Code)
Встановлення: `git clone --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup`
Ключові скіли: `/office-hours` (discovery), `/design-shotgun`→`/design-html` (мокапи), `/browse` (competitor scrape), `/autoplan`→`/ship`→`/qa`→`/land-and-deploy`, `/design-review` (AI-slop детектор), GBrain (память).
> Ми ОБГОРТАЄМО GStack нашою базою: GStack дає процес/інженерію/QA, ScrollCraft дає 3D/motion/award-техніки (recipes), яких у GStack нема. Деталі — `00_foundations/toolbox.md`.

---

## 🧱 ЦІЛЬОВИЙ СТЕК
Next.js 15 · React Three Fiber + drei · GSAP (ScrollTrigger/Flip) · Lenis · Three.js r183 (three/webgpu + three/tsl) · custom GLSL/TSL · Rapier (фізика) · Howler (звук) · gltf-transform (ассети) · Vercel.

---

## 💡 10 ПРИНЦИПІВ (щоб виходив award, не «дешево»)
1. Display-шрифт, НЕ Inter. Палітра з характером.
2. Reveal-темп під нішу: звичайне 0.9s, luxury/real-estate 1.5-3s (урок ERA).
3. Easing power3.out (входи), Lenis lerp 0.1.
4. Гібрид DOM+WebGL (SEO+доступність+візуал) — дефолт, не full-canvas.
5. Один persistent canvas (R03), не десятки.
6. Performance-бюджет святий: LCP <2.5s, 60fps desktop. WebGL дренує — fallback обов'язковий.
7. Mobile/reduced-motion fallback скрізь (важкий WebGL desktop-only + `<picture>`, як ERA).
8. 🔴-техніки (fluid/GPGPU/WebGPU) — адаптуємо перевірені репо, не пишемо solver з нуля.
9. Копірайт справжній (контекст-документи, B1-B2, «сказав би людині?»), не AI-slop.
10. Кожна техніка модульна (один recipe = один компонент), як движок Bruno.

---

## 📊 СТАН БАЗИ
- ✅ 22 recipe (R01-R24) — повне покриття технік
- ✅ 4 teardown (ERA, IG, Lusion/AT, + перші 3 сайти)
- ✅ 2 повні research-звіти (award-сайти 2026 + копірайт) + інтегровані попередні
- ✅ Копірайт-система рівня Fedoriv (3 файли, анти-AI-slop, Фаза 4)
- ✅ PIPELINE (7 фаз) + foundations (3 файли) + templates
- 🔜 нові teardown сайтів 2026 (Cartier, threejs.paris, Scout Motors, Razorpay, Jeton); обкатка recipe на реальних білдах

## 🚀 СТАРТОВА ФРАЗА ДЛЯ НОВОГО ЧАТУ
> «Запускаємо новий проєкт по ScrollCraft PIPELINE. Проєкт: [опис]. Ніша: [...]. Tier: [1/2/3]. Матеріали: [картинки/факти/відео]. Конкуренти: [URL або "знайди"]. Естетика: [...]. Почнімо з Фази 0-1.»
