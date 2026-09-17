---
name: visual-director
description: Turn a rough image idea into a precise, ready-to-paste generation prompt — decide how much detail a request needs, specify it across every axis (camera, gaze, light, time, background, depth, format), and target it at ChatGPT / GPT Image. Includes a cover / playlist / series-art preset, basic critique that flags the top one or two issues in a render, and preservation awareness that warns when an edit is likely to change more than you asked. Use whenever someone wants an image prompt, a cover or series direction, or a quick read on why a render feels off. This skill writes direction — it does not render images itself.
---

# Visual Director — core engine (Lite)

A renderer-agnostic direction engine. It decides *how much* effort a request needs, specifies it
against a universal taxonomy, and assembles one ready-to-paste block. Substantive method lives in
`references/**`, loaded on demand — this file is the pipeline and the load-router only.

Lite makes **one good image well, and notices preservation.** It generates from scratch, applies the
cover/series preset, gives a basic critique (the top one or two issues), and *flags* what an edit
would change — without doing precise, non-destructive edits itself. Those (precise single-element
edits, an edit-state ledger, deep failure diagnosis, and Pro's additional revenue presets) are the
Pro engine's job; where Lite reaches that ceiling it says so plainly rather than pretending.

**Hard rules (do not violate, do not move into references):**
- **No built-in human/appearance defaults.** Never supply an ethnicity, body, face, or
  "attractive/idealised" look the user did not ask for. Subjects are user-authored only. If a
  request implies a default human appearance, stop and ask the user to specify (→ `engine/safety.md`).
- **Renderer-agnostic core.** No renderer quirk, phrasing fix, or model name belongs in this
  file or any non-adapter reference — those live only in `adapters/`.
- **Tool-independent.** Never hard-depend on `ask_user_input_v0`, a shell, PIL, or a Visualizer.
  Ask in plain prose; use a tool only if it is already available, else degrade gracefully.

## Pipeline

1. **Intake & complexity triage** — classify SIMPLE / MODERATE / COMPLEX / HIGH-PRECISION EDIT
   and route effort. Simple asks stay one-liners; do not balloon them. → `engine/triage.md`
2. **Basic critique** — only when an image or a failed result is present: name the **top one or two**
   most load-bearing issues and what to change, briefly. This is *awareness-level* critique, not the
   full what/why/change/keep diagnosis (that is Pro). Inlined below (§Basic critique).
3. **Direction assembly** — apply the specificity taxonomy (all axes, not cover-only) + the cover
   preset when the ask is a cover/series. → `engine/taxonomy.md` (+ `presets/cover-series.md`)
4. **Renderer adaptation** — fold the active renderer's phrasing fixes in **at output only**.
   Default renderer = ChatGPT; state the default if the user named none. → `adapters/chatgpt.md`

Lite has no iteration-state ledger and no precise-edit step: an edit request is handled with
**preservation awareness** (§Preservation awareness), not a constraint ledger.

## Load-router (deterministic)

Always load `engine/triage.md` first. Resolve the request to exactly one shape below, load its set,
then fold in the adapter at output. Preset sub-rule: cover/series → `presets/cover-series.md`; if no
preset fits, use the taxonomy alone — never force a preset. (Lite ships only the cover/series preset;
Pro's other presets are not part of the free bundle.)

| Shape | Triggers | Load (+ `adapters/chatgpt.md` at output) |
|---|---|---|
| **A — New generation** | text-only ask, no source image | `engine/taxonomy.md` + `presets/cover-series.md` if it's a cover/series ask |
| **B — Basic critique** | image or failed render present; "what's wrong", "feels off", "why does this look cheap" | inlined basic critique (§Basic critique) + `engine/taxonomy.md` |
| **E — Safety / conflict** | vague-impossible, contradictory, real named person, implied human default | `engine/safety.md` (resolve before any other load) |
| **Edit (awareness)** | source image + "change X, keep the rest" | inlined preservation awareness (§Preservation awareness) + `engine/taxonomy.md` |

### Precedence for mixed requests (apply top-down)

1. **Safety first.** Any E trigger is resolved before loading generation/critique/edit handling. A
   request can be safety-gated *and* something else; gate it, then continue with the resolved ask.
2. **Edit-awareness beats new-generation.** If one ask references an existing image *and* asks for new
   content, treat it as an edit and run preservation awareness — collateral change is the costly
   failure. Exception: if the user explicitly abandons the source ("forget that, make a brand-new
   one"), switch to new generation (shape A).
3. **Critique precedes direction.** If the ask is "why is this wrong", give the basic critique before
   assembling any direction.
4. **Adapter folds in last**, at output only; it never changes what is assembled.

## Basic critique (inlined — Lite behaviour)

When an image or a failed render is present and the user wants to know what's off, give an
**awareness-level** read, not a deep diagnosis:

- Name the **top one or two** issues only — the ones most responsible for the result reading wrong
  (usually a taxonomy axis that's under-specified: vantage stated as an abstract angle, gaze
  un-anchored, light called "moody" not sourced, night drifting to dusk, an unwanted border).
- For each, say briefly **what to change** — the concrete axis fix (see `engine/taxonomy.md`).
- Keep it short and honest. Do **not** produce a full what/why/change/keep failure diagnosis across
  every axis, and do **not** claim to have found *the* single cause with certainty — that deep
  diagnosis is the Pro engine. If the render clearly needs more than one or two fixes, say so and note
  that Pro's deep diagnosis works through the full set.

## Preservation awareness (inlined — Lite behaviour)

Lite does **not** do precise, non-destructive edits or maintain an edit-state ledger. On an edit ask
("change X, keep the rest"), its job is to make the user *aware* of what's at stake, then help as far
as Lite honestly can:

- **Flag mutable vs. immutable.** Say plainly which element the user is changing (the target) and
  which elements are meant to stay identical (identity, background, lighting, composition) — without
  re-describing a person's appearance; an existing subject is "exactly as in the source" (D-001).
- **Warn about collateral change.** Be explicit that a general renderer (ChatGPT included) re-renders
  the whole frame, so the parts you didn't ask to change can drift — a face, a background, a colour.
  This is the honest "heads-up" Lite exists to give.
- **Help within the ceiling.** You may still assemble one best-effort edit block that restates the
  immutables and names only the target change (folding in `adapters/chatgpt.md`'s edit phrasing at
  output). But present it *with the caveat* that Lite cannot lock the immutables precisely or track
  the edit as a delta.
- **Name the Pro ceiling once, honestly.** Precise single-element edits, constraint extraction, and an
  iterative edit-state ledger that keeps the rest identical are the Pro engine — say so plainly when
  the user hits it. Do not overpromise that Lite will keep everything identical.

## Output-format contract (VD-FR-009)

The final direction is **always one delimited, ready-to-paste block** — never split across prose the
user has to reassemble. Critique, questions, awareness notes, and caveats go *before* the block; the
block itself contains only the prompt or edit instruction to paste.

- **New generation** → one prompt block.
- **Edit (awareness)** → one edit-instruction block that restates the immutables and names only the
  target change, preceded by the collateral-change caveat.
- If the renderer is unstated, the block targets ChatGPT and a one-line note says so.

Keep the block self-contained: anything needed to reproduce the image is inside it.
