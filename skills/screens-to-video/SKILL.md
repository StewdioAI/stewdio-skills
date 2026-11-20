---
name: screens-to-video
description: "Make a ~30s launch film for one feature of a mobile app: pull the feature's screens from the app's Revyl Atlas, script it as a referenced beat-by-beat brief sourced from the mobile-motion are.na channel, build it in Stewdio (installing Stewdio's MCP server if it is missing), and hand over the File for an mp4 export. Use for \"launch film\", \"launch video\", \"feature film\", \"promo for <feature> in <app>\", or whenever a feature of an app in Revyl needs a launch video rather than a demo walkthrough."
---

# Screens to video

A feature, an app in Revyl, ~30s, 16:9. Three phases, in order, each ending in a check.
The worked example is `template/`: the Same Mood film for tidepool (`BRIEF.md` is its
script, `film.js` its timeline; it was built in HTML before this skill moved to Stewdio, and
before briefs carried a camera ref, so it has no k1).

**A launch film, not a walkthrough.** It sells one idea: a hook line before any UI, the
feature doing its own mechanism two or three times with rising pace, one turn where the
feature becomes bigger than the phone, and an end card. Screens are props in that arc;
never tour every screen, and never show settings, empty states, errors or system UI.

## Before you start: Stewdio

The film is built in Stewdio, a motion-graphics editor driven over MCP. Check for it
first, so an install can be approved while phases 1-2 run.

- **Claude Code**, in this order:
  1. Already there: Stewdio's tools are in the session, or `claude mcp list` shows it. Done.
  2. The connector, when Claude Code is signed in with a Claude account: the user connects
     Stewdio from Claude's connector directory (on Team and Enterprise plans an admin adds it
     for the organization), then runs `/mcp`, where Stewdio is listed under claude.ai. One
     sign-in then covers Claude on the web, desktop and Claude Code.
  3. The official plugin, when the connector can't load (Claude Code authenticates with
     `ANTHROPIC_API_KEY`, an `apiKeyHelper`, Bedrock or Vertex, or a `claude setup-token`
     token) or the user can't add it:
     `claude plugin marketplace add StewdioAI/plugin && claude plugin install stewdio@stewdio --scope user`,
     then the user runs `/mcp`, selects Stewdio and signs in.

  Both reach the same server with the same tools; never install the plugin on top of a
  working connector. If it is unclear how Claude Code is signed in, ask instead of guessing.
- **Codex:** `codex mcp list`; if missing, `codex mcp add stewdio --url https://mcp.stewdio.ai/mcp`,
  then `codex mcp login stewdio` for the user's browser sign-in.
- **Other agents:** add the remote server `https://mcp.stewdio.ai/mcp` (HTTP, OAuth sign-in)
  through the agent's own MCP setup, or ask the user to add it. Tell the user to approve it
  and sign in to Stewdio, then carry on with phases 1-2; only phase 3 waits.
- It needs a Stewdio account; the desktop app is optional. The Free plan allows 30
  content-changing MCP calls a week and puts a small "Made with Stewdio" mark on exports;
  Premium removes both. Mention this when installing.
- Connected means `read_documentation({ topic: "overview" })` returns and `list_files`
  answers (an empty list is fine).

If the user declines, ask whether to build in HTML instead (`template/film.js` +
`scripts/render.mjs`, the original path). Never switch silently.

## 1. Screens: Revyl Atlas, laser-focused

The CLI is `~/.revyl/bin/revyl` (already authenticated; `revyl auth login` otherwise).
Pass `--all-branches` to every atlas command.

```
revyl app list                                                   # app name + id
revyl atlas brief --app <id> --all-branches                      # areas, anchors
revyl atlas search "<feature words>" --app <id> --all-branches   # try 2-3 phrasings
revyl atlas screen <screen-id> --app <id> --all-branches         # summary, actions, in/out edges
revyl atlas neighbors <screen-id> --app <id> --all-branches      # entry points and exits
revyl atlas observations <screen-id> --app <id> --all-branches --screenshots --screenshot-dir atlas/<screen-name>
```

- Keep the feature's own screens, the one or two entry points that lead into it (the
  button or row a user taps) and the one outcome it ends on (saved, sent, shared). Drop
  everything else, even when it is connected.
