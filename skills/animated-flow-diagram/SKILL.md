---
name: animated-flow-diagram
description: Build a self-contained animated HTML flow diagram in a hand-drawn "whiteboard" style — pastel rounded-box nodes inside soft themed panels with a numbered title pill, rounded orthogonal arrows, glowing dots travelling along edges, orange pulse on nodes as the flow arrives, and alternating outcomes on each loop. Use when the user asks for an animated diagram, flow/pipeline/architecture/process animation, "diagram like system1-vs-system2", a side-by-side comparison of two flows, or any boxes-and-arrows explainer they want animated in HTML.
---

# Animated flow diagram

Produces one `.html` file: an inline SVG diagram plus a small engine that animates it. The
look is fixed by `assets/template.html`; you only write the `SPEC` object.

## Workflow

1. **Understand the flow.** List the stages, what feeds what, where it branches, and which
   branches are alternatives (only one happens per run) versus parallel (all happen at once).
   If the user gave a vague prompt, pick a sensible flow yourself; don't interrogate them.
2. **Copy the template.** `cp ~/.claude/skills/animated-flow-diagram/assets/template.html <out>.html`
   (default `<out>` = a kebab-case name of the topic in the current directory). Change `<title>`.
3. **Replace `SPEC`** (everything between `const SPEC = {` and the closing `};`). Don't
   edit the engine unless the user asks for a new capability.
4. **Check it renders.** Serve the folder (`python3 -m http.server`) and screenshot with the
   Playwright tools if they're available; otherwise re-read your coordinates against the
   layout rules below. Fix overlaps, arrows that cross nodes, labels on lines, and cramped gaps.
5. Tell the user the file path and, in one line each, what the alternating outcomes are.

## Style rules (what makes it look right)

**Colour means role — keep it consistent:**

| node `color` | use for |
|---|---|
| `purple` | inputs: request, goal, state, data sources |
| `white` | neutral transformation: parse, route, format, embed |
| `teal` | the model/thinking step: LLM, planner, judge, verifier |
| `yellow` | gates, humans, fallbacks, retries: policy gate, escalate, ask user, replan |
| `blue` | actions and tools: execute, search, API, code, final action |

**Panels** group one flow each. Use one panel for a single flow and two side by side for a
comparison (`teal` left, `blue` right, badges `"1"` / `"2"`). There are also `purple` and `amber` themes for
a third panel or a one-panel diagram. Every edge's `ink` should match its panel's theme.
Give every panel a caption of one or two short lines (~70 chars each) saying what the flow
*means*, not what it contains.

**Labels:** 1–3 words per line and at most 2 lines, split with `\n`. Title-case or sentence
case, no trailing punctuation. Short labels are part of the look.

**Solid vs dashed:** solid = the primary/happy path. `dashed:true` = escalation, fallback,
ask-human, or loop-back. Those draw grey no matter what the panel's ink is.

## Layout rules

Coordinates are SVG units; the viewBox is fitted to the panels automatically.

- **Top-to-bottom** (default, `dir:"v"`): rows ~130–160 units apart (node bottom → next
  node top ≥ 55). The first row of nodes starts ~110 below the panel top to clear the pill.
  Keep ~90 units free between the last node and the panel bottom for the caption.
- **Left-to-right** (`dir:"h"`): gaps between columns of **≥ 70** units, more if an
  edge carries a label. Use this for linear pipelines with 5 or more stages.
- Node sizes: w 128–210, h 72–80. Nodes in the same row share `y` and `h`.
- Panel width ~520 for a two-panel diagram (total ~1090 wide); ~1080 for one panel.
  Leave at least 50 units between the panel edge and any node.
- **Fan-in / fan-out:** when several edges meet at one node, spread their anchors with
  `tx` (arriving) or `fx` (leaving), e.g. `-56 / 0 / +56`, and give them a shared `mid` so the
  elbows line up. A child directly below its parent needs no offset.
- **Loop-backs** (replan → planner) can't be auto-routed: write a raw `d` that goes out the
  right side of the source node, up the panel's inner margin, and into the target's right side,
  with a `Q` corner of radius ~10, as `x_loop` does in the template.
- No edge may pass through a node. If routing would, move the node rather than hand-drawing the path.

## Animation rules

- One `timeline` per panel; the timelines run independently and each loops forever.
- Steps are `[edgeId, start, dur, variant?]`. Sibling edges that happen in parallel share
  the same `start`. Leave ~0.3–0.4 s between a dot arriving and the next dot leaving; the arrival
  glow lasts 1.1 s. Dot durations are 0.6–0.9 s (1.2–1.6 s for long loop-back paths).
- **Variants** show the different outcomes: `variants:N` rotates across cycles, and a step tagged
  `variant:k` only plays on cycle *k*. Tag every edge leading to an alternative ending,
  including any follow-on step such as a loop-back.
- `cycle` = last step's end + ~1.5 s rest.
- Glows are derived automatically. Only add `hits` for a node you want pulsed without a dot arriving.
- There are no controls: the animation starts on load and loops forever. With
  `prefers-reduced-motion` the diagram stays static. Don't add a Pause button.

## Reference

- `assets/template.html` — the engine with the original "System 1 vs System 2 harness"
  two-panel spec (vertical flow, fan-in, fan-out, three variants, loop-back).
- `examples/rag-pipeline.html` — a single left-to-right panel with edge labels and a dashed fallback.

Keep the output one self-contained file with no external requests. The font is the system's
Comic Sans / Chalkboard stack on purpose, because it gives the hand-drawn feel.
