---
name: demo
description: Turn a project into a premium 30-90 second product demo video that shows the real product in use, in the product's own brand, optionally matching the motion style of a reference video. Records the actual running app (web apps, Electron/desktop apps, live sites, desktop or mobile view), gets past login screens with local test accounts, picks the tone and format that fit when none is given, focuses on the features the user names, adds captions, music, optional natural voiceover (Kokoro) and share copy, and can honestly show how it was built (with Codex, Claude Code or any tool). Use when someone says "/demo", "make a demo video", "make a product demo", "make a 30 second / 1 minute launch video", "demo these features", "make it look like this video", "demo this for my application/portfolio", "show people how my app works", or wants a walkthrough video of something they built.
---

# /demo

You built it. Now show people how it works. /demo makes a 30-90 second product demo a stranger can follow: what it is, the real product doing its job, how it was built, and where to get it. No screen recording, no editing: it runs your app, records the parts that matter, and cuts them into a launch-ready video. It must look like a studio made it, not like AI made it.

Usage: `/demo [project path | URL] [options]`. Options can be flags or plain language.

| Option | Default |
|---|---|
| `--duration <s>` | auto: you pick the length the content needs (30-90) |
| `--features "<a>, <b>, ..."` | auto: you pick the 3 strongest beats |
| `--style-ref <video path or URL>` | none; `reference/` or `style-ref/` in the project is used if present |
| `--tone polished\|default\|deadpan\|cinematic\|app-store` | auto: you pick what fits the product (see Tone and format) |
| `--format landscape\|vertical\|square` | auto: landscape 1920x1080 unless the product or destination says otherwise; 30fps |
| `--device desktop\|mobile\|both` | auto (see Tone and format); `mobile` shows the app on a phone, `both` shows desktop and phone together |
| `--built-with <tool>` | none; turns on the "How it was built" chapter |
| `--voice [voice name]` | off; captions are always on |
| `--cutdown` | off; also renders a 20s vertical cut for socials |

Plain language maps to these: "a 45 second demo showing the lyrics and the timer, make it look like reference/launch.mp4, with a British voice" sets duration, features, style-ref and voice.

Run from the project directory. Write deliverables to `demo-output/` (or `demo-output-YYYY-MM-DD-HHmmss/` if it exists). Keep intermediates in `work/` inside it. Add `demo-output*/` to the project's `.gitignore` if it isn't covered, and tell the user.

## Reference files: required reading

Some instructions live in `references/` next to this file, so each run loads only what it needs. These are not optional extras. When a row's condition is true, its file is part of the instructions for this run.

| Condition (check at the step shown) | Read | Step | Proof it was followed |
|---|---|---|---|
| A style reference exists (`--style-ref`, or a video in `reference/` or `style-ref/`) | `references/style-ref.md` | 2 | `style-notes.md` with every section filled |
| Anything other than a plain desktop web app: `--device mobile`/`both`, an Electron or desktop app, a live URL, or something that can't run | `references/capture.md` | 3 | Plan's "Capture method" section names the method from that file |
| The app has a login, sign-up, or role-based screens (look for auth routes, login pages, session or JWT code, auth libraries) | `references/login.md` | 3 | `work/demo-accounts.md` and a "Test accounts and data" plan section |
| `--voice`, or the user asks for narration | `references/voice.md` | 4 | Plan's "Voiceover" section with voice name and line timings |
| `--built-with`, or the user wants to show how it was made | `references/built-with.md` | 1 | Evidence summary shown to the user before any claim |

Rules:
1. **Check every condition, every run.** Decide each row as yes or no from what you found, not from a guess.
2. **Read the whole file with the Read tool before that step.** Don't act on the one-line summary above or on memory of an earlier run; the file has steps the summary leaves out. If your context was compacted or summarized since you read it, read it again.
3. **Log it.** `demo-plan.md` starts with a "Files read" list: every row, read or skipped, with the reason (e.g. "login.md: read, app has /login and roles admin/member"; "voice.md: skipped, no --voice").
4. **Gate before building.** Before step 5, confirm the proof exists for every row you marked yes. If any is missing, go back and do that step. Never build past a missing proof.

