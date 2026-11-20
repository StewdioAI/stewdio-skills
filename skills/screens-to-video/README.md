# screens-to-video

A skill for Claude Code, Codex and other coding agents that makes a ~30 second launch film for one
feature of a mobile app. It pulls the feature's screens from the app's
Revyl Atlas, writes a referenced beat-by-beat script sourced from the
[mobile-motion](https://www.are.na/blake-shao/mobile-motion-mnpotx847ma) are.na channel,
and builds the film as an editable File in [Stewdio](https://www.stewdio.ai), ready for
an mp4 export.

It makes a launch film, not a walkthrough: a hook line, the feature doing its own
mechanism two or three times with rising pace, one turn where the feature outgrows the
phone, and an end card.

## Install

```sh
npx skills add StewdioAI/stewdio-skills --skill screens-to-video
```

## How to use

Name the feature and the app:

```
make a launch film for saved collections in my app
```

"launch video", "feature film" and "promo for <feature> in <app>" work too.

## How it works

1. **Stewdio.** Checks that Stewdio's MCP server is connected and installs it if not, so
   the sign-in can happen while the next two phases run.
2. **Screens.** Finds the feature's own screens, its entry point and its outcome in the
   Revyl Atlas, and crops the real content out of the screenshots.
3. **Script.** Picks five look references, three motion references and one camera
   reference from the mobile-motion channel, and writes numbered beats with times and
   camera moves to `film/BRIEF.md`.
4. **Build.** Creates the File in Stewdio with every element as its own layer, checks
   frames at each beat, and hands over the File with the export steps. Export is a click
   in Stewdio.

## Layout

```
skills/screens-to-video/
  SKILL.md                    the procedure
  agents/openai.yaml          Codex display metadata
  references/channel-notes.md vetted blocks from the mobile-motion channel
  scripts/arena.py            are.na listing, downloads, stills and contact sheets
  scripts/sheet.py            contact sheets of Atlas screenshots
  scripts/crop_drops.py       crops image cards out of screenshots
  scripts/render.mjs          frame-by-frame render for the HTML fallback
  template/                   a worked example: brief and HTML timeline of one film
```

## Requirements

- a Revyl account with the app in Atlas, and the `revyl` CLI signed in
- a [Stewdio](https://www.stewdio.ai) account (the free plan works, with a weekly MCP call limit and a watermark on exports)
- `python3` with Pillow, NumPy and SciPy, `curl`, and `ffmpeg`
- for the HTML fallback only: Node and Playwright (`PLAYWRIGHT_DIR` points at it)

## License

[MIT](../../LICENSE)
