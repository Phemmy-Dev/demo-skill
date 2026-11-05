# /demo

**You built it. Now show people how it works.**

`/demo` is an agent skill that turns your project into a premium 30-90 second product demo. It runs your real app, records the features that matter, and cuts them into a launch-ready video with captions, music, optional voiceover and share copy. No screen recording, no editing.

- **Records the real app:** web apps, Electron/desktop apps and live sites, driven automatically with Playwright.
- **Gets past login:** signs up local test accounts (one per role), fills the app with fictional data, and records every screen already signed in.
- **Shows what you pick:** `--features "attendance, admin dashboard"`, or it picks the strongest moments itself.
- **Wears your brand:** pulls your colors, fonts, logo and copy from the code.
- **Copies a style, not a brand:** drop a reference video in `reference/` and it matches the pacing and camera moves.
- **Picks the fit:** no tone or length given? It chooses what suits the product and tells you why.
- **Natural voice:** `--voice` uses Kokoro, running locally on your machine.

## Install

**Claude Code:**

```text
/plugin marketplace add <your-github-username>/<repo-name>
/plugin install demo@demo
```

**Any other agent** (Cursor, Codex, Copilot, Gemini CLI, opencode and more), via the [`skills`](https://github.com/vercel-labs/skills) CLI:

```bash
npx skills add https://github.com/<your-github-username>/<repo-name> --skill demo
```

Add `-g` to install it for every project.

**Manual:** copy `skills/demo/` into your agent's skills folder (for Claude Code, `~/.claude/skills/demo/`) and restart.

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
| `--voice [voice]` | off (captions always on) |
| `--built-with <tool>` | off; adds an honest "how it was built" chapter |
| `--cutdown` | off; adds a 20s vertical version |

You get a `demo-output/` folder with `demo.mp4`, `demo.jpg`, `captions.srt`, `share-copy.txt`, `credits.txt`, `demo-plan.md` and `brand.md`.

## Requirements

Node 22+, FFmpeg and Playwright. `--voice` uses Kokoro (about a 90MB model). The skill checks what's missing and asks before installing anything.

## Privacy

- It never uses real accounts or production data, and never shows passwords on screen.
- It never reads `.env` or key files, and reports hardcoded secrets without printing them.
- Every fictional substitution and test account it creates is listed so you can review and delete it.

## Credits

Built by Oluwafemi Akinruntan. Portions adapted from [latent-spaces/brag](https://github.com/latent-spaces/brag) (MIT). MIT licensed; see `LICENSE`.
