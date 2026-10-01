# Learned how to build sign-in, payments and security into a learning platform

**Date:** 2026-10-01

## What I built or changed

- Worked on a Next.js learning platform (a course, a bookkeeping simulator and accounts), with Claude Code helping me write and check the code.
- Sign-in and accounts: sign-up with a name, a live password strength bar with a show/hide eye, a password rule that is also enforced on the server, "Continue with Google" and "Continue with Microsoft", and changing a password only after confirming the current one.
- A test-mode Stripe Checkout where a paid checkout switches an account from free to paid through a signed webhook.
- Saved projects in the simulator for paid accounts, kept in Postgres with row-level security.
- A security review of the code, then fixes: an open redirect, rate limiting stored in the database, one fixed site address for email links, and a safer password change.
- A new dashboard and top navigation with smoother animations, and a simulator interface with an entrance and hover motion that no longer fights itself.
- Tried several dashboard designs as prototypes before choosing one.

## What I learned

- Only a verified webhook should grant access after a payment. The "success" page can be opened by anyone, so it only shows a message. Stripe signs every webhook, and the signature is checked against the raw body. A unique payment reference in the database makes granting idempotent.
- With Stripe test mode, the secret key, the dashboard and the Stripe command-line tool all have to be in the same account. When they weren't, payments happened where the listener wasn't watching, and nothing arrived. Checking which account a key belongs to, without printing the key, found it quickly.
- Social sign-in (OAuth) needs a client ID and secret from the provider, an exact redirect address that matches in both places, and a provider setting in the auth service. The secrets belong in the service's dashboard, never in the code or in chat.
- A "safe redirect" check can be fooled by hidden tab and newline characters, because URL parsers drop them and read `/<tab>/evil.com` as `//evil.com`. Reject control characters, and check that the parsed address stays on your own origin.
- Rate limiting can be done without a new service by counting attempts per visitor and time window in the database, and it should fail open so a limiter problem can't lock everyone out.
- Server-side checks matter more than browser-side ones: the same password rule has to be enforced in a server action, not only in the form.
- Row-level security plus column-level grants can keep answers and locked content out of the browser entirely. A function that runs with its owner's rights (`security definer`) should pin an empty `search_path`, and should only be callable by those who need it.
- A CSS entrance animation with `fill-mode: both` keeps owning the element's `transform` after it ends, so a hover lift fights it and looks jerky. Use `backwards`, or put the two effects on different elements. A GSAP tween can also leave an inline transform behind unless it is cleared.
- Habits that paid off: keep keys in an ignored env file, scan the git history for secrets, run `npm audit`, read the database's security advisor and understand each warning before acting on it.

## Languages

- TypeScript and JavaScript (React and JSX) for the site, the server actions and the tests
- SQL for the database: Postgres tables, row-level security policies, and functions in PL/pgSQL
- HTML and CSS (including keyframe animations and transitions) for the pages and motion
- Markdown and JSON for the course content and settings
- The site itself is available in 7 human languages, with all the text kept in translation files

## Tools used

- Next.js (App Router, server actions, route handlers), React, Tailwind CSS, next-intl for the translations
- Supabase (Auth with Google and Microsoft, Postgres, row-level security), PGlite to test migrations locally
- Stripe (Checkout, webhooks, command-line tool), Resend for email
- GSAP and CSS for motion, Playwright and axe for end-to-end and accessibility tests
- Claude Code, and Claude Design for dashboard prototypes
- Google Cloud and Microsoft Entra for the sign-in providers

## Link to the project repo

- Private work project, not linked.
