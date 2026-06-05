# Active Theory · Teardown #6 — activetheory.net (Layer 3, network-driven) ✅
> Еталон #2. Розбір через Claude in Chrome. DOM порожній (все в WebGL), тому головна цінність — network-трейс, що розкрив власний движок Hydra.
> **Категорія:** MOTION/WebGL/3D (in-house toolset). Засн. 2012, LA. Клієнти: Google, Nike, Adidas.

---

## ГОЛОВНЕ (суть одразу)
1. **Власний движок «Hydra», multi-threaded.** Network: `hydra-thread.js` завантажується **×8** — рушій крутиться у ~8 Web Workers паралельно (багатопотоковий WebGL-рендер/логіка). Це і є їхній «industry-leading web toolset» зі слогану.
2. **Прекомпільовані шейдери** (`/assets/shaders/compiled.vs`) — власний шейдерний пайплайн, не runtime-компіляція.
3. **Headless CMS на Google Cloud Storage** — контент (проєкти/контакти/мета) тягнеться як JSON із GCS/Firebase. Рушій статичний, контент динамічний.
4. Як і Lusion — **повний WebGL-сайт**: DOM майже порожній, усе мальоване в canvas. Другий еталон поспіль без Three.js і без фреймворку.

---

## СТЕК (probe + network)
- Рендер-движок: **Hydra** (власний, Active Theory). Multi-thread: `hydra-thread.js ×8` (Web Workers).
- Фреймворк: **відсутній** (vanilla, 3 основні скрипти: `app.js`, `modules.js`, `hydra/hydra-thread.js`). Не React/Next/Vue/Three.
- Шейдери: прекомпільовані `.vs` (vertex shaders) з `/assets/shaders/`.
- Контент/CMS: **Google Cloud Storage** (`storage.googleapis.com/activetheory-v6.appspot.com/cms/*.json`) — projects/metadata/contact як JSON. Firebase-проєкт (appspot.com).
- Бекенд-функції: Google **Cloud Functions** (`cloudfunctions.net/geo` — геолокація відвідувача).
- Аналітика: GA4 (G-J7TMDT4F8N).
- Версіонування ассетів: timestamp у назві (`app.1780406240914.js`) — cache-busting.

## ТИПОСИСТЕМА
- **nbarchitekt** (Neue Architekt) — єдиний підвантажений шрифт. Технічний/архітектурний гротеск. Мінімалістична типосистема (як і Lusion — нейтральний гротеск, бо акцент на 3D).
- Чорний фон (`bodyBg #000`) — темна WebGL-база (контраст до Lusion на білому).

## АРХІТЕКТУРНИЙ ПАТЕРН (найцінніше) ★★★
**Розділення «движок ↔ контент»:**
- Статичний шар: скомпільований Hydra-движок + шейдери + модулі (деплоїться рідко).
- Динамічний шар: CMS-JSON на GCS (projects-dev.json, metadata-dev.json, contact-dev.json) — оновлюється без редеплою.
- Це дозволяє нетехнічним людям міняти проєкти/тексти через CMS, поки движок лишається незмінним.
**Урок для нас:** навіть на повністю кастомному WebGL-сайті контент варто винести в headless-CMS-JSON (у нас: Sanity/Contentful/власний JSON на CDN). Не хардкодити проєкти в движок.

## MULTI-THREADING (унікальне)
- `hydra-thread.js ×8` — рідкість: більшість WebGL-сайтів однопотокові (все в main thread). Active Theory виносить роботу в 8 Web Workers → плавність навіть на важких сценах. Це їхня технічна перевага в продуктивності (слоган: «quality & performance»).
- Для нас недосяжно як готове рішення, але принцип «важку логіку — у Web Worker, main thread тримати вільним для рендеру» — береться в toolbox.

## КОПІРАЙТ / ПОЗИЦІОНУВАННЯ
- Title: «Active Theory · Creative Digital Experiences».
- Desc (маніфест): «Founded in 2012. We blend story, art & technology as an in-house team of passionate makers. Our industry-leading web toolset consistently delivers award-winning work through quality & performance.»
- Меседж: акцент на ВЛАСНОМУ тулсеті (Hydra) як конкурентній перевазі + якість/перформанс. Позиціонування «in-house makers», не аутсорс.

## ПОРІВНЯННЯ Lusion ↔ Active Theory (два еталони WebGL)
| | Lusion | Active Theory |
|---|---|---|
| Движок | власний WebGL2 | власний «Hydra» |
| Threading | (не видно multi) | **8 Web Workers** |
| Геометрія | `.buf` бінарний + LOD | (шейдери .vs, моделі не видно) |
| База/палітра | біла + насичені RGB | чорна + мінімум |
| Шрифти | Aeonik + 2 моно (1 кастомний) | nbarchitekt (1 гротеск) |
| Контент | в движку | headless CMS-JSON (GCS) |
| Фреймворк | немає | немає |
**Спільне:** обидва — власний движок, без Three.js, без React, повний canvas, мінімалістична типосистема. Це «вищий ешелон» — студії що пишуть свій рушій. Більшість award-студій (нижче в списку) використовують Three.js/R3F — їх нам копіювати реалістичніше.

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **Патерн «статичний движок + headless CMS-JSON»** → patterns/ (виносити контент з WebGL у CMS).
- **Web Workers для важкої логіки** (тримати main thread для рендеру) → toolbox.
- **Прекомпіляція шейдерів** (build-time, не runtime) → toolbox оптимізація.
- **Cache-busting через timestamp в імені ассета** → toolbox (деплой).
- **Реалізм:** Hydra недосяжна; наш шлях — Three.js/R3F + ці архітектурні принципи (CMS-JSON, workers, прекомпіляція).

## TODO
- DOM не віддав секції/копірайт сцени (важкий прелоадер). За потреби — повернутись зі скріншотами, але технічна суть (Hydra/CMS/workers) уже зрозуміла.
- Далі #3 Immersive Garden (вже є старий teardown — звірити/оновити Layer 3).
