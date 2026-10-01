# Figured out sign-in, payments and security for a learning platform

**Date:** 2026-10-01

## What I built or changed

- Worked on a Next.js learning platform: a course, a bookkeeping simulator and accounts.
- Sign-in and accounts: sign-up with a name, a password strength bar with a show/hide eye, a password rule that the server enforces too, "Continue with Google" and "Continue with Microsoft", and a password change that asks for the current one first.
- A test-mode Stripe Checkout. Paying switches an account from free to paid through a signed webhook.
- Saved projects in the simulator for paid accounts, stored in Postgres with row-level security.
- A security review of the code, then fixes: an open redirect, rate limiting stored in the database, one fixed site address for email links, and a safer password change.
- A new dashboard and top nav with smoother animations, and a simulator whose hover and entrance motion finally stopped fighting each other.
- Tried a bunch of dashboard designs as prototypes before picking one.

## What I learned

- Only a verified webhook should grant access after a payment. The "success" page can be opened by anyone, so all it does is show a message. Stripe signs every webhook, and you check the signature against the raw body. A unique payment reference in the database means granting access can't happen twice.
- In Stripe test mode, the secret key, the dashboard and the command-line tool all need to be in the same account. Mine weren't, so payments happened somewhere the listener wasn't watching and nothing showed up. Asking Stripe which account a key belongs to (without printing the key) found it fast.
- Social sign-in (OAuth) needs a client ID and secret from the provider, a redirect address that matches exactly in both places, and the provider switched on in the auth service. The secrets go in the service's dashboard, not in the code or in chat.
- A "safe redirect" check can be fooled by hidden tab and newline characters. URL parsers drop them, so `/<tab>/evil.com` gets read as `//evil.com`. Reject control characters and check the parsed address stays on your own origin.
- You can rate limit without a new service by counting attempts per visitor per time window in the database. It should fail open, so a limiter hiccup doesn't lock everyone out.
- Server-side checks beat browser-side ones. The password rule has to live in a server action, not just in the form.
- Row-level security plus column-level grants can keep answers and locked content out of the browser completely. A function that runs with its owner's rights (`security definer`) should pin an empty `search_path` and only be callable by whoever needs it.
- A CSS entrance animation with `fill-mode: both` keeps owning the element's `transform` after it ends, so a hover lift fights it and looks jerky. Use `backwards`, or put the two effects on different elements. A GSAP tween can also leave an inline transform behind unless you clear it.
- Habits that paid off: keys in an ignored env file, scanning the git history for secrets, running `npm audit`, and actually reading the database's security advisor before reacting to its warnings.

## Languages

- TypeScript and JavaScript (React and JSX) for the site, the server actions and the tests
- SQL for the database: Postgres tables, row-level security policies, and functions in PL/pgSQL
- HTML and CSS (keyframe animations and transitions included) for the pages and motion
- Markdown and JSON for the course content and settings
- The site comes in 7 human languages, with all the text kept in translation files

## Tools used

- Next.js, React, Tailwind CSS, next-intl for the translations
- Supabase, PGlite to test migrations locally
- Stripe, Resend for email
- GSAP and CSS for motion, Playwright and axe for end-to-end and accessibility tests
- Google Cloud and Microsoft Entra for the sign-in providers

## Link to the project repo

- Private work project, not linked.
