# Zera Studio · Teardown #4 — Masterclass LMS (Deep / Layer 3, залогінено) ✅
> Розбір приватного LMS-порталу через Claude in Chrome у залогіненій сесії (DOM + network-трейс + плеєр).
> **Мета:** зрозуміти, як технічно зроблено навчальний сайт — щоб будувати такі як окрему нішу (курси: проходження, розблокування, прогрес, субтитри).
> Це найцінніший teardown для нової ніші «сайти навчання».

---

## ГОЛОВНЕ ТЕХНІЧНО (одразу суть)
1. **LMS — власна, на Next.js (App Router + RSC), НЕ готова платформа.** Жодного Teachable/Kajabi/Thinkific/Circle у network. Дані курсу приходять через RSC-payload (server components), не через клієнтський REST API.
2. **Бекенд — Prisma + БД.** Доказ: ID уроків — **cuid** (`cmnam3ui7000beawsoehu9m8u`), фірмовий формат Prisma. Тобто є БД з таблицями users/modules/lessons/progress.
3. **Відео — Bunny Stream** (`player.mediadelivery.net` iframe + `assets.mediadelivery.net/playerjs`). НЕ self-host, НЕ Vimeo/YouTube. Дешевий video-CDN з HLS + вбудованим плеєром.
4. **Субтитри різних мов = вбудована фіча Bunny Stream** (авто-транскрипція + авто-переклад), керується в кабінеті Bunny, НЕ кодом сайту. Сайт лише вбудовує iframe.
5. **Керування плеєром — Player.js** (`window.playerjs`, postMessage API): сайт слухає події плеєра (progress/ended) → пише прогрес у свою БД → розблоковує наступний урок.
6. Стек той самий, що вся Zera: Next.js + Tailwind + ті ж 5-6 шрифтів. LMS — просто ще один розділ їхнього моноліту.

**Висновок для нашої ніші:** повторюваний рецепт навчального сайту = **Next.js (App Router) + Prisma/Postgres + Bunny Stream (відео+субтитри) + Player.js (трекінг прогресу) + Stripe (оплата)**. Без жодної готової LMS-платформи. Це і є «як зробити такий сайт».

---

## КАРТА ПОРТАЛУ (sitemap)
```
/masterclass              → Dashboard (welcome, прогрес, continue, mentorship-calls, featured-upsell)
/masterclass/curriculum   → повна програма (9 модулів / 35 уроків), стани done/locked
/masterclass/lessons/[cuid] → урок (Bunny-плеєр + overview + feedback + prev/next)
/masterclass/resources    → магазин шаблонів (9 сайтів) + 4 фреймворки
/masterclass/account      → профіль, course access, телефон, зміна пароля, sign out
/affiliate/apply          → партнерська програма
```
Нав порталу: Dashboard · Curriculum · Resources · Account · Affiliate.

---

