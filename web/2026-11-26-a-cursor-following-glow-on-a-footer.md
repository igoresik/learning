<sub><a href="../README.md">Back to the notebook</a></sub>

# Making a text effect that follows the cursor, with almost no JavaScript

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-web-dark.svg"><img src="../assets/tag-web.svg" alt="Web" height="22"></picture></a>&nbsp; **Date:** 2026-11-26

## What I built or changed

- Added a small effect to the footer of a Next.js company website: the big word in the footer stays dim, and when the pointer moves over it a soft spotlight follows the pointer and lights the letters in the brand colours.
- Split it into a tiny client component and a few lines of CSS, so the footer itself stays a server component.
- Opened one pull request for the main branch and a second one for the development branch, and fixed the conflict where the two stylesheets were organised differently.

## What I learned

- A pointer effect doesn't need React state. The component only writes the pointer position into two CSS custom properties (`--glow-x`, `--glow-y`) on the element, and the CSS does the rest, so the page doesn't re-render on every move.
- How to light up only part of a word: draw a second copy of the text filled with a colour gradient (`background-clip: text`), and show it through a `mask-image` made of a radial gradient centred on the pointer.
- Pointer events cover the mouse and touch with one handler, and the effect fades in on hover with a plain `opacity` transition, so nothing runs until someone moves the pointer.
- A purely decorative effect should be hidden from screen readers (`aria-hidden`) and must not change the layout or take clicks (`pointer-events: none`).
- A hover effect can still be tested: I moved the mouse across the word in Playwright and compared screenshots of the idle state, the left half and the right half.
- Two branches can organise the same stylesheet differently, so a cherry-pick can conflict even on a tiny change. Resolving it meant keeping only my new rules, not the whole conflicting block.

## Tools used

- Next.js, React, Tailwind CSS
- CSS: custom properties, `mask-image`, `background-clip: text`
- Playwright for the hover screenshots
- Git and GitHub pull requests

## Link to the project repo

- Private work project, not linked.
