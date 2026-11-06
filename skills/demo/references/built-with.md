# How it was built (`--built-with`)

Read this in full whenever `--built-with` is set or the user wants to show how the project was made. Proof you followed it: you showed the user the evidence summary and got their facts before writing any claim, and the plan has an "Evidence" section.

Gather evidence from the repo and show it to the user before writing any claim:
- commit count and date range (`git log --format='%ad' --date=short`)
- commit authors and `Co-authored-by` trailers
- agent files: `AGENTS.md`, `.codex/`, `.openai/`, `CLAUDE.md`, `.claude/`, `.gemini*`, `.cursor*`, `.github/copilot*`

Report what you found, including evidence of tools other than the one named. Then ask the user for 2-3 facts in their own words: what they asked the tool to do, what they directed or fixed themselves, and one real moment (a prompt, a bug, a decision). Use only facts the user confirms plus the git numbers. Never invent prompts, timelines, percentages or quotes. Never suggest hiding or rewriting evidence of other tools. If the evidence contradicts the claim, say so plainly and let the user decide; if they give no facts, drop the chapter.

The chapter takes ~15-20% of the video, shown as prompt-to-result. In `share-copy.txt`, add one honest line on the tool's role.