## 1) DASHBOARD (`/masterclass`)
Призначення — повернути і втримати. Блоки згори вниз:
- **«Welcome back, {ім'я}»** + «Pick up where you left off» — персоналізація.
- **Кільце/бар прогресу:** `14% COMPLETE`, класи `mc-nav-progress-*` (кастомні, не бібліотека).
- **CONTINUE LEARNING** — карта останнього незавершеного уроку + кнопка Resume (deep-link на `/lessons/[id]`).
- **UPCOMING MENTORSHIP CALLS** — live-дзвінки з датами/часом (EST) + кнопка Join, мітка NEXT. (Гібрид: курс + жива менторська програма.)
- **FEATURED THIS WEEK** — упсел шаблону з таймером «ENDS IN 4 DAYS», стара/нова ціна, SAVE. (Монетизація прямо в дашборді.)
- **Статистика:** `2/10 MODULES · 5/35 LESSONS · 7h26m REMAINING`.

**Уроки UX:** dashboard = «магазин повернення» — прогрес (зобов'язання) + resume (нуль тертя) + дедлайн-упсел (терміновість) + live-календар (спільнота).

---

## 2) CURRICULUM (`/masterclass/curriculum`) — повна програма
9 модулів (00-08), 35 уроків, ~8h15m. Стан кожного модуля показано як `X/Y` (виконано/всього). Кожен урок: назва + тривалість (3-25 хв).

```
00 Welcome to TAWWD                              1/1
   • Welcome & Course Overview (3m)
01 The $5K–$20K Website Model                    4/4
   • What "high-end" actually means (not tools) (14m)
   • Breakdown of a $5K–$20K website (9m)
   • What clients are really paying for (10m)
   • The exact type of site you'll build (13m)
02 The System Behind Award-Level Websites        0/5
   • Story (messaging+flow) · Structure · Hierarchy · Motion · Interaction
03 Reference & Deconstruction System             0/4   ← це буквально те, що ми зараз робимо
   • How to find high-end inspiration (controlled)
   • Breaking down layouts and sections
   • Extracting animations and flow
   • Turning references into a build plan
04 AI Build Workflow (Your Edge)                 0/4
   • Setup (Cursor, etc.) · Prompt for layout · Prompt animations · Fix/improve AI outputs
05 Getting High-Ticket Clients                   0/5
   • Who to target · Position "premium" · Outreach · Pricing ($2K–$10K+) · Closing
06 1 Site → 10 Clients Without Rebuilding        0/4
   • Reusing structures · Templates · Systemize workflow · Increase pricing
07 Build Your First High-End Website (PART 1)    0/4
   • Project setup · Hero section · Core sections ...
08 (Part 2 / фінал) ...
```
**Спостереження:** структура курсу = наш власний пайплайн (Reference & Deconstruction = крок А/Б, AI Build Workflow = Claude Code, $5K-20K модель = ScrollCraft tiers). Можна взяти як кістяк ВЛАСНОГО курсу.

---

## 3) LESSON (`/masterclass/lessons/[cuid]`) — найважливіше технічно
Layout уроку:
- **Bunny-плеєр** (`<iframe src="player.mediadelivery.net/...">`, params `autoplay=false preload=true`, керується Player.js).
- **Заголовок:** «MODULE 2 — ...» + назва уроку + `0%` прогрес.
- **Статус-рядок розблокування:** *«Lesson complete — next lesson unlocked»* — ядро gating-механіки.
- **Lesson overview** — текстовий конспект під відео (SEO + доступність + швидке повторення).
- **Featured-упсел** (той самий шаблон — наскрізна монетизація).
- **Feedback-віджет:** «How was this lesson?» емоджі-шкала (😕 Confused / 😐 Okay / 🙂 Good / 😊 Great / 🔥 Fire) + 2 опційні текст-поля («What's still unclear?», «What to go deeper on?») + Submit. → збір per-урок якісного фідбеку в БД.
- **Навігація:** ← Previous / Next → + «0/5 in module».

### Механіка прогресу й розблокування (реконструйовано)
1. Bunny-плеєр через Player.js шле події (`timeupdate`/`ended`).
2. Клієнт на `ended` (або X% переглянуто) → пише прогрес у БД (через server action/RSC mutation, не публічний REST).
3. Прогрес ставить урок `done` → наступний `unlocked` («next lesson unlocked»).
4. Дашборд/curriculum читають прогрес із БД → показують `%`, `X/Y`, resume-точку.

### Субтитри різних мов (відповідь на питання)
- Це **функція Bunny Stream** (Automatic Subtitle/Caption Generation + переклад на десятки мов). Завантажуєш відео в Bunny → вмикаєш авто-субтитри → Bunny транскрибує і перекладає → плеєр показує перемикач мов. **Нуль коду на стороні сайту.**
- Тобто «автоматичні субтитри різних мов, дуже круто реалізовано» = це не їхня розробка, а правильно обраний провайдер. **Нам — просто взяти Bunny Stream.**

---

## 4) RESOURCES (`/masterclass/resources`) — магазин шаблонів
«Your toolkit» — 2 розділи: **Premium Templates (9 production-сайтів)** + **Core Resources (4 фреймворки)**.
Бандл-механіка: «Pick any 3 for $299» (замість $149×3, save $148). Кожен шаблон — стек-теги:
| # | Шаблон | Опис | Стек |
|---|---|---|---|
| 01 | Horizon Island | 3D-острів портфоліо з explorable-сценою і біпланом | Three.js / React / Vite |
| 02 | Reel Studio | cinematic video-hero + scroll GSAP | GSAP / React / Vite |
| 03 | Visio | creative studio, fullscreen video hero | GSAP / React / Vite |
| 04 | Orion | agency, 3D-планета hero + smooth scroll | Three.js / GSAP / Lenis |
| 05 | Drift Field | WebGL playground, postprocessing + шейдери | Next.js / Three.js / WebGL |
| 06 | Specimen (-50%) | experimental portfolio, physics-WebGL + editorial | Next.js / Three.js / GSAP |
| 07 | Nexus | WebGL studio marketing, custom shaders + Lenis | Next.js / Three.js / GSAP |
| 08 | Detail Driven | premium auto-detailing, 3D + page transitions | Three.js / GSAP / Barba.js |
| 09 | Aero | immersive scroll-driven ... | (Three.js/GSAP) |

**Урок:** портфоліо-демо (p4/p5/p8/p9, що ми бачили) = ці самі продавані шаблони. Тобто **демо-сайт = товар**. Подвійна монетизація: курс учить будувати + готові шаблони продаються окремо. Стек шаблонів = наш цільовий стек (Next + Three + GSAP + Lenis + Barba/Vite).

---

## 5) ACCOUNT (`/masterclass/account`)
Profile (name/email read-only, зміна через підтримку) · Course access (Active) · Contact number (для mentorship follow-up) · Change password · Sign out. Мінімальний, без зайвого.

---

## АНАЛІТИКА / ТРЕКІНГ (network)
- **Meta Pixel** (`facebook.com/tr`, `fbevents.js`, id 1579685330029498).
- **Google Ads + GTM** (`AW-16954134833`, `GTM-K6KSTS8X`), conversion + remarketing (`1p-user-list`).
- Тобто навіть приватний LMS-портал має повний маркетинг-трекінг (ретаргет учнів на апсели).

---

## БІЗНЕС-МОДЕЛЬ (як влаштовано заробіток)
Три рівні монетизації в одному порталі:
1. **Курс** (масterclass, основний продукт).
2. **Шаблони** ($149 / бандл $299) — апсел у дашборді, в уроці, в resources.
3. **Менторство** (live-calls) + **Affiliate** (партнерка `/affiliate/apply`).
Терміновість скрізь: «ENDS IN 4 DAYS», лічильники, «50% off».

---

## РЕЦЕПТ «НАВЧАЛЬНИЙ САЙТ» ДЛЯ НАШОЇ БАЗИ (нова ніша) ★★★
Стек-рецепт, готовий до повторення через Claude Code:
- **Next.js (App Router)** — сторінки dashboard/curriculum/lesson/resources/account.
- **Prisma + Postgres** — моделі: User, Module, Lesson, Progress, Purchase. (cuid-ID.)
- **Bunny Stream** — хостинг відео + HLS + **авто-субтитри/переклад** (нуль коду на мови) + Player.js для подій.
- **Server Actions / RSC** — читання курсу й запис прогресу (без публічного REST → безпечно, дані не светяться в network).
- **Gating:** lesson.done (по `ended`/%) → unlock next. Прогрес-агрегація на дашборд.
- **Stripe** — оплата курсу/шаблонів/бандлів.
- **Аналітика:** GA4/GTM + Meta Pixel для апселів і ретаргету.
- **UX-модулі:** continue/resume, прогрес-кільце, mentorship-календар, per-lesson emoji-feedback, featured-упсел з таймером.

### Чек-ліст фіч навчального сайту (з цього кейсу)
- [ ] Dashboard з resume + % + статистикою (modules/lessons/time-remaining)
- [ ] Curriculum-дерево з станами done/locked, тривалість уроків
- [ ] Lesson: відео + текст-overview + prev/next + module-progress
- [ ] Послідовне розблокування (next unlocked on complete)
- [ ] Авто-субтитри багатьма мовами (Bunny)
- [ ] Per-lesson feedback (emoji + текст)
- [ ] Магазин шаблонів/ресурсів + бандли
- [ ] Live-mentorship календар
- [ ] Account (профіль, доступ, пароль)
- [ ] Affiliate-програма
- [ ] Маркетинг-трекінг + апсели з терміновістю

## НОВІ ЕЛЕМЕНТИ ДЛЯ БАЗИ
- **Нова ніша «навчальні сайти / LMS»** → niche-profiles (стек-рецепт + чек-ліст фіч + бізнес-модель вище).
- **Патерн «власна LMS на Next+Prisma+Bunny+Player.js»** → patterns/ (альтернатива готовим платформам).
- **Bunny Stream** → toolbox (відео-CDN з авто-субтитрами; дешевший за Vimeo, простіший за self-host HLS).
- **UX-модулі LMS** (resume-dashboard, gating, emoji-feedback, progress-ring) → components-каталог.
- **Патерн монетизації «демо = товар»** (портфоліо-демо продаються як шаблони) → business/.
- **Структура курсу TAWWD** як кістяк, якщо робитимемо ВЛАСНИЙ навчальний продукт.

## TODO
- (опц.) Зайти в Bunny-плеєр і підтвердити перелік мов субтитрів кліком по CC (поки що: підтверджено провайдера + Player.js, мови керуються в Bunny).
- (опц.) Подивитись /affiliate/apply — структуру партнерки.
- Крок Б (наступний): award-агенції світу для балансу датасету.