## 0. Preflight

Check, don't assume: Node (22+), FFmpeg on PATH, Playwright (in the project, globally, or in `work/`). Electron apps also need the project's own `electron` dependency. `--voice` needs Kokoro (see `references/voice.md`). If anything essential is missing, list it with its download size and ask before installing. Prefer the platform package manager (winget, brew, apt) or npm over `curl | sh`. Never install into the user's project dependencies; install into `work/` (`npm i playwright` there, then `npx playwright install chromium`) or globally.

## 1. Inspect

### What it is

Read the README, main entry, routes, key components and styles. Answer before planning:
1. What is it, in one sentence a stranger understands?
2. Who is it for, and what does it do for them?
3. What are its beats in use (entry, key action, result)?
4. What sets it apart? What's the single most impressive true claim?
5. What tone fits? (See Tone and format.)

Then decide every row of the reference table.

### Tone and format

If the user names a tone or format, use it. Otherwise choose from who the product is for and how it should feel, not from what looks flashiest:

| Product | Tone | Feel and pacing |
|---|---|---|
| Community, church, school, health, nonprofit, family | `polished`, warm | Serious, elegant, friendly copy, no hype; long holds, soft fades |
| Fintech, B2B, SaaS dashboards, admin tools | `polished` or `app-store` | Credible; clean feature cards, smooth slides |
| Consumer app with several distinct features | `app-store` | Feature-by-feature walkthrough, smooth slides |
| Utilities, dev tools, design-forward or Apple-inspired products | `polished`, keynote feel | Big type, slow reveals |
| Games, creative tools, big launches | `cinematic` | Trailer-scale, big type, dramatic wipes |
| Playful, absurd or joke projects | `default` or `deadpan` | Punchy and playful, or calm and dry with big empty space and slow fades |

