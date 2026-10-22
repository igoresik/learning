# Art-directed a product page around typography and real screenshots

**Date:** 2026-10-13

## What I built or changed

- Redesigned the website of my lesson-management app a second time, from a written brief: warm, editorial, premium (Next.js static export, English and Portuguese).
- A new visual system: ivory paper, ink, a serif for headings (Newsreader) with a sans for text (Onest), and the app's purple kept for links and markers only.
- A hero built from real app screenshots: the student's history in front, the teacher's lesson editor behind, and a label pointing at the homework to match the headline "Every lesson leaves a clear next step."
- The working two-phone demo moved to its own section, with three steps beside it that light up as the demo reaches them.
- Three product chapters with screenshots instead of a long pinned scroll story, and a real table for "who does what" that turns into blocks on phones.
- New share images for both languages, rendered from an HTML page.
- Work is on a branch, not yet live.

## What I learned

- A design brief is easier to follow when I write the system down first: a few named colours, two typefaces with clear jobs, one label style, three animation speeds. After that most decisions are already made.
- Typography can carry a page. Regular weight with tight tracking at large sizes looks more refined than a heavy serif, and a variable font's optical-size axis gives large headings a finer cut than small text.
- That axis has a price. The full Newsreader file was 132 KB; the same font with one weight is 57 KB. Google Fonts serves different files depending on the axis ranges asked for, so I compared them before choosing.
- An italic used only for small labels does not need to be preloaded. Splitting it into its own font declaration took it off the critical path.
- Lighthouse picked the logo as the largest element because the headline and intro text started invisible. Entrances that only move (no opacity 0) let the first frame count. Local score went from 85 to 95.
- A pinned scroll section can be replaced by plain sections. The old story pinned the page for about 4,800 px; three chapters say the same and need no scroll library.
- CSS scroll-driven animation (`animation-timeline: view()`) gives a small parallax with no JavaScript, inside `@supports` so other browsers just show a still image.
- An IntersectionObserver on a 1 px marker at the top of the page replaces a scroll listener for "has the page moved?".
- Translations wrap differently. I set headline lines per language and used a non-breaking hyphen so "Regista-se" does not split.
- How to test many widths when the browser window will not resize: load the page in same-origin iframes of fixed widths and compare `scrollWidth` with `clientWidth` to find overflow.
- A background browser window reports `visibilityState: hidden`, and code that waits to be visible does not start. That is the test setup, not a bug.

## Tools used

- Next.js 16, React 19, Tailwind CSS 4, TypeScript
- `next/font` (Google and local), Newsreader and Onest (SIL Open Font License)
- CSS: custom properties, scroll-driven animations, container queries, `clip-path`, view transitions
- Lighthouse, headless Edge for screenshots and share images, ESLint
- Node scripts for bulk text and CSS edits

## Link to the project repo

- Private repository (NotaLivre). Live site (still the previous version): https://nota-livre.vercel.app
