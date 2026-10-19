# Adding a dark theme to an existing Android app (NotaLivre)

**Date:** 2026-10-19

## What I built or changed

- Gave my app NotaLivre a dark theme that follows the phone's setting, without changing how the light version looks.
- First moved every hardcoded colour in the layouts, drawables and Java code into named colour resources, keeping the exact same light values. Each of those commits changed nothing on screen, so they were safe to push one by one.
- Then added a `values-night` folder with a dark value for each named colour and switched the theme from a light-only parent to `DayNight`.
- Went through the teacher and student screens on the emulator in both modes and fixed what still looked wrong: cards that turned grey, a white calendar, a black back arrow and a "+" that almost disappeared.

## What I learned

- Android picks resources by configuration: the same colour name can have one value in `values` and another in `values-night`, and the app switches between them by itself.
- A dark theme is mostly a naming job. Colours named by their role (page background, card, muted text) can flip; raw hex values in layouts can't.
- Doing the renaming first, with identical light values, splits a risky change into small steps that can each be checked and shipped.
- Some colours must not flip. Text on a brand-coloured button has to stay dark or light in both modes, or it vanishes against a background that didn't change.
- Components have their own defaults: a plain `CardView` is white in light mode but turns grey in dark mode unless its background is set explicitly.
- An app can override a library's colours by defining resources with the same names. Lint then asks for a light default next to each night value, so nothing can crash in a configuration that has none.
- Status bar and navigation bar icons need to switch too, which a boolean resource with a night value handles.

## Tools used

- Android Studio, Java, Gradle, Android Lint
- Material Components (Material 3 `DayNight` theme)
- Android emulator and `adb` (switching night mode, screenshots)
- Firebase Auth and Firestore emulators for local test accounts
- Python (a small script to swap the hardcoded colours)

## Link to the project repo

- Private repository (NotaLivre)

## Screenshot (optional)

<!-- ![Description](screenshot.png) -->
