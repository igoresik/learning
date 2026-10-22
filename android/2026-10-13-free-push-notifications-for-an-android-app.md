<sub><a href="../README.md">Back to the notebook</a></sub>

# Free push notifications for an Android app (NotaLivre)

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-android-dark.svg"><img src="../assets/tag-android.svg" alt="Android" height="22"></picture></a>&nbsp; **Date:** 2026-10-13

## What I built or changed

- Tested my app NotaLivre by hand, screen by screen, and wrote down what broke. The bugs found that way were then fixed one small commit at a time.
- Brought push notifications back without a paid service. A small server on Cloudflare Workers sends them through Firebase Cloud Messaging: a new chat message, a lesson request, the answer to a request, a recorded payment, and a reminder within the hour before an accepted lesson.
- The app only tells the server which document changed. The server checks that the caller belongs to that document, then reads who should be notified from the database.
- The server sends data only (who, what, when). The phone writes the notification text itself, so it comes out in the language the app is set to.
- A scheduled job on the server runs every 15 minutes for the lesson reminders and marks each lesson once it has been reminded.
- Removed the OneSignal SDK, since Firebase now does the whole job, and updated the privacy texts to match.
- Added an unread-message count on the chat button, and a way out of the "student or teacher" choice when the wrong account was picked.

## What I learned

- Firebase Cloud Messaging is free to send with; what costs money is usually the place the sending code runs. A free Worker with the project's service account is enough.
- How the FCM HTTP v1 API is called: an OAuth token signed from the service account, then one POST per device token.
- The difference between a "notification" message and a "data" message. The system shows a notification message by itself when the app is in the background; a data message always reaches the app's own code, which is what makes translated text possible.
- Never let the client decide who gets a notification or what it says. The client names a document and the server works out the rest.
- Android 13 and later need the notification permission to be asked at runtime, or nothing is ever shown.
- Deleting the device's messaging token on sign-out stops the old account's notifications from reaching that phone.
- Dates stored as local text ("20/10/2026", "15:00") have to be compared in the right time zone on a server that runs in UTC, including summer time.
- A bit-mask bug: checking `inputType & PASSWORD` is true for "visible password" as well, because that value contains the same bit. Comparing against the variation mask fixes it.
- Removing one library can remove others that it pulled in, so build rules written for them can go too.

## Tools used

- Android Studio, Java, Gradle
- Firebase Cloud Messaging (HTTP v1), Firebase Auth, Firestore
- Cloudflare Workers (JavaScript, Cron Triggers), Wrangler
- Node.js test runner

## Link to the project repo

- Private repository (NotaLivre)
