<sub><a href="../README.md">Back to the notebook</a></sub>

# Online payments in an Android app with Stripe Connect (NotaLivre)

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-android-dark.svg"><img src="../assets/tag-android.svg" alt="Android" height="22"></picture></a>&nbsp; **Date:** 2026-10-15

## What I built or changed

- Turned on online payments in my app NotaLivre ahead of its first Google Play release. Each teacher connects their own Stripe account, and students pay the teacher directly from the app.
- Set up Stripe as a Connect platform in a sandbox, added a webhook destination for the teachers' accounts, and stored the keys as secrets on the Cloudflare Worker that talks to Stripe.
- Moved the Worker from the old Accounts v1 API to Accounts v2, because Stripe no longer lets new platforms create v1 connected accounts. Teachers keep the full Stripe Dashboard, pay Stripe's fees themselves, and Stripe covers losses.
- The Worker now decides a teacher can take payments from the card payments capability being `active`, instead of the old `charges_enabled` flag.
- Ran a full end-to-end test against the live backend with throwaway accounts: teacher onboarding, a €20 card payment from the student, the webhook writing the payment and history into Firestore. Then deleted the test accounts and data.

## What I learned

- With Stripe Connect "direct charges", the teacher is the seller: the payment lands in their account and the platform never holds the money. That keeps the app out of handling other people's funds.
- Webhooks are the real proof that a payment happened, not the "payment done" page. The Worker only marks something paid when Stripe's signed event arrives.
- A webhook for payments on connected accounts has to listen to "Connected accounts" events, not "Your account" ones, or it never fires.
- Stripe's v2 API is JSON with a pinned `Stripe-Version` header, while v1 is form-encoded; one small client can handle both by looking at the path.
- When an API call fails, reading the server logs (`wrangler tail`) shows the real error text that the app only sees as a short code.
- Onboarding links are single-use and expire: opening one twice, reloading, or a link preview burns it.
- Sandbox identity checks only pass with Stripe's special test values, so a realistic fake phone number fails verification.
- `wrangler secret put` takes the secret's name and asks for the value afterwards. Putting the key itself in the command makes the key the name.
- Google Play only requires its own billing for digital goods used inside the app. Paying a teacher for in-person lessons is a real-world service, so an outside payment provider is allowed.

## Tools used

- Stripe (Connect, Accounts v2, Checkout, webhooks, sandbox)
- Cloudflare Workers, Wrangler
- Firebase Auth, Firestore, Firebase emulator
- Node.js (test runner and a small end-to-end test script)

## Link to the project repo

- Private repository (NotaLivre)