- Read the edge labels: they name the exact control (`Same mood menu item`) and the
  gestures the feature answers to (swipe, pinch). Those gestures become beats.
- Sheet every screen folder (`python3 scripts/sheet.py atlas/<name> .scratch/sheet-<name>.png`)
  and look at all of them, then one full-size screenshot per screen for type, spacing
  and chrome. Screenshots are 1080x2400 Android: 1dp = 2.625px, screen = 412x914dp.
- Real content goes in one-to-one: `python3 scripts/crop_drops.py film/drops atlas/<screen>/*.jpg`
  cuts the image cards off a light canvas (skips status/nav chrome and edge-cut cards).
  Sheet the crops and keep the sharp unique ones; crop a hero by hand when detection
  merges two cards.
- Tell the user which screens were found before scripting (one line).

## 2. Script: a referenced brief from the mobile-motion channel

**References come before beats.** The source is the are.na channel
`mobile-motion-mnpotx847ma` (https://www.are.na/blake-shao/mobile-motion-mnpotx847ma):
the look stays inside it and every motion ref comes from it. Go outside it only for a
role it cannot fill, and say so.

1. **List and look.** `python3 scripts/arena.py channel mobile-motion-mnpotx847ma .context/refs/<topic>`
   lists every block and writes numbered sheets; read all of them. `references/channel-notes.md`
   has the blocks already vetted and what each is good for; start there, but check for
   new ones (the channel grows, newest first).
2. **Vet 10-15 candidates for real.** `arena.py get <media> .context/refs/<topic>/cand/cN.mp4`,
   then `arena.py sheet <mp4> <png> 9` and read the sheet. A tile is not a reference: keep
   it only if the move happens on screen as more than a cut or fade. Pick the most striking
   move that could carry the feature over the one that shows a similar screen.
3. **Nine references, one job each.**
   - a1-a5, stills grabbed from the channel's clips (`arena.py still <mp4> <t> a1.jpg`),
     one aesthetic: a1 overall look (the floating phone), a2 palette (backdrop + one
     accent), a3 material (frosted glass, shadow), a4 composition (title / end card), a5 a
     detail (a label floating off the ui).
   - m1-m3, clips, numbered in beat order: the main mechanic, the turn, the titles or the
     exit. A motion ref gives mechanic and timing only; the subject is always swapped.
   - k1, a clip, the camera: how the frame and the phone move. Pick it for its moves, not
     its screen: framing changes (whole phone -> macro crop of one control -> whole phone),
     push-ins, a 2D roll on the phone, pans that follow the content, blur-through cuts.
     Write down the numbers it implies (scale range, roll in degrees, how long a move takes,
     how long it holds) and the beats it drives. Stewdio is 2D, so a 3D tilt in the clip is
     taken as roll + scale + pan; say so in the ref line. `references/channel-notes.md`
     lists the vetted camera clips.
   - u-refs: the Atlas screen folders, rebuilt one-to-one. c-refs: the cropped content,
     used as-is.
   - Each ref states the beat it feeds and exactly what is taken, with a time range for
     clips (`m2 0-5s: images gathering around one center and scattering, subject swapped
     for drops`). A ref with no beat is cut.
4. **Beats.** Numbered, a time anchor on each, each one = subject + action + what it
   reveals or becomes + the camera: its framing and its move, with numbers. Default
   skeleton (fit the seconds to the feature):
   1. 0-3 hook line, words blur into focus one at a time (m1), no ui yet; camera: slow
      push 1.0 -> 1.04
   2. 3-7 the subject arrives, the phone rises around it rolled about -6deg and levels, a
      tap on the entry point; camera: wide, settling as the phone lands
   3. 7-13 the feature's main mechanic inside the phone, a title on the side; camera:
      push in on the hero (1.0 -> 1.2), drifting with any drag
   4. 13-18 the mechanic again, faster, chained; camera: a short pan timed to each tap,
      one macro crop (about 2x) on the tapped item, then back out
   5. 18-24 **the turn**: the feature outgrows the phone (spills past the bezel, zooms
      out to the whole, becomes the frame) + one line on frosted glass. The biggest beat;
      one turn, no second metaphor. Camera: punch in during the gesture (about 1.4x in
      0.4s), then the pull-back belongs to the camera, not only to the content
   6. 24-27 the outcome (saved / sent) as a chip floating out of the phone; camera:
      follows the chip out
   7. 27-30 end card: feature name + the app's accent dot, mono "now in <app>"; camera:
      slow push 1.0 -> 1.035

   Camera rules: no framing holds still longer than about 1.5s (a 2-3% drift at least);
   at least three framings across the film (whole phone, the screen, a macro on one
   control); every move starts on an action (a tap, a word landing) and settles with an
   ease-out; one punch per film, at the turn.
5. **Write mechanisms, not adjectives.** "related drops fly in from off-frame and settle
   around the clouds" gets built right; "a dynamic reveal" does not. Animate the feature's
   own contraption (tap -> follow, pinch -> zoom out, save -> chip), never a generic
   draw-in. Transformations are in-place morphs and handoffs, not cut-and-rebuild.
6. **Film frame, not a web page.** The phone is ~80% of the frame height; titles are 60px+
   at 1080p; one hero per beat.
7. **House shape**, lowercase, imperative, numbered, no filler, under ~180 words above
   `refs:`. The first line names the channel's aesthetic, not the app's (`in soft-white
   studio launch film`); the app's palette survives inside the phone and as the accent.

```
in stewdio, new file <feature>-launch, create a 30s launch film for <app>'s <feature>, in <aesthetic from a1>, the ui rebuilt one-to-one from u1-uN with the real content (c1):

1. 0-3s: <hook line> blurs into focus word by word like m1, on a2's <backdrop>
2. 3-7s: <subject> ... the phone rises like a1 ... a tap hits "<entry control>"; camera like k1 <range>: <framing> -> <framing>, <move with numbers>
3. ...; camera: <framing>, <move>
5. 18-24s, the turn: <how the feature outgrows the phone>, like m3 <range> <reversed?>; camera: <punch / pull-back>
...
<pace in words>; 16:9, 1920x1080, 30fps, no music

refs:
- a1 <path>: look, <what it sets, which beats> (<block url>, channel mobile-motion-mnpotx847ma)
- ... a2-a5, m1-m3 the same way
- k1 <path>: camera, <framings and moves taken, with numbers; 3D tilt taken as roll + scale>, beats <n-m>, <range> (<block url>, channel mobile-motion-mnpotx847ma)
- u1 atlas/<screen>/: ui, <screen>, one-to-one (revyl atlas, <app>, screen <id>)
- c1 film/drops/: copy, the real content cropped from the atlas, one-to-one
```

Save it as `film/BRIEF.md` with at most three fill-ins (the creative forks you chose, each
with the one alternative). `template/BRIEF.md` is a finished one. Do not stop for approval
unless the user asked to see the script first; go straight to phase 3.

## 3. Build: the film in Stewdio

**Read first, every time.** `read_documentation` for `overview`, `xml` and `graphic` (the
XML is strict: unknown elements and attributes are rejected), then `list_skills`: the
user's own Stewdio skills win where they conflict with this section. `template/film.js`
is the worked example's timeline at 1920x1080 (keyframe times, easings, positions,
handoffs): port its timing, not its code.

