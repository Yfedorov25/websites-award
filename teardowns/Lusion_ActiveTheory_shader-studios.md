# Lusion + Active Theory · Teardown (Shader-Craft & Full-Canvas Studios)
> Lusion: SOTY 2023, FWA Hall of Fame. Active Theory: Spotify Wrapped, Hydra engine.
> Lusion витягнуто через Apify (повний DOM-UI + canvas). Active Theory — повністю порожній body (100% WebGL).

---

## ЧАСТИНА 1 · LUSION (lusion.co)

### Стек / архітектура (декодовано з DOM)
Гібридний підхід: **DOM-UI поверх повноекранної WebGL-сцени**.
```html
<body>
  <canvas id="canvas"></canvas>          <!-- головна WebGL-сцена (весь візуал) -->
  <div id="ui"> ...header, pages, footer... </div>  <!-- DOM лише UI/текст/проксі -->
  <canvas id="transition-overlay"></canvas>  <!-- ОКРЕМИЙ canvas для page transitions -->
  <div id="preloader">...роллер цифр...</div>
</body>
```
- **Один головний canvas** = весь візуал (hero, проєкти, tunnel) рендериться тут.
- **Окремий transition-overlay canvas** — присвячений WebGL-перехід між сторінками (wipe/shader-морф поверх усього).
- DOM = чистий контент-проксі: `home-hero-visual-container`, `project-item-image`, `home-goal-image-in/out` — порожні контейнери, на місці яких WebGL малює візуал (як r3f-scroll-rig ScrollScene, R03).

### Ключові знахідки
1. **Preloader-роллер цифр** (як ERA, підтверджено вдруге):
```html
<div id="preloader-percent-digits">
  <div class="preloader-percent-digit">
    <div class="preloader-percent-digit-num">0</div>  <!-- поточна -->
    <div class="preloader-percent-digit-num">0</div>  <!-- наступна -->
  </div> ×3 (сотні/десятки/одиниці)
</div>
```
Кожна цифра — стовпчик, що прокручується translateY. 3 розряди = 000→100.

2. **Data-driven палітра переходу на проєктах:**
```html
<a class="project-item" data-id="oryzo_ai"
   data-color-bg="#1a1411" data-color-text="#ffedd7" data-color-shadow="0.9">
```
Кожен проєкт несе свою палітру → при ховері/відкритті вся сцена морфить у ці кольори. Це створює відчуття, що кожен кейс — окремий світ.

3. **Декоративні «хрести» (crosses) + SVG-рамки** — технічна естетика (`home-hero-scroll-container-cross`, `video-container-crosses`). Дрібні маркери на кутах — фірмова деталь Lusion.

4. **Text-clone патерн для hover-ефектів:**
```html
<span class="header-menu-link-text">Home</span>
<span class="header-menu-link-text-clone">Home</span>  <!-- дублікат для slide-up hover -->
```
При ховері оригінал їде вгору, клон заходить знизу (стандартний award-патерн «масковане слово»).

5. **Labs (labs.lusion.co)** — окремий R&D-сайт з експериментами (шейдери, fluid). Джерело технік.

### Копірайт (антислоп, studio-tone)
- "We do not chase trends or produce work that looks like everyone else."
- "Step into a new world and let your imagination run wild."
- "Is Your Big Idea Ready to Go Wild?"
Прямий, впевнений, без жаргону. Заклик до уяви.

### Уроки Lusion
1. Окремий **transition-overlay canvas** — присвячений шар для page-transition шейдера (не змішувати з основною сценою).
2. **Data-driven палітри** на елементах → сцена морфить кольори контекстно.
3. **Preloader-роллер цифр** — третє підтвердження (ERA, Lusion) що це must-have деталь.
4. **Text-clone hover** — масковані слова в меню/кнопках.
5. **Технічні маркери (crosses)** на кутах — естетика «інженерної точності».

---

## ЧАСТИНА 2 · ACTIVE THEORY (activetheory.net)
- **Body повністю порожній** — навіть DOM-проксі немає. 100% рендериться у WebGL через їхній пропрієтарний движок **Hydra** (текст, UI, усе — у canvas).
- Це найрадикальніший підхід: нічого в DOM, усе — GPU.
- **Висновок для нас:** НЕ копіюємо (закритий движок, екстремальна складність, погано для SEO/доступності). Але вчимося ідеї: для арт-проєктів/лендингів-вражень можна рендерити навіть текст у WebGL (SDF/MSDF, R-text recipe). Для комерційних сайтів (нерухомість, продукт) — гібрид Lusion/IG (DOM-контент + WebGL-візуал) кращий: SEO + доступність + award-візуал.
- Active Theory тримає toolset закритим; публічних репо немає. Орієнтир, не джерело коду.

---

## ПІДСУМОК: 3 АРХІТЕКТУРНІ ПІДХОДИ (вибір для проєкту)
| Підхід | Хто | DOM | Коли обирати |
|---|---|---|---|
| **Гібрид DOM+WebGL** | Lusion, IG, ERA | контент у DOM, візуал у canvas | 90% проєктів: SEO+доступність+award-візуал. **Наш дефолт.** |
| **Progressive (DOM-first)** | r3f-scroll-rig сайти | повний DOM, WebGL зверху | контент-важкі сайти, де 3D — підсилення |
| **Full-canvas** | Active Theory | майже нічого | арт-інсталяції, ігри-враження; НЕ для комерції/SEO |

→ Для ЖК/нерухомості/продукту = **Гібрид** (R03 persistent canvas + DOM-контент). Підтверджено всіма трьома еталонами (Lusion/IG/ERA).

## НОВІ ЕЛЕМЕНТИ ДЛЯ БАЗИ
- **Окремий transition-overlay canvas** — додати в R03 (присвячений шар page-transition шейдера).
- **Data-driven палітра морфінгу** (`data-color-*`) — елемент задає кольори, сцена/секція морфить. Додати в 00_foundations/toolbox.md.
- **Text-clone hover** (масковане слово) — додати в патерни (текстові ефекти).
- **Preloader-роллер цифр** — підтверджено двічі (ERA+Lusion) → канонічний R-preloader.
- **Технічні crosses/маркери на кутах** — естетична деталь.
