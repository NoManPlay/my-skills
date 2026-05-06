---
name: animejs
description: Anime.js v4 implementation and debugging guide for building, adapting, migrating, and validating animations across DOM and CSS, SVG, text splitting, scroll-linked motion, draggable interactions, layout transitions, JS-object animation, and WAAPI. Use when a task explicitly mentions anime.js, animejs, or anime.js imports, when code already imports from animejs, when a project needs v3-to-v4 migration, or when the request involves timeline, stagger, scope, splitText, onScroll, createDraggable, createLayout, SVG motion path or line drawing, or WAAPI animation work.
---

# Anime.js

## Overview

Use this skill to convert a motion requirement into the narrowest Anime.js v4 solution that fits the real codebase. Prefer official v4 APIs, integrate into the existing component or module, and leave a clean revert path for anything stateful.

## Workflow

1. Detect version and runtime first.
- Inspect `package.json`, lockfiles, and imports before writing code.
- If the codebase already uses v3 patterns such as `anime({... targets ...})`, read [references/migration-v3-to-v4.md](references/migration-v3-to-v4.md) before editing.
- Note the runtime: plain DOM, React or Next, Vue or Svelte, canvas, WebGL, SSR, or a mixed environment.

2. Classify the animation request.
- Single DOM or CSS animation
- Ordered choreography or sequence
- List or grid offsets
- Scroll-triggered or scroll-synchronised motion
- Text split or per-word or per-char effect
- SVG line drawing, motion path, or morphing
- Drag, flick, snap, carousel, or gesture work
- Layout transition after DOM reordering
- High-frequency imperative updates or custom render loop
- WAAPI-first or performance-first CSS animation

3. Choose the smallest Anime.js surface that fits.
- Read [references/adaptation-matrix.md](references/adaptation-matrix.md) first when the request spans multiple animation types.
- Default to `animate()` for most DOM, CSS, SVG, and JS-object animation work.
- Promote to `createTimeline()` when order, offsets, or multi-step composition matter.
- Add `stagger()` instead of hand-written per-item delay loops.
- Use `createScope()` for component-scoped, responsive, or teardown-sensitive code.
- Use `waapi.animate()` only when the tradeoffs in the adaptation matrix clearly favor WAAPI.

4. Implement inside the real module, not as a detached demo.
- Match the existing import style. Use the main `animejs` entrypoint unless the repo already prefers subpath imports.
- Reuse existing refs, selectors, tokens, and CSS variables.
- Keep selectors local to a root element in component code.
- Preserve existing transforms and inline styles whenever possible.
- If you create a scope, text split, layout, draggable, scroll observer, or animatable, expose or return a `revert()` cleanup path.

5. Validate behavior before finishing.
- Check for SSR-safe DOM access.
- Check unmount, route-change, and teardown cleanup.
- Check reduced-motion handling.
- Check transform composition, loop semantics, and callback timing.
- Check scroll bounds, drag bounds, and pointer cleanup.
- Check that text split output remains accessible and reversible.

## Selection Rules

- Use `animate()` for general-purpose DOM or CSS transforms, opacity, SVG values, and JS-object animation.
- Use `waapi.animate()` when CPU or network load tolerance, smaller payload, or better native handling of some CSS values is more important than advanced JS-side control.
- Use `createTimeline()` when sequencing matters more than one-off interpolation.
- Use `stagger()` for entrances, exits, waves, grids, carousels, and index-based offsets.
- Use `splitText()` for line, word, and char-level text effects. Always plan `revert()`.
- Use `svg.createDrawable()` for line drawing, `svg.createMotionPath()` for path following, and `svg.morphTo()` for shape morph work.
- Use `onScroll()` for trigger-based or progress-synced scroll motion instead of manual scroll listeners.
- Use `createDraggable()` for drag, snap, momentum, and carousel behavior instead of hand-rolled pointer math.
- Use `createLayout()` when the task is about animating DOM reflow after adding, removing, or reordering elements.
- Use `createAnimatable()` or `createTimer()` when animation state must be updated imperatively on every frame.
- Use `engine.update()` only for custom render-loop integration.

## Framework Guardrails

- In React or Next client components, create Anime.js instances inside an effect and return cleanup with `scope.revert()` or instance `revert()`.
- Prefer `createScope({ root, defaults, mediaQueries })` for component code that needs lifecycle cleanup or responsive behavior.
- When adding DOM event listeners inside a scope constructor, return a cleanup function or register reusable methods with `scope.add(name, fn)`.
- If an animation should survive media-query refreshes inside a scope, consider `addOnce()` instead of re-creating it on every refresh.

## Output Expectations

- Produce integrated code, not isolated playground snippets, unless the user explicitly asks for a demo.
- If the request is broad, first map it to one or more capability buckets using the adaptation matrix, then implement each bucket with the correct API surface.
- If the project mixes animation systems, edit only the part the user asked for and avoid rewriting unrelated motion code.
- When a request sounds ambiguous, infer the closest Anime.js surface from the existing codebase instead of asking open-ended questions.

## References

- Read [references/adaptation-matrix.md](references/adaptation-matrix.md) for feature selection and WAAPI vs JS tradeoffs.
- Read [references/implementation-patterns.md](references/implementation-patterns.md) for copyable implementation shapes.
- Read [references/module-map.md](references/module-map.md) for imports, subpaths, and module boundaries.
- Read [references/migration-v3-to-v4.md](references/migration-v3-to-v4.md) when old snippets or legacy code appear.
