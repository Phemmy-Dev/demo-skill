# Voiceover (`--voice`)

Read this in full whenever `--voice` is set or the user asks for narration. Proof you followed it: the plan has a "Voiceover" section with the voice name and a table of each line, its scene and its start time; one `.wav` per line exists in `work/voice/`.

Write a natural script that adds to the visuals instead of reading them: short sentences, spoken rhythm, about 2.5 words per second, with breathing room before key reveals. Time each line to its scene and keep it inside the duration; if it doesn't fit, cut words, never speed the voice up.

Use **Kokoro** (open, runs locally, sounds natural). In `work/`, `npm i kokoro-js` (first run downloads the ~90MB model from Hugging Face; ask before installing):
```js
import { KokoroTTS } from 'kokoro-js';
const tts = await KokoroTTS.from_pretrained('onnx-community/Kokoro-82M-v1.0-ONNX', { dtype: 'q8' });
const audio = await tts.generate(line, { voice: 'af_heart' });
await audio.save('work/voice/line-01.wav');
```
Generate one file per line so each can be placed on its scene. Default voice `af_heart`; British `bf_emma` or `bm_george`; male US `am_michael`. Honour a voice the user names, and list the choices if they ask. If Kokoro can't be installed, ask before falling back to the system's built-in voice, and warn that it sounds robotic. Never send the script to an online service without asking.

Duck the music 8-12 dB under the voice with short fades. Listen back (or check levels with FFmpeg `volumedetect`) to make sure every line is clear over the music.
