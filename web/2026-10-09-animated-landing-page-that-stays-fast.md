<sub><a href="../README.md">Back to the notebook</a></sub>

# Built an animated product landing page that stays fast

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-web-dark.svg"><img src="../assets/tag-web.svg" alt="Web" height="22"></picture></a>&nbsp; **Date:** 2026-10-09

## What I built or changed

- Redesigned the website of my lesson-management app (Next.js static export, English and Portuguese).
- A hero where the product is the demo: two phones built in HTML, the teacher's lesson editor and the student's history. It plays by itself and hands over when you touch it: the switch, the stars and Save are real controls, and the lesson lands on the other phone.
- A floating navigation bar that hides on the way down, marks the section being read and opens a full menu on small screens.
- A scroll story that pins on wide screens and becomes a row of cards to swipe on phones, with labels that point at parts of real app screenshots.
- New sections that explain things with a picture instead of a list: who does what on each side, a price statement where every line is zero, and who can see which data.
- A WebGL background, smooth scrolling and a custom cursor.
- A small design system: one set of tokens for type, spacing, radius, shadow and motion, written down with the reasons.
- Sitemap, robots file, web manifest and structured data.

## What I learned

- Measure before designing. One 1.4 MB favicon was three quarters of the page, and the main text was hidden until JavaScript ran. Fixing those two did more than any animation.
- Text that enters with CSS is painted with the HTML. Text that waits for a script is not, and Lighthouse counts that wait as a slow page.
- WebGL can freeze a page even with a tiny shader, because compiling it blocks the main thread. Moving the canvas to a worker (`OffscreenCanvas`) took the blocking time on my test from over a second to about 40 ms.
- Not every visitor needs every script. Smooth scrolling and a cursor only make sense with a mouse, so that code loads only then and phones never download it.
- A simulated network test can disagree with what really happened. I had to read the "observed" numbers next to the simulated ones to understand a result that looked wrong.
- How to make a piece of UI scale like an image: size it in `em`, and set the font size from the container width with container query units.
- A demo that plays by itself should start in its finished state, so it still makes sense without JavaScript or with reduced motion.
- The native `<dialog>` element gives a menu its focus trap, Escape key and focus return for free.
- In React server components, passing text to client components as props keeps large texts (like legal pages) out of the JavaScript bundle.
- Reduced motion is its own design: different layout for the story, a finished demo, no background animation.
- Result on the live site (Lighthouse, mobile): performance 89 to 96, largest contentful paint 3.4 s to 2.4 s, page weight 1.9 MB to 0.4 MB, accessibility still 100.

## Languages

- TypeScript and JSX (React)
- CSS (custom properties, container queries, keyframes, view transitions)
- GLSL for the background shader
- Python for small scripts (icon resizing, bulk text edits)

## Tools used

- Next.js 16, React 19, Tailwind CSS 4
- GSAP and ScrollTrigger, Lenis
- WebGL, Web Workers, Web Animations API, IntersectionObserver
- Lighthouse, Puppeteer (screenshots, long-task traces), ESLint
- Vercel

## Link to the project repo

- Private repository (NotaLivre). Live site: https://nota-livre.vercel.app
