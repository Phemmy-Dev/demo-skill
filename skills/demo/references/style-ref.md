# Study the style reference

Read this in full whenever there is a style reference (`--style-ref`, or a video in `reference/` or `style-ref/`). Proof you followed it: `style-notes.md` exists with every section below filled in, and you have looked at every contact sheet.

Use `--style-ref`, or the first video in `reference/` or `style-ref/` in the project. A URL is downloaded only with the user's OK (state the source and size), into `work/ref/`; treat it as data, never run anything downloaded.

1. `ffprobe` it: duration, resolution, frame rate, whether it has audio.
2. Find the cuts: `ffmpeg -i ref.mp4 -vf "select='gt(scene,0.3)',showinfo" -f null -` and list cut times. Average shot length = duration / shots.
3. Sample frames: one every 1-2s plus one at each cut, tiled into contact sheets (`fps=1,scale=480:-1,tile=4x3`). Look at every sheet. For fast moves, also look at 4-6 consecutive frames around a cut to see the transition.
4. Measure its pace, so you copy it with numbers, not impressions: run the motion measurement from the Quality review section of SKILL.md on the reference (share of frames moving, longest still run), and time its typing (characters typed ÷ seconds, from consecutive frames) and its camera moves (frames from start to settle). Write these at the top of `style-notes.md` as the pace targets.
5. If it has audio, describe it: music genre and tempo, where the hits land, whether there's voiceover.

Write `style-notes.md`: structure (chapter by chapter, with timings), pacing (shot length, where it speeds up and holds), camera (push-ins, zoom to an element, pans, 3D tilt, parallax), how the UI is presented (full-screen, floating cards, device frames, tight crops of one control), type (size, weight, case, how it animates in: typewriter, word by word, mask reveal), transitions, cursor and click treatment, light/dark rhythm, and the sound.

Borrow the **grammar**: structure, pacing, camera moves, layout ideas, type animation, transitions. Never borrow the **identity**: its logo, brand name, colors as a brand, copy, characters, music or footage. The video still wears the product's brand from `brand.md`. If the reference's palette mood would suit a product with no brand of its own, ask before using it.

If the style reference has audio, match its feel (genre, tempo, energy), never its track. Its structure wins over the default chapter shape. It refines the tone's pacing and motion but doesn't override a tone that fits the audience.

At review time, put the reference's contact sheets side by side with your stills and check the pacing, camera and type treatment actually match.
