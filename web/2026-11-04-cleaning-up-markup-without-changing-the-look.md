<sub><a href="../README.md">Back to the notebook</a></sub>

# Cleaning up a site's markup without changing how it looks

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-web-dark.svg"><img src="../assets/tag-web.svg" alt="Web" height="22"></picture></a>&nbsp; **Date:** 2026-11-04

## What I built or changed

- Ran a Next.js marketing site through a public "does this look vibe-coded" checker. It scored 29.2, where lower means more hand-made. I read the checker's raw numbers to see what it actually measures before changing anything.
- Moved the Tailwind class groups the site repeats everywhere (buttons, cards, nav links, form fields, the FAQ) into named classes in the stylesheet. The legal pages got one scoped stylesheet instead of a class on every tag. Active, open and error looks now come from ARIA attributes.
- Built a check that records the computed style of every element, on every page, at desktop and mobile size, including hover, focus and open menus. I ran it before and after and compared the results: nothing changed.
- In the same round: the browser now only gets the translations it uses, the site went down to four languages with redirects for old links, and every language got a generated share image for link previews.
- The score went from 29.2 to 23.2, and the homepage HTML from about 154 KB to 107 KB.

## What I learned

- Read a tool's scale before reacting to it. I first read the score backwards; the raw numbers showed which parts were actually bad.
- Detectors like this look at surface signals, such as how many utility classes each element has. I only made changes that also make the site better, and left alone the signals that can't be removed without rewriting the framework.
- In Tailwind v4, `@layer components` with `@apply` produces the same CSS under a readable name, and utilities written in the markup still win because of layer order.
- Styling from `aria-current`, `aria-expanded` and `aria-invalid` keeps what you see and what a screen reader hears in sync.
- Comparing computed styles is a stricter safety net for a refactor than screenshots. It also catches hover and focus states and differences of a single pixel.
- next-intl sends every translation to the browser unless the provider gets only the parts the client components need.
- Next.js image routes like `opengraph-image` don't receive the layout's language parameter at build time, so they need their own `generateStaticParams`.

## Tools used

- Next.js 16, Tailwind CSS v4, next-intl
- Playwright for the computed-style comparison
- Vercel preview deployments to scan each change before merging
- Git and GitHub pull requests

## Link to the project repo

- Private work project, not linked.
