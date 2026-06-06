# 🗂️ КАРТА ТЕК БАЗИ (куди що класти — правило структури)
> Усе нове кладеться строго за цією картою. Тримаємо чистоту.

ScrollCraft_PRO_v2/
├── START_HERE.md            # точка входу
├── _SYSTEM_INDEX.md         # повний індекс системи
├── _FOLDER_MAP.md           # ця карта
├── META_PROMPT_v2.md        # оркестратор (7 фаз)
│
├── analyzers/               # ЯК ДУМАТИ (мозок)
│   └── A1 критерії · A2 діагностика · A3 збірка ТЗ
│
├── library/                 # БУДІВЕЛЬНІ БЛОКИ
│   ├── _NORMS_reference.md  # виробничі норми (числа з замірів)
│   ├── playbooks/           # ЯК БУДУВАТИ — майстер-класи (PB_*)
│   ├── patterns/            # ЩО СТАВИТИ — карти ніш (P_*)
│   ├── recipes/             # ТОЧНІ блоки секцій (R_*)
│   └── effects/             # GSAP-ефекти (E_*)
│
├── 00_foundations/          # meaning-copy, motion-system, philosophy, toolbox
├── copywriting/             # Федорів, анти-слоп, ніша-профілі, мікрокопі
│
├── teardowns_reference/     # ПРИКЛАДИ — розбори award-сайтів
│   ├── D_*.md               # глибокі розбори (1 файл = 1 сайт)
│   ├── _*_QUEUE.md          # черги скрапу
│   └── _ANALYSIS / _SCHEMA / _REGISTRY
│
└── _archive/                # старе (TIER3 WebGL recipes, old research) — не чіпати

## ПРАВИЛА КЛАСТИ:
- Новий розбір сайту → teardowns_reference/D_Назва.md
- Новий майстер-клас аспекту/секції → library/playbooks/PB_назва.md
- Нова карта ніші → library/patterns/P_назва.md
- Новий рецепт секції → library/recipes/R_назва.md
- Новий ефект → library/effects/E_назва.md
- Норми оновлювати в library/_NORMS_reference.md
- Після КОЖНОГО додавання — оновити _SYSTEM_INDEX.md + перезібрати ZIP.
