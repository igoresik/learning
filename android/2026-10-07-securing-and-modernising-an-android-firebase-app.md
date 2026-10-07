# Securing and modernising an Android + Firebase app (NotaLivre)

**Date:** 2026-10-07

## What I built or changed

- Audited my school project NotaLivre (Java/XML lesson-management app on Firebase) before changing anything, and wrote down its real architecture, broken flows and security risks.
- Took payment and push-notification keys out of the APK and moved anything secret to a small server.
- Rewrote the Firestore security rules around least privilege: roles fixed at sign-up, single-use teacher invite codes, data visible only to the people involved, and teacher-only payment amounts.
- Fixed broken user journeys: teacher registration, payments marked "paid" before anyone paid, students hidden from lists, account deletion that left data behind, and crashes that only happened in release builds.
- Built a payments server on Cloudflare Workers with Stripe Connect, so students can pay teachers in the app and teachers get paid to their own bank account.
- Redesigned most screens with insets, per-app language and accessibility fixes, and kept the sign-in screens in their original design.

## What I learned

- How to test Firebase security rules with the Firestore emulator, including atomic checks like "this invite code is deleted in the same write".
- Why a key inside an Android app is public, and how to move secrets behind a server that checks Firebase ID tokens.
- How Stripe Connect works: connected accounts, direct charges, signed webhooks, and making a webhook safe to receive twice.
- How to run a debug build against local emulators to test real user flows end to end without touching production data.
- How Android's edge-to-edge and window insets work, and why fixed margins break layouts on some phones.
- To audit and test before redesigning, and to keep what the owner already likes.

## Tools used

- Android Studio, Java, Gradle, Android lint, Pixel emulator, adb
- Firebase Auth and Firestore, Firebase Emulator Suite, @firebase/rules-unit-testing
- Cloudflare Workers, Stripe Connect, Node.js test runner
- Material Components, AppCompat per-app languages, Credential Manager

## Link to the project repo

- Private repository (NotaLivre)

## Screenshot (optional)

<!-- ![Description](screenshot.png) -->
