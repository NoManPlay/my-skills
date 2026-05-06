# Animation Adaptation Matrix

Use this file when the task says "make it animate" but does not already imply the right Anime.js surface.

## Primary Mapping

| Request shape | Primary API | Common companions | Notes |
| --- | --- | --- | --- |
| Single DOM or CSS motion | `animate()` | `utils`, `stagger()` | Default choice for most UI motion |
| Ordered multi-step choreography | `createTimeline()` | `stagger()`, `animate()` | Prefer timeline offsets over nested callbacks |
| List or grid entrance, exit, wave | `animate()` | `stagger()` | Use stagger for index, center, random, or range-based delay |
| Scroll-triggered or scroll-synced motion | `onScroll()` | `animate()`, `createTimeline()`, `createScope()` | Use `autoplay: onScroll(...)` or link a scroll observer to an existing instance |
| Responsive or component-scoped animation | `createScope()` | `animate()`, `createTimeline()`, `onScroll()` | Best default for React or component lifecycle cleanup |
| Text reveal, hover, shuffle, per-char motion | `splitText()` | `animate()`, `createTimeline()`, `stagger()` | Always keep `revert()` available |
| SVG line drawing | `svg.createDrawable()` | `animate()`, `createTimeline()` | Animate the synthetic `draw` property |
| SVG motion path | `svg.createMotionPath()` | `animate()` | Use for path-following transforms |
| SVG morphing | `svg.morphTo()` | `animate()` | Use only when shape morphing is actually required |
| Drag, snap, carousel, flick, gesture | `createDraggable()` | `animate()`, `createTimeline()`, `createAnimatable()` | Avoid hand-written pointer physics unless the built-in surface is insufficient |
| Layout transitions after DOM reorder | `createLayout()` | `stagger()`, `utils` | Use when DOM structure changes drive the animation |
| High-frequency imperative state updates | `createAnimatable()` | `createTimer()`, `engine` | Good for pointer-follow, continuous value updates, or mixed state |
| JS object, canvas, WebGL, WebGPU animation | `animate()` | `createTimer()`, `engine` | Stay on JS-driven animation, not WAAPI |
| Performance-first CSS animation | `waapi.animate()` | `stagger()`, `createSpring()` | Good for native browser execution and lighter bundle size |

## WAAPI vs JS

Prioritize `waapi.animate()` when:

- Animating during CPU or network load where native execution matters
- Initial payload size is important
- The animation mainly targets CSS properties and transforms
- Native handling of certain CSS values is more reliable than the JS path

Prefer `animate()` when:

- Animating a large number of targets, especially more than 500
- Animating JS objects, canvas, WebGL, or WebGPU data
- Animating SVG, DOM attributes, or properties WAAPI does not cover well
- Building complex timelines or keyframe logic
- Needing richer control methods or callback behavior

## Scope Heuristics

Use `createScope()` by default when:

- The animation lives inside a React, Next, Vue, or Svelte component
- The task needs media-query-specific behavior
- The task adds event listeners, draggables, or scroll observers that must be cleaned up
- The animation must be rebuilt on resize or responsive breakpoints

Avoid global selectors in component code when a root ref is available.

## Failure Modes to Check

- Existing transforms are overwritten instead of composed
- `splitText()` mutates markup without a matching `revert()`
- `onScroll()` or `createDraggable()` listeners survive unmount
- WAAPI is chosen for JS-object or SVG work where it cannot express the requirement cleanly
- A sequence is implemented as chained callbacks instead of a timeline

## Sources

- https://animejs.com/documentation/web-animation-api/when-to-use-waapi/
- https://animejs.com/documentation/events/onscroll/
- https://animejs.com/documentation/scope/
- https://animejs.com/documentation/svg
- https://github.com/juliangarnier/anime
