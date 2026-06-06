# 🎯 ScrollCraft PRO — Трирівнева award-система (індекс)
> Claude ДУМАЄ за критеріями (аналізатори) → обирає блоки (бібліотека) → збирає суперТЗ → Claude Code будує award-результат.
> Фокус: TIER 1-2 (GSAP+Lenis, R3F дозовано, без важкого WebGL).

## РІВЕНЬ 0 — ОРКЕСТРАТОР
- **META_PROMPT_v2.md** — копіюй у новий чат. 7 фаз: діагностика→сенси→структура→візуал→аналіз за 50 критеріями→суперТЗ→медіа.

## РІВЕНЬ 1 — АНАЛІЗАТОРИ (як ДУМАТИ — мозок)
- **A1_award_criteria.md** — 50 award-критеріїв (8 блоків) + червоні прапорці джуніора. Мозок оцінки.
- **A2_context_diagnosis.md** — тип сайту / тір / ніша-профіль (темп+регістр+палітра).
- **A3_brief_assembler.md** — як зібрати суперТЗ.

## РІВЕНЬ 2 — БІБЛІОТЕКА (будівельні блоки)
### recipes/ — 7 рецептів секцій (повний набір сайту):
R_hero · R_work · R_services · R_about · R_cta · R_nav · R_footer
(кожен: норми тексту + структура DOM + типографіка + анімація з кодом GSAP + інструмент + чеки)
### effects/ — 30 ефектів (3 файли):
- E_scroll_core (E1-E10): fade-up, stagger, pin, split, marquee, magnetic, mask, parallax, sticky-stack, counter.
- E_advanced (E11-E20): horizontal-scroll, text-mask, image-parallax, drag-gallery, line-draw(SVG), scramble, velocity-marquee, hover-distortion(CSS) та ін.
- E_transitions_preloader: прелоадер (лічильник/маска/лого-морф), переходи секцій, page-transition (Flip shared-element).
### patterns/ — 9 карт структур під нішу:
P_agency(+video-production) · P_saas_landing(+AI) · P_realestate(+invest) · P_ecommerce(+DTC) · P_edtech · P_consulting_expert · P_art_culture · P_industrial_deeptech(Phoenix-tec) · P_hospitality(+invest-RE)
P_agency · P_saas_landing · P_realestate · P_ecommerce · P_edtech
(готова карта секцій + норми + емоційна дуга + які рецепти використати)
### _NORMS_reference.md — виробничі норми (символи/екрани/секції з реальних замірів).

## ЯК РОСТЕ:
Бібліотека розширюється (нові рецепти/ефекти/патерни/ніші). META + аналізатори стабільні.
A1 уточнюється новими критеріями з нових розборів.

## СТАН: каркас + наповнена бібліотека (7 рецептів, 30 ефектів, 5 патернів). Готово до тесту.

## РІВЕНЬ 2+ — ПЛЕЙБУКИ (playbooks/) ★ — майстер-класи побудови (17 шт, ПОВНЕ покриття)
> Як ПОБУДУВАТИ кожен аспект крок-за-кроком (типи+дерево рішень+норми+код+помилки+приклади з еталонів). Claude читає перед побудовою.
### Секції (7): PB_hero, PB_work, PB_services, PB_about, PB_cta, PB_nav, PB_footer.
### Аспекти (12): PB_copywriting, PB_typography, PB_motion_system, PB_color, PB_grid_spacing, PB_scroll_choreography, PB_microinteractions, PB_preloader_transitions, PB_responsive, PB_performance, PB_scroll_smoothness(інженерія плавності), PB_app_showcase(телефон+функції).
### _PLAYBOOK_FORMAT.md — формат плейбука.


## 📊 СТАН БАЗИ (оновлено):
- Аналізаторів: 3 | Плейбуків: 19 | Патернів ніш: 9 | Рецептів: 7 | Ефектів: 30 | D-розборів: 21
- Черги скрапу: teardowns_reference/_TIER12_SCRAPE_QUEUE.md (10/12) + _APP_SHOWCASE_QUEUE.md (app-лендинги)
- Карта тек: _FOLDER_MAP.md
