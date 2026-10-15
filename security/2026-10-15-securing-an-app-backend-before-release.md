# Securing an app's backend before release (NotaLivre)

**Date:** 2026-10-15

## What I built or changed

- Reviewed the security of my app NotaLivre before its first Google Play release: the Firestore security rules, the Cloudflare Worker and the Android code. I used an automated code review, then checked each finding against the code myself.
- Closed the access holes it found in the database rules, so a user can only reach the people and data they are really connected to.
- Made the backend resist abuse by design instead of with rate limits: notifications only follow real events, invite codes are too long to guess, and teacher accounts need a confirmed email.
- Backed every change with automated tests on the Firebase emulators and an end-to-end run on the Android emulator, then deployed the rules and the Worker to production.

## What I learned

- A permission check is only as strong as the cheapest way to get the role it checks. When the way to get a role changes, every rule that trusts that role has to be checked again.
- Rules between two users should check how they are connected, not only who is writing.
- Limits per account do not stop attackers when accounts are free. Making each account cost more, or making the secret much harder to guess, works better.
- A rate limit only slows abuse down. Tying each action to a real, one-time event removes it.
- Only trust what the server sets, such as its own timestamps and the claims in the sign-in token, and make "check, then mark" a single safe step.
- Every exit path needs the same cleanup, and data nobody uses still needs closed rules.
- Review findings need checking against the real situation before fixing; some only matter for data that no longer exists.

## Tools used

- Firestore security rules, Firebase emulator, `@firebase/rules-unit-testing`
- Android (Java), Firebase Auth, Firebase Cloud Messaging, Gradle (unit tests, lint)
- Cloudflare Workers, Node.js test runner

## Link to the project repo

- Private repository (NotaLivre)

## Screenshot (optional)

<!-- ![Description](screenshot.png) -->
