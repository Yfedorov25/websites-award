# 📘 ПЛЕЙБУК — RESPONSIVE/МОБІЛЬНИЙ (свідома перебудова, не зламаний desktop)
## 1. РОЛЬ: більшість трафіку мобільна. Award на desktop+зламаний мобільний = провал. Мобільний = свідома реконструкція.
## 2. ПРИНЦИП: не «зменшити desktop», а ПЕРЕБУДУВАТИ композицію під вузький екран.
## 3. ПОБУДОВА: 1) svh замість vh (мобільний бар) → 2) clamp-типографіка вниз (H1 до 36-40px) → 3) сітка 12→1-2 кол → 4) split→стек → 5) тап-зони ≥44px → 6) важкі ефекти спростити/вимкнути (3D, pin, horizontal, кастом-курсор) → 7) меню→burger-overlay → 8) реальний тест на середньому Android.
## 4. БРЕЙКПОІНТИ: 1440/1024/768/480/390. Mobile-first або desktop-down (консистентно).
## 5. ПРАВИЛА: body ≥16px (iOS зум). svh/dvh не vh. Тач замість hover (active-стани). 3D off на слабких (урок IG підвисав). Зображення responsive (srcset). 
## 6. КОД:
```css
.hero__title{font-size:clamp(36px,7vw,120px)} /*вниз до 36*/
.hero{min-height:100svh}
@media(max-width:768px){.split{grid-template-columns:1fr}.cta__btn{width:100%}}
```
```js
const mob=matchMedia('(max-width:768px)').matches
if(mob){/* вимкнути pin/horizontal/3D, спростити stagger */}
```
## 7. ПОМИЛКИ: vh замість svh (бар ріже) · body <16px (iOS зум) · hover-only на тачі · 3D на слабких (лаг) · тап-зони <44px · desktop-композиція втиснута.
## 8. ПРИКЛАДИ: 14islands/NK home=відео (легко мобільному); IG підвисав на resize (урок: 3D off мобільний); hamburger-overlay скрізь.
## 9. ЧЕК: ☐ svh ☐ body≥16px ☐ clamp вниз ☐ split→стек ☐ тап≥44px ☐ важкі ефекти off ☐ burger-меню ☐ реальний тест Android.
