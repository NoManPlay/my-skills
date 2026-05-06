# Module Map

Anime.js v4 is modules-first. Default to the main `animejs` entrypoint unless the repo already prefers subpath imports or only needs one narrow subsystem.

## Main Import Rule

Use the main module when you need a mixed stack:

```js
import { animate, createTimeline, stagger, splitText, onScroll } from 'animejs';
```

Use subpaths when tree-shaking is weak, when the repository already uses subpaths, or when you need only one subsystem.

## Confirmed Subpaths

These subpaths are exported by the package and documented in the official v4 module-import guide:

| Subpath | Typical export to use | Purpose |
| --- | --- | --- |
| `animejs/animation` | `animate` | General JS-driven animation |
| `animejs/timer` | `createTimer` | Timer and frame-loop work |
| `animejs/timeline` | `createTimeline` | Sequencing and choreography |
| `animejs/animatable` | `createAnimatable` | Imperative animation state |
| `animejs/draggable` | `createDraggable` | Drag, snap, flick, gestures |
| `animejs/layout` | `createLayout` | Layout and DOM reflow transitions |
| `animejs/scope` | `createScope` | Component scope and responsive rebuilds |
| `animejs/engine` | `engine` | Manual loop and global engine settings |
| `animejs/events` | `onScroll` | Scroll observers and scroll playback |
| `animejs/easings` | easing helpers | Shared easing entrypoint |
| `animejs/easings/eases` | preset eases | Named ease helpers |
| `animejs/easings/linear` | linear helpers | Linear easing helpers |
| `animejs/easings/steps` | step helpers | Steps easing helpers |
| `animejs/easings/irregular` | irregular helpers | Irregular easing helpers |
| `animejs/easings/cubic-bezier` | bezier helpers | Cubic bezier helpers |
| `animejs/easings/spring` | `createSpring` | Spring generation |
| `animejs/utils` | `utils`, `stagger`, `random` | DOM and math helpers |
| `animejs/svg` | `morphTo`, `createMotionPath`, `createDrawable` | SVG helpers |
| `animejs/text` | `splitText` | Text splitting |
| `animejs/waapi` | WAAPI-specific animation API | Native Web Animations wrapper |

## Import Notes

- The official docs confirm direct main-module imports for `animate`, `splitText`, `stagger`, SVG helpers, and other common APIs.
- The docs explicitly confirm subpath imports such as `createScope` from `animejs/scope`, `onScroll` from `animejs/events`, `splitText` from `animejs/text`, and SVG helpers from `animejs/svg`.
- For the WAAPI surface, the main-module form `waapi.animate(...)` is fully documented. If the codebase already uses the `animejs/waapi` subpath, follow the local typings and style.
- Match the style already used in the host repository unless there is a clear reason to normalize imports.

## Selection Shortcut

- Mixed or uncertain scope: import from `animejs`
- Component lifecycle: `animejs/scope`
- Scroll-only work: `animejs/events`
- Text-only work: `animejs/text`
- SVG-only work: `animejs/svg`
- Manual render-loop integration: `animejs/engine`

## Sources

- https://animejs.com/documentation/getting-started/module-imports/
- https://animejs.com/documentation/scope/
- https://animejs.com/documentation/events/onscroll/
- https://animejs.com/documentation/text/splittext
- https://animejs.com/documentation/svg
- https://github.com/juliangarnier/anime/blob/master/package.json