Let the product's own copy break ties: a site that talks warmly gets warm; one that talks in metrics gets crisp. Freeform direction refines or overrides a preset. A style reference refines pacing and motion but doesn't override a tone that fits the audience (a church app doesn't become a hype reel because the reference is one).

Format: landscape for web and desktop apps, portfolios and applications. Vertical when the user mentions TikTok, Reels, Shorts or Stories, or the product is a phone app meant for social. Square only when asked. When unsure, landscape, and offer `--cutdown`.

Device (the screen the app is shown on, separate from the video's shape): `desktop` for desktop apps and dashboards built for big screens. `mobile` when the product is mobile-first: its users are mostly on phones (members checking in, customers on the go), it's a PWA (`manifest.json`, "Add to Home Screen"), or its CSS is written mobile-first with a phone-sized layout as the main design. `both` when it's responsive and the point is "works everywhere". A vertical video with a web app defaults to `mobile`. Mobile on a landscape video: the phone sits to one side with the headline beside it, or desktop and phone side by side for `both`.

State the choice and the reason in one line before building, e.g. "Polished and warm, landscape, mobile: it's a church app members use on their phones, so trust beats hype." The user can switch it.

### Features to show

- **`--features` given:** each named feature gets its own beat, in the order given unless another order tells a clearer story (say why in the plan). Map each name to the real code (component, route, view) and to the exact interaction that shows it off: what to click, hover, type, and what result the viewer sees. If a named feature can't be found or can't be shown, say so and ask; never fake it. If the duration is too short for all of them, say which ones get a quick montage shot instead of a full beat, or ask to lengthen.
- **No `--features`:** pick the 3 strongest beats yourself, in order of how much they make a stranger say "oh, nice". List them in the plan, with what you left out and why.

### Brand system

The video wears the product's brand, never a generic look. Extract its design system from the source and write it to `brand.md`:
- **Colors:** exact values from CSS variables, Tailwind config, theme files or the main stylesheet. Name the roles: background, surface, text, muted text, accent, accent-2. Note light and dark modes if both exist.
- **Type:** the fonts it really loads (from `@font-face`, Google Fonts links, `font-family` stacks), with weights used. Use the same files in the video; if a font is a system font (Segoe UI, SF Pro), use it only if it's available on the machine, otherwise pick the closest open font and say so.
- **Shape and depth:** corner radii, shadows, borders, blur, spacing scale.
- **Assets:** logo, app icon, product images, and any signature motion (an easing, a hover state, a transition) already in the code.
- **Voice of the copy:** the headline, tagline and the way the product talks.

If the product has no clear system, derive a restrained one from its UI (one accent, neutrals, one type family) and say so in the plan. Never borrow another brand's identity to fill the gap.

### Safety

Never read build output, lock files, `.git/` internals (except the log, for `--built-with`), `.env*`, key or credential files, anything under `secrets/` or `credentials/`, or files `.gitignore` excludes for those reasons.

While reading, note any hardcoded keys or tokens you happen to see, and their files. Never print their values. Tell the user in the plan which files contain them and keep those files out of every frame.

Nothing private reaches the video or copy: no keys, internal URLs, real client or customer names, emails, phone numbers, bank details or real financial data. Use plausible fictional stand-ins and list every substitution in the plan.

## 2. Study the style reference (if any)

If the reference-table row is yes, follow `references/style-ref.md` now.

## 3. Capture the real product

Real footage beats rebuilt UI. Use the first that applies:

1. **User recordings.** If `demo-assets/` exists in the project, use its recordings and screenshots first.
2. **Web app, recorded live.** Start the project's own dev server (from `package.json` scripts) on a free port, using demo, seed or offline mode if the README describes one, so no real account or data is touched. Drive it with Playwright (`recordVideo` at the target resolution, or frame captures) through each beat. Record each beat as its own clip in `work/capture/`, 1-2s longer on both ends than needed.
3. **Mobile view, Electron, other desktop apps, live public sites, or things that can't run:** follow `references/capture.md`.

If the app has a login, follow `references/login.md` before recording any beat.

For every capture: enter only fictional data. Never sign in with real credentials (only test accounts from `references/login.md`), submit to production services, send messages or make payments. Hide real notifications, other windows, file names and personal data. Stop every server and app when done. Check every captured frame for private data before using it.

Also capture clean stills of each key control (the button that gets clicked, the result card) at 2x resolution, for zoom-ins and floating-card shots.

## 4. Plan

### Duration

- **`--duration` given:** use it exactly (allowed 30-90; outside that, say so and use the nearest limit).
- **Not given:** pick the length the content needs, then round to a clean number. Budget roughly: hook 3-5s, reveal 4-6s, 7-12s per full feature beat, 3-5s per quick montage shot, 6-12s for a build chapter, close 5-8s. Clamp to 30-90. A product with 2 beats is a 30-35s video; 3 beats about 45s; 4-5 beats 60-75s. Shorter and tighter beats longer and padded. State the length you chose and why in one line.

### Plan file

Write `demo-plan.md`: the "Files read" list, the 5 answers, the features and how each is shown, tone, format and device, a link to `brand.md` and `style-notes.md`, the capture method, every data substitution, the sections the reference files ask for, and a storyboard with durations summing to the target, every cut on a beat of the chosen tempo. For each scene say: what's on screen (which capture or still), the camera move, the text and how it animates in, the transition out, and the sound.

Default shape, scaled to the duration (cut the build chapter if `--built-with` isn't set, and give its time to product beats). If a style reference exists, its structure wins over this table.

| Chapter | Share | Job |
|---|---|---|
| Hook | ~8% | The problem or sharpest true claim. Decides if anyone stays. |
| Reveal | ~12% | Name, one-line what it is, who it's for. |
| In use | ~45-60% | One beat per feature: real footage, cursor, typing, clicks, zoom to the result. |
| How it was built | ~15-20% | User-confirmed facts plus git numbers, shown as prompt-to-result. |
| Close | ~12% | Name, where to get it (URL or repo), one line on what's next. |

Also write `captions.srt` timed to the storyboard; captions carry the story with the sound off.

Show the user the plan (length, features, angle, look) in a few lines and continue unless they object; for videos over 60s, wait for an OK.

## Premium bar

The test: would a viewer believe a product-design studio made this? Everything below serves that.

**Do:**
- **Real UI, framed with care.** Show captured footage as floating cards with the product's radius and a soft, wide shadow; a subtle 3D tilt (a few degrees) that settles flat; or cropped tight on one control. Never a raw, full-frame screen recording sitting flat for seconds.
- **Direct the eye.** Every feature beat has one focal point. Push the camera in on the control being used, then on the result. Dim or blur what doesn't matter. The viewer should never search the frame.
- **Fast moves, firm landings.** At 30fps: camera travel 8-14 frames, then a hard ease-out settle of 10-16 frames (`Easing.out(cubic)` into a spring around damping 18, stiffness 140); element pop-ins on a snappier spring (damping 12, stiffness 180); cascades staggered 3-6 frames apart. Directional motion blur on fast camera moves; sharp means settled. When scenes change, the outgoing and incoming ride one camera move together (push through, crane up, slide across), not two separate fades. Never linear, never floaty.
- **Never let it sit.** Something is always moving: the camera drifting, a cursor travelling, a list filling in. No stretch longer than 1.5s without motion, except one deliberate hold on the key result. Logo reveals take under 2s; the end card holds 2-3s, not longer.
- **Quick typing.** Typed text runs at about 2 frames per character (15 characters a second), never slower than 3; a sentence types in about 2 seconds, then holds just long enough to read.
- **Typography as a design element.** The product's typeface. One big line at a time, large (6-9% of frame height for headlines), tight tracking, sentence case unless the brand shouts. At most two sizes per scene. Animate it in word by word, as a mask reveal, or a typed line with a caret; let it settle and hold.
- **Rhythm.** Alternate tension and release: quick cuts through steps, then a held reveal on the result. Alternate light and dark scenes if the brand has both. Every cut and every text entrance lands on the beat.
- **Cursor with intent.** A clean, slightly enlarged pointer moving on a smooth curve, a short press-and-release scale on clicks, a soft ripple or glow on the clicked control. (Phones: see `references/capture.md`.)
- **Restraint with color.** Neutrals carry the frame; the accent appears only where the eye should go (the clicked button, the key word in a headline, the success state).
- **Generous space.** Safe margins of at least 6% of the frame; align to a grid; nothing crowds an edge.

**Never (these read as AI slop):**
- Generic purple-to-blue gradients, glowing orbs, particle fields, lens flares, glitch effects, or "tech" grids that aren't from the product's brand.
- Emoji, clip-art icons, stock illustrations, or AI-generated imagery standing in for the product.
- Dot-pills: a capsule with a small glowing or colored dot before the text ("● Now live", "● Your whole day on one calm screen"), especially on a soft glow halo. Put that line in plain type or a simple pill with no dot, unless the exact badge is in the product's own UI.
- Text slapped over busy footage with no scrim or space; drop shadows on text; more than one line animating in at once.
- Everything centered, everything the same size, every transition the same crossfade.
- Bouncy or overshooting easings on everything; motion that starts and stops at the same instant across the frame.
- Filler words on screen ("Revolutionary", "Seamless", "Next-level", "Unlock", "Streamline your workflow").
- Long flat holds of a static screenshot; zooms into blurry, upscaled pixels (use the 2x stills).

## Creative laws

- **Clear to a stranger** after one viewing: what it is, who it's for, how to get it.
- **Show the real thing.** Real footage and real UI over illustration. No abstract filler.
- **Specific.** Its own copy and claims.
- **True.** No invented numbers, testimonials or claims, about the product or how it was built.
- **Readable.** Lines meant to be read stay settled about 0.3s per word (0.8s minimum for a label). Captions never cover the action.
- **Alive.** Things appear one by one; cursor moves, typing and clicks beat static slides. Vary pace across chapters so a long video doesn't drag.
- **Every frame postable.**

## Sound

Music and effects as one piece on one beat grid from frame 0, in the same key, effects mixed softly under the music. Use music generated on the machine, or a track the user supplies or that is clearly licensed for reuse (record the license in `credits.txt`). Never use a track without a known license. Voiceover: `references/voice.md`.

## 5. Build, review, render

First, run the gate: every reference row marked yes has its proof. Then:

Compose in a browser page where every frame is a pure function of time; wait for fonts, images and video frames before capturing each frame. Load the product's real fonts and assets locally. Render frames with Playwright and encode with FFmpeg (H.264, yuv420p, AAC audio, `-movflags +faststart`).

### Quality review (required before the full render)

Export stills from the middle of every scene and from mid-transition. Look at each one, then write `work/review.md` with a row per scene, scored 1-5 on:

| Check | 5 means |
|---|---|
| Real product | Real captured UI is the hero, not a rebuilt or abstract stand-in |
| One focal point | The eye lands on one thing instantly; the camera pushes in on it |
| Readable | Every line meant to be read is large, high-contrast, settled long enough |
| On brand | Colors, fonts, radii and copy match `brand.md`; nothing generic |
| Alive and smooth | Motion eased and settled; no dead frames; nothing on the "never" list |
| Matches the reference | (only with a style reference) Pacing, camera and type match the contact sheets |

Then measure the pace from a draft render instead of judging it by eye. From inside `work/` (relative file names: Windows drive colons break FFmpeg's filter syntax):
```bash
ffmpeg -i draft.mp4 -vf "scale=320:-1,tblend=all_mode=difference,signalstats,metadata=print:key=lavfi.signalstats.YAVG:file=motion.txt" -f null -
```
Each `YAVG` value is how much that frame changed. Report the share of frames above 1.5 (moving) and the longest run below it (sitting still). Targets: at least 40% moving, no still run over 1.5s except the planned hold and the end card. With a style reference, measure it the same way: be within 20% of its moving share, or explain why not. Too slow? Shorten holds, speed up typing and travel, and add camera drift before adding anything new.

Then three whole-video checks, written in the same file:
- **Hook:** would the first 3 seconds stop a scroll? Say why in one line.
- **Stranger test:** from the stills alone, write what it is, who it's for and how to get it. If you can't, the video doesn't say it yet.
- **Plan match:** every planned feature is shown, durations match, captions line up.

Common fixes: text overflow or collisions, low contrast, muddy crossfades between busy layouts (stagger them: old content out, then new in, or dip through the background), dead frames, clicks that miss the real button, text too small after a push-in.

**Any score below 4 gets fixed, re-exported and re-scored.** Score honestly: a 3 called a 4 only ships a weaker video. Do at least one revision pass even if everything scores 4+, aiming the weakest scene at 5; log what you changed. After three passes, stop and render, and tell the user plainly which scenes are still below 4 and why.

### After the render

Check with `ffprobe` that duration, resolution and audio match the plan. Make a contact sheet of the final file and look at it: no black frames at cuts, nothing cut off, frame 0 is the poster. Never report a check you didn't run.

`--cutdown`: also render `demo-vertical.mp4`, 15-20s 1080x1920, hook + best product beat + close, re-framed for vertical (not a letterboxed landscape), and review its stills the same way.

## 6. Deliver

In the output folder:
- `demo.mp4`, with the strongest settled frame baked in as frame 0 (replace, don't add) and saved as `demo.jpg`
- `captions.srt`
- `share-copy.txt`: 1-3 specific sentences, postable as-is, no "excited to share"; if there's a build chapter, one honest line on the tool's role
- `credits.txt`: music, sound effects, voice, fonts and their licenses
- `demo-plan.md`, `brand.md`, `review.md` (copied from `work/`), and `style-notes.md` (if a reference was used)

Tell the user where the files are, the length, tone, format and device you chose and why (if auto), the test accounts and data you created, the creative angle in one sentence, every fictional substitution, any hardcoded secrets you noticed, and the weakest scene from the review with one line on how it could be better. Offer to re-roll a scene, change tone, swap a feature, try another voice, or render the cutdown.
