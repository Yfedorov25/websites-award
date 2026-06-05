# Epic Agency · Teardown #21 — epic.net (Layer 3 + MOTION) ✅
> Еталон #19. Льєж, Бельгія. «Digital agency, blend of technical/strategic/creative.» branding+web+WebGL-ігри. Клієнти: Red Bull, Galler.
> **Категорія:** MOTION/WebGL + branding.

## СТЕК (probe)
- **Vue (без Nuxt) + WebGL2 + GSAP** (gsapVersions=true). 2 canvas, 11 скриптів. Lenis=false. Перший «чистий Vue» (не Nuxt).
- Шрифти (3, ДВІ антикви!): **Sang Bleu** (розкішна fashion-антиква, Swiss Typefaces) + **PP Hatton** (антиква, Pangram Pangram) + Inter. Найбільш luxury/editorial типосистема досі.
- База: тепло-вугільна `#272928`.

## АНАТОМІЯ РУХУ (виміряно)
- Змішаний широкий темп: швидкі 0.15s (11×), 0.2s (13×), 0.25s (5×) + повільні 1s (8×) + рідко 2s. Двошарово: швидкий UI + повільні драматичні моменти.
- Easing: **`cubic-bezier(0.25,0.46,0.45,0.94)`** (easeOutQuad — **4-те підтвердження!** Dogstudio/14islands/Aristide/Epic).

### Емоція:
Дві антикви + тепла вугільна база + двошаровий темп = розкішно-редакторське з технічною чіткістю (їхній «blend of technical+creative»).

## ЧОГО ВЧИМОСЯ / В БАЗУ
- **easeOutQuad 4-те підтвердження** → остаточно ДЕФОЛТ індустрії. Беремо як базову криву.
- **Дві антикви + гротеск** (Sang Bleu + PP Hatton + Inter) = максимальний luxury/editorial → toolbox типосистем.
- **Sang Bleu** → toolbox (fashion-рівень антиква).
- **Двошаровий темп** (швидкий UI 0.15-0.25s + повільні драми 1-2s) → motion-system.

## TODO
- Далі #20 Use All Five.
