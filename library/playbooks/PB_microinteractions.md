# 📘 ПЛЕЙБУК — МІКРОВЗАЄМОДІЇ (відрізняють $$ від $)
## 1. РОЛЬ: дрібні реакції на курсор/дію = відчуття якості й «живого інтерфейсу». Те, що запам'ятовується.
## 2. ТИПИ: кастомний курсор · магнітні кнопки · hover-картки (scale/overlay) · underline-draw links · кнопки (fill-slide/text-swap/стрілка-рух) · focus/active стани.
## 3. ПОБУДОВА: 1) обери чи потрібен кастом-курсор (агенція/портфоліо так) → 2) магнітні ключові CTA → 3) hover на всі картки (scale 1.03-1.05) → 4) links underline-draw → 5) усі стани (hover/focus/active/disabled) → 6) тривалість 0.3-0.4s.
## 4. ДЕРЕВО: агенція/креатив→кастом-курсор+магніт; продукт/B2B→hover+focus чисті; мінімалізм→тільки delικатні hover.
## 5. ПРАВИЛА: hover не миттєвий (0.3-0.4s). Усі інтерактивні елементи реагують. Focus-стани для accessibility. Курсор доречний (не скрізь). 
## 6. КОД:
```js
// кастом-курсор (база Cuberto cuberto/mouse-follower — open source)
const cur=document.querySelector('.cursor'); let x=0,y=0,cx=0,cy=0
addEventListener('mousemove',e=>{x=e.clientX;y=e.clientY})
const loop=()=>{cx+=(x-cx)*0.15;cy+=(y-cy)*0.15;cur.style.transform=`translate(${cx}px,${cy}px)`;requestAnimationFrame(loop)};loop()
// hover-картка
card.onmouseenter=()=>gsap.to(card,{scale:1.03,duration:0.4,ease:'power2.out'})
```
## 7. ПОМИЛКИ: миттєвий hover (0s) · нема focus-станів · кастом-курсор що ламає UX · нема feedback на дії · стани не всі.
## 8. ПРИКЛАДИ: Cuberto .cb-cursor (open source!); магнітні CTA; hover image-scale на картках work.
## 9. МОБІЛЬНИЙ: кастом-курсор+магніт ПРИБРАТИ (нема курсора), лишити tap-feedback, active-стани.
## 10. ЧЕК: ☐ hover з рухом 0.3-0.4s ☐ усі інтерактивні реагують ☐ focus-стани ☐ курсор доречний ☐ desktop-only ефекти прибрані на мобільному.
