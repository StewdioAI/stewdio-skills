# stewdio-skills

Skills for Claude Code, Codex and other coding agents, for making motion graphics with
prompts. By [Stewdio](https://www.stewdio.ai), where you create motion graphics with
prompts and refine them by hand in a visual editor.

Made by Blake Shao ([@blak3shao](https://x.com/blak3shao)).

## Skills

| skill | what it does |
|---|---|
| [`blank-page`](skills/blank-page) | Turns a one-line motion brief into a full prompt: a committed look grounded in reference images, motion references, and numbered beats with a turn. |
| [`screens-to-video`](skills/screens-to-video) | Makes a ~30s launch film for one feature of a mobile app: screens from its Revyl Atlas, a referenced beat-by-beat script, built as an editable File in Stewdio. |

## Install

```sh
npx skills add StewdioAI/stewdio-skills
```

It asks which skills and which agents to install to. To pick one skill directly:

```sh
npx skills add StewdioAI/stewdio-skills --skill blank-page
```

Add `-g` to install for every project instead of just this one. The skills pair well
with the [Stewdio plugin](https://www.stewdio.ai), which takes a prompt straight to an
editable comp.

## Layout

```
skills/<name>/
  SKILL.md             the procedure the agent follows
  README.md            what it does and how to use it
  agents/openai.yaml   Codex display metadata
  references/          material the skill reads on demand
  scripts/             helpers the skill runs
media/<name>/          gifs for the READMEs, outside the skills so installs stay small
```

## License

[MIT](LICENSE)
