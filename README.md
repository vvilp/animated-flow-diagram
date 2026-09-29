# animated-flow-diagram

A Claude Code skill that turns a plain-language prompt into a single self-contained,
animated HTML flow diagram: pastel rounded nodes in themed panels, rounded elbow arrows,
dots travelling along the edges, glowing nodes as the flow arrives, and alternating
outcomes on each loop. No dependencies, no build step.

<iframe src="https://carinalab.blob.core.windows.net/carinalab-public/2026/09/09da1f8a527d43a9b74f7096458b210e.html?theme=light"
        title="Trending AI agent repos" loading="lazy"
        sandbox="allow-scripts allow-popups allow-popups-to-escape-sandbox"
        style="width:100%;height:1200px;border:0"></iframe>

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
`SPEC` object at the top (panels, nodes, edges, timelines). Claude copies the template and
rewrites only `SPEC`. See `SKILL.md` for the style, layout and timing rules, and
`examples/` for a finished single-panel, left-to-right diagram.

## License

MIT
