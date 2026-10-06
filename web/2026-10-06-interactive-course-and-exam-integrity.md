# Built interactive lessons and a fair exam system for an online course

**Date:** 2026-10-06

## What I built or changed

- Worked on the same Next.js learning platform: a bookkeeping course with a double-entry simulator.
- Simulator labs inside the lessons. The learner does the exercise in the real simulator on the page, and the server checks every step before the lab counts as done. Before, there was only an "I've done this" button.
- Quiz integrity: copying is turned off, leaving the tab shows a warning, and after 5 warnings (3 on the final exam) the quiz submits itself and the attempt is flagged.
- A per-year journal of every entry, so a closed year still shows what happened in it. Clicking an entry explains where the money came from and where it went.
- A content pipeline: lessons and lab steps are written in Markdown, and a script checks them (every entry must balance) before loading them into the database.
- Animated worked examples: each entry plays out on a see-saw that tips on the first line and levels on the second.
- A redesign of the simulator through four clickable prototypes, then a live accounting equation above it that turns from "≠" to "=" when the books balance.
- Smoother page changes with React view transitions.
- Researched how to turn the course into a credible certificate (accreditation, verifiable certificates, exam rules).

## What I learned

- Never trust the browser to say "I passed". The server rebuilds what the learner did from their raw entries and grades that, and only the server records a pass.
- A browser can count tab switches (the `blur`, `focus` and `visibilitychange` events), but it can't see a phone or a second laptop. Anti-cheat on the web discourages cheating and records evidence; it doesn't prove anything. So the warning count is saved with the attempt for a person to review.
- A `<form>` inside another `<form>` is invalid HTML. The browser's parser drops the inner one, and React then reports a hydration error. I made the outer one a plain element with a button.
- Writing content as a small, readable text format ("Dr Bank 1000, Cr Share Capital 1000") and validating it in a script catches mistakes before learners see them, and non-developers can still read it.
- Playwright's `evaluateAll` doesn't wait for anything. Once pages faded in, tests that read the page too early got empty results. Waiting for the content first fixed several "flaky" tests at once.
- Test events can fire before React attaches its listeners. Retrying the first action until the page reacts is more honest than adding a fixed delay.
- Tests that share one account can race each other (two tests counting the same rows). Checking "at least" instead of "exactly", or giving each test its own account, removes the race.
- Building several clickable prototypes was faster than arguing over screenshots, because the user could only decide once they could use them.
- Formal accreditation (like QQI in Ireland) is a long, expensive process. CPD accreditation and a certificate with a public verification page are the realistic first steps, and the course must never claim exemptions it doesn't have.

## Languages

- TypeScript and JavaScript (React and JSX) for the site, the server actions, the seed script and the tests
- HTML and CSS for the pages and animations
- Markdown and JSON for the course content and lab steps
- Python for one-off scripts that edited many content files at once

## Tools used

- Next.js 16 (view transitions), React 19, Tailwind CSS, next-intl
- Supabase (Postgres)
- GSAP for the see-saw motion
- Playwright and axe for end-to-end and accessibility tests
- Claude Code, including a research agent for the certificate options

## Link to the project repo

- Private work project, not linked.
