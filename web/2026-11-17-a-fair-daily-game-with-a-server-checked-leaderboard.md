<sub><a href="../README.md">Back to the notebook</a></sub>

# Building a daily game with a leaderboard and badges that the server can trust

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-web-dark.svg"><img src="../assets/tag-web.svg" alt="Web" height="22"></picture></a>&nbsp; **Date:** 2026-11-17

## What I built or changed

- Added games to the same Next.js learning platform, starting from a standalone prototype I had built the day before.
- A 60-second "Sort it!" game: the learner taps whether each transaction is a debit or a credit. There is a daily challenge (same transactions for everyone, one try a day) and an unlimited practice mode.
- A practice game with levels, three hearts and one-use helpers, built from the exam question bank.
- A real leaderboard on Supabase (Postgres): today and this week, shown only for learners who opt in, under a name they choose, with a small diamond for paying learners.
- Streaks with one forgiven missed day a week, and 12 badges grouped by colour. The hardest badge of each colour unlocks a flair to show next to the name.
- Reworked the dashboard and account pages after a review, with loading skeletons that match the real pages and a collapsible badges section.
- Tests for the scoring rules, the free and paid versions of the games, and phone width.

## What I learned

- The browser can't be trusted with a score. It only sends the taps and their times, and the server rebuilds the day's order from a seed, grades each tap and works out the score itself.
- A cheap way to catch obvious cheating is to refuse results that no person could produce: a round faster than a human can answer, or one that runs past the time limit.
- Database rules can enforce privacy, not just the page. Learners can read only their own scores and write none, and the function that builds the leaderboard can only be called by the server.
- A unique key on learner, game and day stops a second try at the database level, even if the page has a bug.
- Privacy by default: opting in is a choice, a default name uses a first name and an initial, and a learner who hasn't opted in sees their own row marked as hidden.
- Deploying code before its database change is safe if the page degrades well. With the migration missing, the game says "being set up" instead of crashing.
- Some things are better worked out when a page is read (a streak) and others saved once earned (a badge, so it stays even if the rules change later).
- A leaderboard place only means something with enough players, so places on days with fewer than three players don't count towards badges.
- Naming a known limit is part of the work: the day's order is in the page code, so a script could post a high score at human pace. A real fix would have the server send each transaction one at a time.
- Checking a migration on a throwaway local database (running it twice, a duplicate day, a name that is too long, ties) caught problems before it touched anything real.
- Loading skeletons should be rebuilt whenever a page changes, or the page jumps when the real content arrives.

## Tools used

- Next.js, React, Tailwind CSS, next-intl (English, French, German and Spanish text)
- Supabase (Postgres migrations, row-level rules, a server-only function)
- PGlite to check a migration locally
- Playwright and axe for end-to-end and accessibility tests
- Git and GitHub pull requests

## Link to the project repo

- Private work project, not linked.
