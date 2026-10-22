# Finding out why an Android app feels slow (NotaLivre)

**Date:** 2026-10-12

## What I built or changed

- Measured where the time went in my app NotaLivre before touching the code: sign-up, sign-in and the first screen all felt slow on the emulator.
- Found that most of the delay was not the app. The computer's Wi-Fi took about 1.75 seconds for one round trip to its own router, and every Firebase call needs several round trips.
- Made the app wait on the network less: a returning user now opens the home screen from the session saved on the device, and the server check happens in the background. Cold start to the home screen went from about 7.8 s to about 2.5 s on the same slow connection.
- Opened the Firestore connection while the sign-in form is on screen, so the first read after signing in does not pay for the connection setup.
- Made the home screen show the cached profile first and react when the server copy arrives; if the server no longer agrees with the device, the user is sent back to sign-in.
- Deployed updated Firestore security rules after the emulator tests passed, and reset the project's test data after taking a local backup.

## What I learned

- To measure first. Timestamps in logcat showed how long each step took, and a ping to the router showed the real cause in one line.
- How to read an Android app's timeline from logcat: process start, "Displayed" lines, Firebase Auth and Firestore messages.
- How to measure a cold start with `adb shell am start -W` and compare before and after a change.
- That a Firestore `get()` waits for the server and falls back to the cache only after 10 seconds, while a snapshot listener gives the cached copy at once and the server copy later.
- That "trust the device, verify in the background" needs a tested path for when the server disagrees, and that a slow network can hide that path during testing.
- How to test a sign-in flow end to end on local Firebase emulators by seeding data over REST and driving the app with adb.
- That a build error "Found item String/... more than one time" means the same string name is defined twice in one `strings.xml`.

## Tools used

- Android Studio, Java, Gradle, Pixel emulator
- adb (logcat, `am start -W`, `uiautomator dump`, `input`)
- ping, tracert, curl
- Firebase Auth and Firestore, Firebase Emulator Suite, Firebase CLI, @firebase/rules-unit-testing
- javap (to check what the Firestore SDK does when it starts)

## Link to the project repo

- Private repository (NotaLivre)
