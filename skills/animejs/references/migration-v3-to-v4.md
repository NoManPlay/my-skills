# Migration v3 to v4

Default this skill to v4. If the host project still uses v3 patterns, apply these renames and behavior changes first.

## Core API Changes

| v3 | v4 |
| --- | --- |
| `import anime from 'animejs'` | `import { animate } from 'animejs'` |
| `anime({ targets: '.box', ... })` | `animate('.box', { ... })` |
| `easing` | `ease` |
| `value` inside property objects | `to` |
| `endDelay` | `loopDelay` |
| `direction: 'reverse'` | `reversed: true` |
| `direction: 'alternate'` | `alternate: true` |
| `round: 100` | `modifier: utils.round(2)` |
| `anime.timeline(...)` | `createTimeline(...)` |
| top-level timeline child defaults | `defaults: { ... }` inside timeline config |
| `begin`, `complete`, `update` | `onBegin`, `onComplete`, `onUpdate` |
| `change` | `onRender` |
| `loopBegin` and `loopComplete` | `onLoop` |
| `animation.finished` | `animation.then()` |
| `anime.path()` | `svg.createMotionPath()` |
| `anime.setDashoffset()` | `svg.createDrawable()` |
| spring string in `easing` | `createSpring({...})` passed to `ease` |
| `anime.get`, `anime.set`, `anime.random` | `utils.get`, `utils.set`, `utils.random` |

## Behavior Changes That Break Silent Assumptions

- `loop` now means repeat count, not total iteration count.
- `play()` always plays forward. Use `resume()` to continue in the last direction.
- `reverse()` always plays backward. Use `alternate()` to flip direction.
- `onBegin` now runs after delay, not immediately.
- Function-based custom easing is passed directly to `ease`; do not wrap it in another function.

## Manual Engine Control

If old code relied on manual ticking, switch to the v4 engine:

```js
import { engine } from 'animejs';

engine.useDefaultMainLoop = false;

function render() {
  engine.update();
}
```

Also replace `anime.suspendWhenDocumentHidden` with `engine.pauseOnDocumentHidden`.

## Migration Triage

When touching a legacy codebase:

1. Replace imports and the `targets` option first.
2. Rename `easing`, `value`, `direction`, and callback keys.
3. Replace timelines and SVG helpers.
4. Re-check loop behavior and any code that depends on `play()` or `reverse()`.
5. Re-check promises and teardown logic.

## Sources

- https://github.com/juliangarnier/anime/wiki/Migrating-from-v3-to-v4
- https://animejs.com/documentation/getting-started/module-imports/
