# Making an Android app work with a screen reader and in two languages (NotaLivre)

**Date:** 2026-10-19

## What I built or changed

- Used Android Lint's report as a to-do list for my app NotaLivre and cleared two whole groups of warnings: text that never got translated and images a screen reader can't describe.
- Moved the remaining Portuguese-only hints into the string files, so they switch to English with the rest of the app. Kept the Portuguese wording users already saw.
- Replaced two places where Java glued text together by hand with strings that have a placeholder for the value.
- Gave every button that only shows an icon (back, change language, send, show password, Google sign-in, export PDF) a translated label for TalkBack.
- Marked icons that only decorate a row as unimportant, so TalkBack reads the row's text once instead of announcing "unlabelled image".
- Checked the screens I hadn't opened in dark mode yet and fixed the three that were still wrong.

## What I learned

- An icon-only button with no `contentDescription` is read by TalkBack as just "button". Anyone using a screen reader can't tell what it does.
- Decorative images need the opposite: `importantForAccessibility="no"`, so they are skipped instead of labelled.
- Text glued together in code (`label + ": " + name`) can't be translated properly, because word order changes between languages. A string with `%1$s` lets each language put the value where it belongs.
- A hint typed straight into a layout stays in one language forever, even in an app that has an English and a Portuguese version.
- Lint is a cheap audit: it found these problems across the whole app in seconds, and the warning count shows the progress.
- A disabled Material button greys its text out with the theme's text colour. On a fill that stays the same in dark mode, that grey disappears, so the button needs its own colour list.

## Tools used

- Android Studio, Java, Android Lint
- TalkBack concepts (content descriptions, `importantForAccessibility`)
- Android string resources (`values` and `values-en`) with placeholders
- Android emulator and `adb` for the dark-mode checks
- Python (a small script to add the labels screen by screen)

## Link to the project repo

- Private repository (NotaLivre)
