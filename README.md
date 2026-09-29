# animated-flow-diagram

A Claude Code skill that turns a plain-language prompt into a single self-contained,
animated HTML flow diagram with rounded elbow arrows, particles travelling along the
edges, and nodes that glow as the flow arrives. No dependencies, no build step.

Three themes:

- **dark** (default): dark glass cards with icons, grouped lanes, comet particles.
  See `examples/rag-pipeline-dark.html`.
- **light**: the same modern look on a clean light page. See `examples/rag-pipeline-light.html`.
- **whiteboard**: hand-drawn pastel boxes in themed panels, with alternating outcomes on each
  loop. See `examples/rag-pipeline.html`.

Say "dark", "light" or "whiteboard" in your prompt to choose.



https://github.com/user-attachments/assets/8b50562e-0683-494b-abe3-3514dda0947e




https://github.com/user-attachments/assets/4b292629-68ed-4d30-ae7e-eedcd09c6226



## Install

In Claude Code:

```
/plugin marketplace add vvilp/animated-flow-diagram
/plugin install animated-flow-diagram@animated-flow-diagram
```

Or without the plugin system:

```
git clone https://github.com/vvilp/animated-flow-diagram /tmp/afd
cp -R /tmp/afd/skills/animated-flow-diagram ~/.claude/skills/
```

## Use

Just describe the diagram:

> Make an animated diagram of how a PR goes through CI: lint and tests run in parallel,
> then either deploy to staging or go back to the author.

> Make a side-by-side animated diagram, same style as system1-vs-system2, comparing a
> single-agent and a multi-agent research workflow.

Or call it directly: `/animated-flow-diagram <description>`.

## How it works

`skills/animated-flow-diagram/assets/template.html` contains a fixed drawing engine and a
`SPEC` object at the top (panels, nodes, edges, timelines) for each theme (`template.html`,
`template-dark.html`, `template-light.html`). Claude copies the template and rewrites only `SPEC`. See `SKILL.md`
for the style, layout and timing rules, and `examples/` for the same diagram in both themes.

## License

MIT
