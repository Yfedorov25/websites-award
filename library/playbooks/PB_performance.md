# 📘 ПЛЕЙБУК — ПЕРФОРМАНС (швидкість = гроші + SEO)
## 1. РОЛЬ: повільний award-сайт = втрачені клієнти + Google карає. Core Web Vitals критичні.
## 2. ЦІЛІ: LCP <2.5s, INP <200ms, CLS <0.1.
## 3. ПОБУДОВА: 1) home легкий (відео-hero не real-time 3D — урок 14islands/NK) → 2) зображення webp/avif+lazy+srcset → 3) шрифти woff2+preload+font-display:swap → 4) 3D-ассети Draco+KTX2 (drei) якщо є → 5) код-спліт (Next dynamic) → 6) анімація transform/opacity (GPU) → 7) без layout shift.
## 4. ПРАВИЛА: текст у DOM (SEO — урок AT vs IG). Семантичні теги+meta. Critical CSS inline. Відео poster+preload=metadata. Lenis+GSAP легкі (не важкі бібліотеки).
## 5. КОД:
```js
// Next dynamic для важкого
const Scene=dynamic(()=>import('./Scene'),{ssr:false})
// шрифт preload: <link rel=preload as=font type=font/woff2 crossorigin>
// зображення: <img loading=lazy srcset=... > або next/image
```
## 6. ПОМИЛКИ: real-time 3D на home (повільно) · png замість webp · шрифти без preload (FOUT/shift) · текст у canvas (нуль SEO) · анімація width/top (reflow) · важкі бібліотеки.
## 7. ПРИКЛАДИ: 14islands/NK home=відео; IG/AT Draco+KTX2 оптимізація; AT помилка — текст у WebGL (нуль SEO) vs IG текст у DOM.
## 8. ЧЕК: ☐ LCP<2.5 ☐ INP<200 ☐ CLS<0.1 ☐ home легкий ☐ webp/avif+lazy ☐ woff2+preload ☐ Draco/KTX2 для 3D ☐ текст у DOM ☐ transform/opacity only.
