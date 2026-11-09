<sub><a href="../README.md">Back to the notebook</a></sub>

# Real 404s from pages that stream

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-web-dark.svg"><img src="../assets/tag-web.svg" alt="Web" height="22"></picture></a>&nbsp; **Date:** 2026-11-03 (updated 2026-11-09)

## What I built or changed

- On the same Next.js learning platform, a course, module, lesson or certificate that doesn't exist showed a "Page not found" screen, but the server answered HTTP 200 (success).
- Moved the check in front of the page. The proxy (what Next.js 16 calls middleware) looks the address up in the database before anything renders, and answers 404 when it doesn't exist. The page still shows its own not-found screen.
- Changed the existing end-to-end test to expect the 404, ran it against the old code to see it fail ("Received: 200"), then against the fix to see it pass. It also checks that a real page still answers 200.
- Later: on the hosted preview the 404 page lost its title and its menu, while on my PC it was fine. The host answered with the site's plain built-in 404 page instead of the platform's own one. I moved missing pages to a small dedicated page with no loading skeleton, which calls `notFound()` itself, so the framework sends the 404 and keeps the right page and title.

## What I learned

- Streaming sends the status code with the first piece of HTML. A loading skeleton (`loading.tsx`) starts that stream, so a `notFound()` that runs later can only change what the page shows, not the 200.
- Next.js then adds `<meta name="robots" content="noindex">`, so search engines skip the page. Anything that reads status codes, like monitoring or link checkers, still sees a success. Google calls this a "soft 404".
- The check has to happen before streaming starts: in the proxy, or before any `await` or loading boundary in the page. In the proxy it should stay small, one quick lookup, not the page's full data.
- A rewrite in the proxy can carry its own status, and `next start` on my PC renders the page with it. The hosting platform doesn't: a 404 status set in the proxy made it serve the prebuilt root 404 page. Letting the page itself call `notFound()` works the same in both places.
- Passing locally isn't enough for anything that touches status codes or caching. I compared the raw HTML from my PC and from the hosted preview to find the difference, then ran the test against the preview too.
- The page title has to come from the same not-found path (`notFound()` in `generateMetadata` as well), or the right title shows in the first HTML and is then replaced by the default one after the page loads.
- If the lookup itself fails, let the page load and show its normal error message, instead of calling a working page missing.
- Next.js ships its docs inside the installed package (`node_modules/next/dist/docs`). Reading the docs for the exact version I run beat guessing from older answers online.
- Running a new test against the old code first proves it actually catches the bug.

## Tools used

- Next.js 16 (proxy, `loading.tsx`, `notFound()`)
- Supabase (Postgres)
- Playwright for the end-to-end test
- Git and GitHub pull requests

## Link to the project repo

- Private work project, not linked.
