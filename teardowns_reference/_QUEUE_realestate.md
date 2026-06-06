# _QUEUE_realestate — черга глибокого скрапу НЕРУХОМОСТІ
> Мета: добити десятки award-RE прикладів. По 1-2 за раз, повна глибина (схема D_), з відтворюваністю.
> Тека: teardowns_reference/. Ставимо ✅ коли D-файл готовий.

## ✅ ГОТОВО (D-файли в базі)
- ERA — era.estate (Locomotive, 3s reveal, найповільніший) ✅
- AIR — aircenter.space (кастомний transform-scroll, бізнес-центр) ✅ + **AIR_location** (карта-план SVG → PB_interactive_map) ✅
- Springs — springs.estate (Locomotive+WebGL, luxury-поезія) ✅
- Silver Pinewood — silver-pinewood.com (Locomotive+WebGL, «quiet luxury») ✅
- Olivia Harper — oliviaharperhomes.com (WP+Lenis+GSAP, US ultra-luxury, інвест) ✅
- FIND Real Estate — findrealestate.com (Cuberto, Lenis, платформа/брокеридж) ✅
- Taryan Towers ✅ · Son Daven (hospitality+invest) ✅

## 🎯 ДАЛІ — Vide Infra RE-ростер (живі лінки з кейсів videinfra.com/work/<slug> → «Visit Website»)
> Топ-студія RE; кожен — award. Резолвити живий лінк у кейсі, перевірити доступність (деякі RU можуть бути заблоковані).
- poklonnaya-9 → poklonnaya9.site ⚠️ НЕДОСТУПНИЙ (помилка/гео-блок) — пропустити або через архів
- victory-park-residences → (резолвити live; Москва — ризик гео-блоку)
- azure → (резолвити live)
- eniteo → (резолвити live; Москва — ризик)
- Parametr → комерційна RE-вітрина (без живого лінку в кейсі)

## 🎯 ДАЛІ — Awwwards recent RE SOTD/HM (резолвити URL пошуком/візитом)
- Elyse Residence (HM) · Fame Estate · TANDEM · Hubtown · BoxCar Apartments
- LET US Ibiza · PAISANA©Living · Real Estate Abbey · TRANS×HOME
- Cuberto: Born & Bred, Sweeping Corp (суміжні, перевірити чи RE)

## ⚪ НИЖЧИЙ ПРІОРИТЕТ (WordPress/Squarespace — не GSAP/Locomotive-tier)
- The Coloradan (Squarespace) · Akoya Boca West (Divi) · Rich Land Dubai · Icon Villas
- Tbilisi Gardens — ⚠️ ПРОПУСТИТИ (старий AngularJS `ng-animate`, не наш модерн scroll-motion рівень)

## МЕТОД (нагадування)
1. Резолв live URL → navigate → screenshot (перевірка доступності; важкі WebGL-сайти можуть фризити рендер — давати 4-6с, зонд робити легким/частинами).
2. Повний зонд: стек+плавність (Lenis/Locomotive/will-change/split) → типо (px/weight/lh/ls) → палітра → структура секцій → копірайт (фрагменти) → темп (durations) → відтворюваність 🟢🟡🔴.
3. D-файл за схемою → ЗБАГАЧУЄ відповідні PB_/P_.

## НАСКРІЗНИЙ RE-ВИСНОВОК (оновлюється)
- Рецепт плавності Vide Infra (Locomotive + split-text + will-change + transform-only + 0.4-0.8s) = стабільний відтворюваний стандарт luxury-RE.
- Lenis = універсальний рушій плавності — працює і кастом (FIND), і на WordPress (Olivia Harper).
- Монумент-display (lh 0.9-0.95, від'ємний трекінг −2…−5px), часто ОДНА родина шрифту АБО антиква+грос пара.
- Палітра: або дарк-монохром (AIR), або тепла кремова (ERA/Silver Pinewood/Olivia Harper), або графічна near-black+сіре (FIND).
- Копірайт: luxury = адреса-спадок/спосіб життя, нуль м²; платформа/брокеридж = маніфест «руху» + пруф-цифри + послуги.
- Карта локації = намальований SVG-план (PB_interactive_map), не Google.
