# 📘 ПЛЕЙБУК — NAV/НАВІГАЦІЯ (sticky, реагує на скрол)
## 1. РОЛЬ: орієнтація без відволікання. «Знає» де користувач (ховається/показується).
## 2. ТИПИ: мінімальний (лого+меню+CTA) · hamburger-overlay (Lusion) · sticky з фоном-при-скролі (ERA) · mix-blend difference (читається на будь-якому фоні).
## 3. ПОБУДОВА: 1) лого ліво + меню/CTA право → 2) fixed + поведінка на скрол (hide-down/show-up) → 3) фон-при-скролі (прозорий→з фоном після 50px) → 4) links hover (underline-draw) → 5) мобільний burger→overlay зі stagger.
## 4. ДЕРЕВО: контентний→sticky hide/show; емоційний/мало пунктів→hamburger-overlay; складна структура→mega-menu.
## 5. НОРМИ: 3-6 пунктів. 13-15px weight 500. Висота 60-90px desktop.
## 6. КОД:
```js
let last=0
ScrollTrigger.create({onUpdate:s=>{const y=s.scroll();
  gsap.to('.nav',{yPercent:(y>last&&y>100)?-100:0,duration:0.4}); last=y}})
```
## 7. ПОМИЛКИ: nav не реагує на скрол · перекриває контент · дефолтне підкреслення links · burger тап-зона <44px · лого не читається на різних фонах.
## 8. ПРИКЛАДИ: Lusion HOME/ABOUT/PROJECTS/LABS+LET'S TALK overlay; ERA sticky-header; mix-blend-difference прийом.
## 9. МОБІЛЬНИЙ: burger→full-overlay, пункти stagger-поява, тап ≥44px.
## 10. ЧЕК: ☐ реагує на скрол ☐ не перекриває ☐ links hover-рух ☐ читається на фонах ☐ мобільне меню зі stagger.
