<sub><a href="../README.md">Back to the notebook</a></sub>

# Triaging security risks against how each project is built

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-security-dark.svg"><img src="../assets/tag-security.svg" alt="Security" height="22"></picture></a>&nbsp; **Date:** 2026-10-16

## What I built or changed

- Took a generic list of vulnerability types, the kind that code scanners and AI-code checklists give, and sorted each one, per project, into "fix now", "low priority or overkill" and "does not apply", based on how each app is really built and hosted.
- Reviewed an Android + Firebase app before its release and a Next.js + Supabase learning platform, then fixed what mattered: access rules, server-side checks, input handling and dependency updates, plus several smaller hardening changes.
- Added automated tests for the fixes, including abuse cases that try what an attacker would try.

## What I learned

- The same checklist means different things per stack. Some items don't apply at all, while access rules and free-tier limits matter a lot.
- When an app talks to its database directly, the database rules are the real access control, and the app itself can't be trusted.
- Every place that checks permissions has to check the same things, because the weakest check decides.
- Input that ends up in paths, queries or patterns needs strict limits before it is used.
- Reproducing a problem with a failing test first, then fixing it, leaves a test that keeps it fixed.
- Some protections cost more than they give for a small app, and saying so is part of a review.

## Tools used

- Firestore security rules, Firebase emulators, `@firebase/rules-unit-testing`
- Cloudflare Workers, Web Crypto, Node.js test runner
- Android (Java), Gradle
- Next.js, Supabase (Postgres grants and row-level security), Playwright
- `npm audit`, a scan of the git history for secrets

## Link to the project repo

- Private repositories (NotaLivre and the learning platform)