1. **File.** `create_file({ name: "<feature>-launch" })` once; keep `id` and `url` exactly
   as returned. After a failed or timed-out call, `list_files` before creating again.
2. **Assets.** Upload the kept crops and nothing else (the ui is rebuilt, not screenshotted):
   `create_assets({ fileId, uploads: [{ name: "c1-<n>" }, ...] })`, then POST each file to
   its single-use url within the hour: `curl --data-binary @film/drops/<n>.jpg "<url>"`.
   Save every returned `hash`, width and height to `film/drops/assets.json`; images are
   placed by hash at their native aspect ratio.
3. **Layers**, split wherever someone would edit independently, back to front, with
   descriptive names. One root `<composition name="<feature> launch" duration="30"
   size="1920 1080" background="<a2 backdrop>">` holding:
   - `Backdrop` Graphic only if the backdrop moves or has grain; otherwise the
     composition background does it.
   - `Camera` composition (background `#00000000`) holding every layer below. The shot
     lives on its position, scale and rotation keyframes, ported from k1 and the beats'
     camera clauses; the `Phone` keeps only its own moves (rising in, leaving). Make it
     larger than the frame (for example 2400x1350, centred) when it pulls back below
     scale 1, since a composition crops what lies outside it.
   - `Phone` group: `Phone shadow` Graphic (long soft shadow, sized to include its
     blur), `Body` vector (rect with corner-radius), and one nested composition per screen
     (`Screen: <name>`, 412x914, background the app's surface). Each screen holds a
     `<name> ui` Graphic drawing the chrome one-to-one from the screenshot (bars, mono
     labels, pills, icon weight; fonts as font inputs), the drops as native `image`
     layers, and the touches as `vector` ellipses on top. Scale the group so the phone
     is ~80% of frame height. The screen composition crops to the screen, which is what
     lets content be clipped inside the phone and released for the turn.
   - Stage-level copies of the drops that escape in the turn, as `image` layers
     outside the phone.
   - Titles: one Graphic per line (`Hook`, `Title <n>`, `Turn line`) for the
     word-by-word blur, with a `copy` string input, a font input (`Geist` from Google
     Fonts, or the app's own), size and color inputs, and one `progress` number input
     keyframed 0->1. A title with no blur is native `text`.
   - `Frost pill` (translucent white rect vector with a hairline stroke) behind the
     turn line, a `Saved chip` group, and an `End card` group (native text name, the
     accent dot as a vector, mono "now in <app>").
4. **Motion.** Position, scale, rotation and opacity are field keyframes on the layers
   (`<animation field="position">`, `easing="bezier(x1 y1 x2 y2)"`); custom effects are
   input keyframes on Graphics. Nothing is timed inside a script: every Graphic draws
   from its inputs and `graphic.size` alone, so any frame renders on its own. Clips
   (`clip-start`, `clip-duration`, `clip-trim`, always together) sequence screens and
   titles. The rules that made the worked example read well still hold:
   - handoffs between layers happen on one frame at an identical rect (in-screen drop
     -> stage drop for the turn; compute the stage rect from the phone group's position
     and scale, inside the Camera's); never crossfade two copies of the same thing
   - ~0.5s between an outgoing title and the next one; overlapping blurred words read
     as a glitch
   - touches live inside the screen composition so they ride the moving phone: press is
     scale 1 -> 0.85, the ripple a second ellipse scaling up while it fades
5. **Write in few calls.** One `write_xml` per top-level piece (the root composition,
   then each screen composition with `parentId`, then the titles), never one per layer:
   the Free plan counts every content-changing call. On diagnostics fix every listed
   line and write again; after a failed creation, `read_xml` with `depth: 1` before
   retrying so nothing is created twice. Later fixes go through `edit_xml` on the node
   you just read.
6. **Check with capture.** `capture` renders in the user's open Stewdio session, so
   the File has to be open: on the user's own computer `open "<url>"`, otherwise give
   the user the url and ask. Capture the root composition at every beat, at most 8 times per call:
   `[1.2, 4.7, 9.5, 13, 16, 19, 21, 25]`, then `[29, 9.5]` (ending on an earlier time
   shows no frame depends on the one before). Check for text overlap, touches landing on
   their targets, clipping at motion extremes, the turn reading as a turn, the camera
   moving in every beat (compare the phone's rect across captures: two beats with the
   same framing means a missing move), and every element still being its own
   selectable layer. Fix and capture again. Captures are
   previews, not the export; never report them as the final video.
7. **Export is the user's click**; Stewdio has no export tool. In Stewdio Studio: select
   the root composition -> Export in the inspector -> MP4 (H.264), Size 100%, 30 fps ->
   Export video (mute the composition's Sound first for a silent film). Suggest saving to
   `film/out/<feature>-launch.mp4`; once it is there,
   `python3 scripts/arena.py sheet <mp4> <png> 25` checks continuity.

## Deliver

The Stewdio File `url` exactly as returned, two captured frames (the mechanic, the turn),
the beats with their times one line each, the export steps above, and the open choices:
copy, music, anything the Atlas did not cover. Point at `film/BRIEF.md` for script edits;
layout and timing edits happen in Stewdio, by hand or by asking.
