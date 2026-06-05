# R21 · Dithering / ASCII / Halftone Post-Shaders
> Хвиля 3. Retro-tech естетика 2026. Post-processing pass: ordered dithering (Bayer), ASCII-арт, halftone. Реальне репо: github.com/damarberlari/visualizing-dithering-codrops.

## ЩО ЦЕ / КОЛИ
Повноекранний post-фільтр, що стилізує рендер під ретро/принт: dithering (Bayer-патерн, як старі ПК/Game Boy), ASCII (символи замість пікселів), halftone (друкарські крапки). Тренд retro-tech/80s 2026.
**Коли:** retro/tech-бренди, editorial/artsy сайти, music/culture, hero-стилізація, hover-стейт «декодування» зображення. Дозовано — як характерна деталь, не на весь сайт.

## РІВЕНЬ РИЗИКУ: 🟡
Post-pass через @react-three/postprocessing або кастомний shader. Claude Code адаптує з репо.

## ПЕРЕВІРЕНЕ ДЖЕРЕЛО
- **github.com/damarberlari/visualizing-dithering-codrops** (Three 0.175 + tweakpane). Codrops Apr 2026 «Animating 160,000 Cubes with Dithering».
- @react-three/postprocessing — `<Dithering>` ефект (вбудований).
- **emilwidlund/ASCII** — ASCII post-effect для postprocessing.
- Codrops: dithering (Jun 2025), «Efecto ASCII» (Jan 2026).
- postprocessing (pmndrs) — база для кастомних Effect.

## ПРИНЦИП
- **Dithering:** порівняти яскравість пікселя з порогом з 4×4 (або 8×8) Bayer-матриці → чорний/білий (або квантувати в N рівнів/кольорів палітри).
- **ASCII:** розбити екран на комірки (8×8px), за середньою яскравістю комірки вибрати символ зі шкали (` .:-=+*#%@`), намалювати його.
- **Halftone:** комірки → крапки, радіус ∝ яскравість.

## КОД-СКЕЛЕТ A — Dithering (готовий drei/postprocessing)
```jsx
import { EffectComposer } from '@react-three/postprocessing'
import { Dithering } from '@react-three/postprocessing'   // вбудований
<EffectComposer>
  <Dithering />
</EffectComposer>
```

## КОД-СКЕЛЕТ B — Dithering власний (Bayer 4×4, GLSL)
```glsl
// fragment post-pass
uniform sampler2D tDiffuse; varying vec2 vUv;
const mat4 bayer = mat4(
  0.0,  8.0,  2.0, 10.0,
 12.0,  4.0, 14.0,  6.0,
  3.0, 11.0,  1.0,  9.0,
 15.0,  7.0, 13.0,  5.0) / 16.0;
void main(){
  vec3 c = texture2D(tDiffuse, vUv).rgb;
  float lum = dot(c, vec3(0.299,0.587,0.114));
  ivec2 p = ivec2(mod(gl_FragCoord.xy, 4.0));
  float threshold = bayer[p.x][p.y];
  float v = step(threshold, lum);
  gl_FragColor = vec4(vec3(v), 1.0);   // або mix двох кольорів палітри
}
```

## КОД-СКЕЛЕТ C — ASCII (концепт)
```glsl
// розбити на 8px-комірки, яскравість → індекс символу з atlas-текстури символів
vec2 cell = floor(gl_FragCoord.xy / 8.0) * 8.0;
float lum = luminance(texture2D(tDiffuse, cell/resolution));
float charIndex = floor(lum * 10.0);  // 0..9 → " .:-=+*#%@"
// семпл символу з msdf/char-atlas за charIndex + локальний uv комірки
```
Або готове: `emilwidlund/ASCII` як postprocessing Effect.

## ТОЧНІ ПАРАМЕТРИ
- Dithering: Bayer 4×4 (грубіше) або 8×8 (тонше); квантування 2-8 рівнів; опційно палітра (2-4 кольори).
- ASCII: комірка 6-10px; шкала символів 8-12 рівнів; моноширинний шрифт.
- Halftone: dot scale, кут растру (15°/45°).
- Анімація: зсув порогу/палітри в часі = «живий» retro-екран.

## PERFORMANCE + FALLBACK
- Post-pass дешевий (1 fullscreen quad). ASCII з atlas-семплом трохи дорожче.
- Можна застосувати лише до частини (hover-reveal зображення).
- reduced-motion: статичний фільтр без анімації порогу.
- Mobile: dithering ок; ASCII — більші комірки.

## ПРОМПТ ДЛЯ CLAUDE CODE
```
Прочитай recipes/R21_dithering-ascii-halftone.md.
Зроби retro post-фільтр (@react-three/postprocessing або кастомний Effect):
- dithering: Bayer 4×4, квантування у 2-кольорову палітру (drei <Dithering> або власний GLSL вище)
- (опц.) ASCII (8px комірки, шкала " .:-=+*#%@", char-atlas) або halftone (крапки ∝ яскравість)
- анімований поріг/палітра для «живого» екрана
- застосувати глобально АБО лише hover-reveal зображення
- mobile → більші комірки; reduced-motion → статичний фільтр
Репо-орієнтир: damarberlari/visualizing-dithering-codrops, emilwidlund/ASCII. Поверни Effect + демо.
```
