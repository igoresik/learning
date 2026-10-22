<sub><a href="../README.md">Back to the notebook</a></sub>

# Protecting sign-up and sign-in from bots with a captcha

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-security-dark.svg"><img src="../assets/tag-security.svg" alt="Security" height="22"></picture></a>&nbsp; **Date:** 2026-10-19

## What I built or changed

- Added Cloudflare Turnstile to the sign-up, sign-in and forgot-password forms of an online course platform I work on (Next.js with Supabase Auth). Each form sends a Turnstile token with its request.
- Switched on captcha protection in Supabase, so the auth server itself refuses those requests without a valid token. Scripts that call the auth API directly are refused too, not only the forms.
- The change-password form checks the current password on the server instead, which skips the captcha, so it got its own limit of 10 tries an hour per learner.
- Changed the automated tests: they sign in on the server instead of through the form, and the form tests use Cloudflare's always-pass test key. Added a test that checks the token is actually sent.
- Wrote the setup steps in the project's README, in the order they have to happen.

## What I learned

- A captcha only protects anything if the server checks it. Checking only in the browser stops nobody who calls the API directly.
- The order of switching it on matters: first the widget and its site key on the live site, then enforcement on the server. The other way round, nobody can sign in until the site catches up.
- A Turnstile token works once. It has to be taken right before each request, and the widget reset afterwards.
- In "managed" mode the widget stays hidden for most people and only asks for a tick when Cloudflare is unsure about the visitor.
- Test browsers can't pass a real captcha, so tests need another way in. That way must only exist on the server side, never in the public forms.
- Any path that skips the captcha needs its own rate limit, or it becomes the easy way around it.
- I checked that the auth server refuses requests without a token and that a headless browser gets the "Verify you are human" box. A real sign-up through the live server is still to be tested.

## Tools used

- Next.js, TypeScript
- Supabase Auth (captcha protection)
- Cloudflare Turnstile
- Playwright
- Vercel

## Link to the project repo

- Private repository (client project)
