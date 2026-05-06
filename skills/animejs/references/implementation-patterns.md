# Implementation Patterns

Adapt these patterns to the host codebase instead of copying them blindly. Keep imports consistent with the project's existing style.

## 1. Basic DOM or CSS Animation

```js
import { animate } from 'animejs';

animate('.card', {
  y: ['1rem', '0rem'],
  opacity: [0, 1],
  duration: 600,
  ease: 'out(3)',
});
```

Use this as the default starting point for DOM and CSS motion.

## 2. Timeline Plus Stagger

```js
import { createTimeline, stagger } from 'animejs';

createTimeline({
  defaults: { duration: 500, ease: 'out(3)' },
})
  .add('.panel', { opacity: [0, 1], y: ['1rem', '0rem'] })
  .add('.panel .item', { opacity: [0, 1], x: ['-0.5rem', '0rem'] }, stagger(60))
  .init();
```

Use this when the request describes phases, beats, or overlapping steps.

## 3. Component-Scoped Pattern

```js
import { createScope, animate, stagger } from 'animejs';

const scope = createScope({
  root: rootRef.current,
  defaults: { ease: 'out(3)', duration: 500 },
}).add(() => {
  animate('.chip', {
    y: ['0.75rem', '0rem'],
    opacity: [0, 1],
    delay: stagger(50),
  });
});

return () => scope.revert();
```

Use this for React or other component systems where cleanup matters.

## 4. Scroll-Driven Pattern

```js
import { animate, onScroll } from 'animejs';

animate('.section-title', {
  y: ['2rem', '0rem'],
  opacity: [0, 1],
  autoplay: onScroll({
    target: '.section-title',
    enter: 'top bottom',
    leave: 'bottom top',
  }),
});
```

Prefer this over manual scroll listeners when the animation is tied to element visibility or scroll progress.

## 5. Text Split Pattern

```js
import { splitText, animate, stagger } from 'animejs';

const split = splitText(titleEl, {
  words: { wrap: 'clip' },
  chars: true,
});

animate(split.chars, {
  y: ['100%', '0%'],
  opacity: [0, 1],
  delay: stagger(18),
  ease: 'out(3)',
});

return () => split.revert();
```

Use `wrap: 'clip'` or `true` when vertical reveal is required. Keep the revert path.

## 6. SVG Line Drawing Pattern

```js
import { animate, svg, stagger } from 'animejs';

animate(svg.createDrawable('.line'), {
  draw: ['0 0', '0 1', '1 1'],
  ease: 'inOutQuad',
  duration: 1600,
  delay: stagger(100),
});
```

Use `svg.createMotionPath()` for path-following transforms and `svg.morphTo()` for actual shape morphs.

## 7. WAAPI Wrapper Pattern

```js
import { waapi, stagger, createSpring } from 'animejs';

waapi.animate('.dot', {
  x: '12rem',
  scale: [1, 0.8, 1],
  ease: createSpring({ stiffness: 160 }),
  delay: stagger(50),
  loop: true,
  alternate: true,
});
```

Use this for CSS-heavy motion where native execution is the better tradeoff.

## 8. Draggable Pattern

```js
import { createDraggable, animate } from 'animejs';

const draggable = createDraggable('.drawer', {
  container: () => [0, drawer.offsetWidth, drawer.offsetHeight, 0],
  x: false,
  y: { snap: () => drawer.offsetHeight },
});

button.onclick = () => {
  animate(draggable, {
    progressY: draggable.progressY > 0.5 ? 0 : 1,
    duration: 375,
    ease: 'out(4)',
  });
};
```

Use the draggable object itself as an animation target when toggling open or closed states.

## 9. Layout Transition Pattern

```js
import { createLayout } from 'animejs';

const layout = createLayout('.grid', {
  duration: 500,
  ease: 'out(3)',
});

layout.update(() => {
  grid.prepend(grid.lastElementChild);
});
```

Use `createLayout()` when DOM reordering is the source of the motion.

## 10. Imperative Frame Loop Pattern

```js
import { createAnimatable, createTimer } from 'animejs';

const cursor = createAnimatable('.cursor', { x: 0, y: 0 });

createTimer({
  onUpdate: () => {
    cursor.x(nextX);
    cursor.y(nextY);
  },
});
```

Use this when animation state is updated by external input on every frame.

## Sources

- https://animejs.com/documentation/animation
- https://animejs.com/documentation/events/onscroll/
- https://animejs.com/documentation/text/splittext
- https://animejs.com/documentation/svg/createdrawable/
- https://animejs.com/documentation/scope/
- https://github.com/juliangarnier/anime
