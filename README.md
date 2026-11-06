# /demo

**You built it. Now show people how it works.**

`/demo` is an agent skill that turns your project into a premium 30-90 second product demo. It runs your real app, records the features that matter, and cuts them into a launch-ready video with captions, music, optional voiceover and share copy. No screen recording, no editing.

- **Records the real app:** web apps, Electron/desktop apps and live sites, driven automatically with Playwright.
- **Desktop or mobile:** shows your web app in its real phone layout, in a phone frame with touch taps, or desktop and phone side by side.
- **Gets past login:** signs up local test accounts (one per role), fills the app with fictional data, and records every screen already signed in.
- **Shows what you pick:** `--features "attendance, admin dashboard"`, or it picks the strongest moments itself.
- **Wears your brand:** pulls your colors, fonts, logo and copy from the code.
- **Copies a style, not a brand:** drop a reference video in `reference/` and it matches the pacing and camera moves.
- **Picks the fit:** no tone or length given? It chooses what suits the product and tells you why.
- **Natural voice:** `--voice` uses Kokoro, running locally on your machine.

## Install

**Claude Code:**

```text
/plugin marketplace add Phemmy-Dev/demo-skill
/plugin install demo@demo
```

**Any other agent** (Cursor, Codex, Copilot, Gemini CLI, opencode and more), via the [`skills`](https://github.com/vercel-labs/skills) CLI:

```bash
npx skills add https://github.com/Phemmy-Dev/demo-skill --skill demo
```

Add `-g` to install it for every project.

**Manual:** copy the whole `skills/demo/` folder (including `references/`) into your agent's skills folder (for Claude Code, `~/.claude/skills/demo/`) and restart.

## Use

From your project folder:

```text
/demo
/demo --duration 45 --features "mark attendance, admin dashboard" --voice
/demo https://example.com --tone cinematic
/demo --style-ref reference/launch.mp4 --cutdown
```

| Option | Default |
|---|---|
| `--duration <s>` | auto (30-90) |
| `--features "<a>, <b>"` | auto: the 3 strongest moments |
| `--style-ref <video>` | `reference/` folder if present |
| `--tone polished\|default\|deadpan\|cinematic\|app-store` | auto |
| `--format landscape\|vertical\|square` | auto |
| `--device desktop\|mobile\|both` | auto: shows the app on desktop, a phone, or both |
| `--voice [voice]` | off (captions always on) |
| `--built-with <tool>` | off; adds an honest "how it was built" chapter |
| `--cutdown` | off; adds a 20s vertical version |

Type values without brackets: `--duration 60`, not `--duration <60>`; `--voice am_michael`, not `--voice[am_michael]`. The `< >` and `[ ]` above only mark where a value goes.

### Every option at once

```text
/demo --duration 60 --features "mark attendance, admin dashboard, weekly report" --style-ref reference/launch.mp4 --tone polished --format landscape --device both --voice am_michael --built-with "Claude Code" --cutdown
```

Or say it in plain English, which works just as well:

```text
/demo a 60 second polished demo of attendance, the admin dashboard and the weekly report, styled like reference/launch.mp4, desktop and phone, with an American male voice, show it was built with Claude Code, plus a vertical cut
```

Leave out anything you don't care about and `/demo` picks it for you.

### Choosing a voice

`--voice` on its own uses `af_heart`. Add a name to pick another. The first letter is the accent (`a` American, `b` British), the second is the voice (`f` female, `m` male):

| Voice | Sounds like |
|---|---|
| `af_heart` | American female, warm (default) |
| `af_bella` | American female, bright |
| `af_sarah` | American female, calm |
| `am_michael` | American male, steady |
| `am_adam` | American male, deeper |
| `bf_emma` | British female, clear |
| `bf_isabella` | British female, soft |
| `bm_george` | British male, classic narrator |
| `bm_lewis` | British male, relaxed |

Not sure? Ask "list the voices" during a run, or describe one ("a calm British woman") and it picks the closest. Voices are [Kokoro](https://huggingface.co/hexgrad/Kokoro-82M), running locally.

You get a `demo-output/` folder with `demo.mp4`, `demo.jpg`, `captions.srt`, `share-copy.txt`, `credits.txt`, `demo-plan.md` and `brand.md`.

## Match a reference video

Got a demo whose style you love? `/demo` can match it in your own brand.

1. **Put the video in a `reference/` folder** inside your project, then run `/demo`. It finds it automatically.
2. **Or point to it:** `/demo --style-ref "path/to/video.mp4"` (a link works too; it asks before downloading).

It studies the cuts, frames and pacing, writes what it learned to `demo-output/style-notes.md`, then builds your video with that style and your app's colors, fonts and logo.

| It copies | It never copies |
|---|---|
| Pacing and structure | Their logo or brand name |
| Camera zooms, 3D tilts, floating cards | Their colors as a brand |
| How text animates in | Their copy |
| Transitions and cursor clicks | Their music or footage |

**Tips**
- Pick a reference like your project: an app demo for an app demo, not a movie trailer for a dashboard.
- 30-60 seconds works best.
- Use one with sound if you want the music to match its feel; without audio, it matches the visuals only.
- Download YouTube videos yourself with a downloader you trust and pass the file.
- Read `style-notes.md` after a run. If something's off, tell it ("more zoom-ins like the reference").
- Don't commit someone else's video: add `reference/` to your project's `.gitignore`.

## Requirements

Node 22+, FFmpeg and Playwright. `--voice` uses Kokoro (about a 90MB model). The skill checks what's missing and asks before installing anything.

## Privacy

- It never uses real accounts or production data, and never shows passwords on screen.
- It never reads `.env` or key files, and reports hardcoded secrets without printing them.
- Every fictional substitution and test account it creates is listed so you can review and delete it.

## Credits

Built by Oluwafemi Akinruntan. Portions adapted from [latent-spaces/brag](https://github.com/latent-spaces/brag) (MIT). MIT licensed; see `LICENSE`.
