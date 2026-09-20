# Recorders

Write scripts to the scratch directory. Replace uppercase placeholders. Keep the whole recording under 60 seconds.

## Playwright (web)

Records `.webm` (VP8). Requires the app running; start it per repository docs and wait for the URL to answer.

```js
// demo.mjs — run: node demo.mjs   (or: npx playwright install chromium first)
import { chromium } from "playwright";
const browser = await chromium.launch();
const context = await browser.newContext({
  viewport: { width: 1280, height: 720 },
  recordVideo: { dir: "OUT_DIR", size: { width: 1280, height: 720 } },
});
const page = await context.newPage();
await page.goto("APP_URL");
await page.waitForTimeout(1500);
// one block per scenario step; pause after each visible state
await page.click("SELECTOR");
await page.waitForTimeout(1500);
const video = page.video();
await context.close();          // flushes the file
console.log(await video.path());
await browser.close();
```

- Use `page.locator(...)` with role/text selectors from the PR's UI; no test IDs unless they exist.
- Pauses of 1–2 s between steps keep it followable. Slow-motion typing: `launch({ slowMo: 50 })`.

## VHS (terminal)

Requires `vhs`, `ttyd`, and `ffmpeg` on PATH. `vhs --version` to check; missing means report and stop.

```tape
# demo.tape — run: vhs demo.tape
Output OUT_DIR/demo.mp4
Set Width 1280
Set Height 720
Set FontSize 18
Set TypingSpeed 50ms
Type "COMMAND_STEP_1"
Enter
Sleep 2s
Type "COMMAND_STEP_2"
Enter
Sleep 3s
```

- Run from the repository root so relative commands work. `Set Shell` if the repo expects a specific shell.
- `Hide`/`Show` around setup commands that should not appear.

## Size and checks

- Duration and bytes: `ffprobe -v error -show_entries format=duration,size -of csv=p=0 FILE`
- Review frames (one timestamp per step), then view the strip image:

```sh
for t in 1.5 4 7.5 10; do ffmpeg -v error -y -ss $t -i FILE -frames:v 1 -vf scale=320:-2 f_$t.png; done
ffmpeg -v error -y -i f_1.5.png -i f_4.png -i f_7.5.png -i f_10.png -filter_complex "[0][1][2][3]hstack=4" strip.png
```
- Re-encode when over cap (also converts WebM to MP4):

```sh
ffmpeg -i IN -vf scale=1280:-2 -c:v libx264 -crf 30 -preset veryfast -movflags +faststart -an OUT.mp4
```

- Still over: shorten the scenario and re-record. Do not raise CRF beyond 34; legibility matters more than fitting.
