# R06 · Image Displacement / Distortion / Scroll-Velocity Warp (Shaders)
> WebGL-ефекти на зображеннях: hover-displacement, bulge, scroll-velocity warp, RGB-зсув. Те, що робить галереї/портфоліо «живими» (Lusion, Obys, Cuberto).

## ЩО ЦЕ / КОЛИ
Зображення стають WebGL-текстурами й деформуються шейдером: при ховері (displacement map), на швидкості скролу (velocity warp), при наведенні (bulge/ripple), RGB-split. Замість статичних `<img>` — інтерактивна матерія.
**Коли:** галереї проєктів, портфоліо, hero-візуали, list-hover (проєкти Lusion з morph), будь-де де зображення має реагувати.

## РІВЕНЬ РИЗИКУ: 🟡
Claude Code адаптує Codrops-шейдери; ми контролюємо силу ефекту (легко перестаратися — стає «дешево»).

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО (Codrops — готові туторіали з кодом)
- **WebGL Distortion Hover Effects** (tympanus.net/codrops/2018/04/10) — displacement-карта між двома зображеннями при ховері.
- **Creating a Bulge Distortion Effect with WebGL** (2023/06/28, на OGL) — опукле спотворення під курсором.
- **How to Create Distortion and Grain Effects on Scroll** (2024/07/18, Three.js) — scroll-velocity deformation у vertex-шейдері.
- **How to Animate WebGL Shaders with GSAP: Ripples, Reveals, Dynamic Blur** (2025/10/08, Adoratorio).
- **Replicating CSS Object-Fit in WebGL** (2025/03/11) — правильний scale текстури (cover).
- **VFX-JS** (бібліотека, Codrops 2025/01/20) — WebGL-ефекти на DOM-елементах без ручного сетапу.

## КОД-СКЕЛЕТ A — Hover displacement (між img → texture)
```glsl
// fragment
uniform sampler2D uTexture;      // зображення
uniform sampler2D uDisp;         // displacement map (шум/градієнт)
uniform float uHover;            // 0..1 (анімується GSAP при ховері)
varying vec2 vUv;
void main(){
  vec4 disp = texture2D(uDisp, vUv);
  vec2 distortedUv = vUv + (disp.r * 0.1 * uHover) * vec2(1.0, 0.0); // зсув по displacement
  vec4 color = texture2D(uTexture, distortedUv);
  // RGB-split на піку ховера
  color.r = texture2D(uTexture, distortedUv + vec2(0.01*uHover,0.0)).r;
  gl_FragColor = color;
}
```
```js
// GSAP анімує uHover
el.addEventListener('pointerenter', ()=> gsap.to(mat.uniforms.uHover,{value:1,duration:0.6,ease:'power3'}))
el.addEventListener('pointerleave', ()=> gsap.to(mat.uniforms.uHover,{value:0,duration:0.6,ease:'power3'}))
```

## КОД-СКЕЛЕТ B — Scroll-velocity warp (vertex)
```glsl
// vertex — вигин площини за швидкістю скролу
uniform float uScrollVelocity;   // з Lenis (R04: lenis.on('scroll', e=> v=e.velocity))
varying vec2 vUv;
void main(){
  vUv = uv;
  vec3 pos = position;
  float wave = sin(uv.y * 3.1415) * uScrollVelocity * 0.0008;  // вигин
  pos.x += wave;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(pos,1.0);
}
```

## КОД-СКЕЛЕТ C — Bulge під курсором (fragment)
```glsl
uniform vec2 uMouse;     // позиція курсора в uv
uniform float uRadius;   // радіус опуклості
varying vec2 vUv;
void main(){
  float dist = distance(vUv, uMouse);
  float strength = smoothstep(uRadius, 0.0, dist);
  vec2 bulgedUv = vUv + (vUv - uMouse) * strength * 0.3;   // відштовхування від курсора
  gl_FragColor = texture2D(uTexture, bulgedUv);
}
```

## ТОЧНІ ПАРАМЕТРИ
- Hover displacement: сила 0.05-0.12 (більше = «дешево»), GSAP duration 0.5-0.8s power3.
- RGB-split: 0.005-0.015 на піку (ледь помітний).
- Scroll-velocity: множник 0.0005-0.001 (інакше нудить).
- Bulge: radius 0.3-0.5 uv, strength 0.2-0.4.
- Object-fit cover: завжди корегуй uv під aspect ratio (Codrops object-fit recipe), інакше текстура спотворена.

## PERFORMANCE + FALLBACK
- Текстури: WebP, розумна роздільність, KTX2 для великих.
- Десктоп-фіча: на mobile/touch — статичні зображення (hover нема сенсу).
- reduced-motion: без warp/displacement, чисті зображення.
- Velocity-warp: clamp velocity (різкий скрол не має ламати геометрію).
- VFX-JS — якщо треба швидко багато DOM-ефектів без ручних шейдерів.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R06_image-displacement.md.
Зроби WebGL-галерею зображень (R3F) з ефектами:
- hover displacement (displacement-карта, RGB-split, uHover анімується GSAP 0.6s)
- scroll-velocity warp (vertex вигин за Lenis velocity, множник 0.0008, clamp)
- object-fit cover у шейдері (корекція uv під aspect)
- сила ефектів стримана (displacement 0.1, RGB 0.01) — не «дешево»
- mobile/touch: статичні зображення; reduced-motion: без warp
- текстури WebP, lazy
Базуйся на Codrops «Distortion Hover» + «Distortion/Grain on Scroll». Поверни файли + демо-галерею 6 зображень.
```
