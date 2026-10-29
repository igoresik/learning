<sub><a href="../README.md">Back to the notebook</a></sub>

# GPU effects, a code-made film and a fact-checked guide for a product site

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-web-dark.svg"><img src="../assets/tag-web.svg" alt="Web" height="22"></picture></a>&nbsp; **Date:** 2026-10-28

## What I built or changed

- Moved the website of my lesson-management app onto a WebGPU effects library (Shaders): the hand-written background shader became a library component, and a few new effects went in (a soft light behind the demo, paper grain, ink you can stir with the mouse, a magnifier over screenshots). The old WebGL version stays as the fallback.
- A 24-second product film in English and Portuguese, rendered frame by frame from code and the app's real screenshots, with a generated soundtrack. It plays in a dialog on the site.
- A step-by-step guide page for teachers and students in both languages, and a click-through tour of the same steps.
- Shipped as four stacked pull requests, plus a security update for the framework.

## What I learned

- A GPU effect library is heavy (about 730 KB compressed here). Loading it only on desktops with a mouse, only when the section comes near, and only once the browser is idle keeps phones and the first paint untouched.
- Third-party libraries can send data on their own. This one samples visits for telemetry by default, which would contradict the site's privacy page, so I turned it off and checked the network log for any outside requests.
- Every effect needs a plan for browsers that can't run it: a fallback, or a page that still looks finished without it. I tested without WebGPU, with touch, and with reduced motion.
- Library defaults are not brand colours. One gradient came out orange and pink until I gave it the site's own colour stops.
- A video can be made like a web page: a `render(t)` function that draws any moment from the time alone, a browser that screenshots each frame, and ffmpeg to join them with audio. Because every frame depends only on `t`, the whole film is reproducible and can be checked automatically (frame count, loudness, frozen moments).
- Writing a user guide is a retrieval job first. I took every step from the app's own interface strings and the site's published copy, then cut a sentence that promised more privacy than the privacy policy does.
- Stacked pull requests keep each feature reviewable; a fix in the bottom branch is merged up through the others.
- On a flaky connection, large pushes went through with HTTP/1.1 and a bigger post buffer.

## Tools used

- Next.js 16, React 19, TypeScript, Tailwind CSS 4
- Shaders (WebGPU components, a custom WGSL shader), WebGL fallback in a worker
- Canvas 2D, Playwright, ffmpeg, a code-generated score
- Vite + React for the click-through tour
- Headless Edge driven over the DevTools protocol for testing and screenshots, ESLint

## Link to the project repo

- Private repository (NotaLivre). Live site: https://nota-livre.vercel.app (all four pull requests merged and live).
