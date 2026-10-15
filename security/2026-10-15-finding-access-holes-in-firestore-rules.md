# Finding and fixing access holes in an app's security rules (NotaLivre)

**Date:** 2026-10-15

## What I built or changed

- Before the first Google Play release of my app NotaLivre, I ran an automated code review over the Android code, the Cloudflare Worker and the Firestore security rules. I checked each finding against the code and fixed the ones that were real.
- Firestore rules: a teacher can now only link students who are members of the teacher's own school, and only record lessons for students they are linked to. Before, anyone could become a teacher by creating their own school and then attach themselves to any student, which opened chat, lesson requests and notifications to that student.
- Closed an old public `noticias` (news) collection that nothing used anymore but any teacher could write to.
- Teacher invite PINs went from 6 to 10 digits.
- Logging out from the school-choice screen now does the same cleanup as the profile logout (it deletes the phone's push token), through one shared function.
- The Worker now removes push tokens that Firebase Cloud Messaging reports as unregistered.
- Extended the emulator tests: 48 rules tests and 23 server tests pass.

## What I learned

- A rule is only as strong as the cheapest way to get the role it checks. "Is a teacher" was safe while becoming a teacher needed an invite. Once anyone could create a school, every rule that only checked the role was open to anyone.
- Rules between two users should check the relationship (same school, an existing link), not just the role of the person writing.
- A rate limit per account does not stop brute force when accounts are free, because guesses can be spread over many accounts. With 200 accounts, a 6-digit PIN had about a 1 in 170 chance of being guessed during its 5 minutes; with 10 digits it is about 1 in 1.7 million.
- Every logout path needs the same cleanup. A second logout button that only signed out left the push token in place, so the next person on that phone could get the previous user's notifications.
- Data nobody uses still needs closed rules: deny by default and delete rules for old collections.
- Review findings need checking against the real situation. Two findings only mattered for old data that had already been wiped, so they needed no fix.

## Tools used

- Firestore security rules, Firebase emulator, `@firebase/rules-unit-testing`
- Android (Java), Firebase Cloud Messaging, Gradle (unit tests, lint)
- Cloudflare Workers, Node.js test runner

## Link to the project repo

- Private repository (NotaLivre)

## Screenshot (optional)

<!-- ![Description](screenshot.png) -->
