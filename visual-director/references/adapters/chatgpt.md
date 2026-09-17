# ChatGPT / GPT-Image adapter

**Profile confidence: `Tested`** · Renderer of record (v1, D-002).

Loaded at **Pipeline step 6 (renderer adaptation)** and folded in **at output only** — it never
changes what triage decides, what critique reads, or what constraint extraction preserves. It only
adjusts how the finished block is *phrased* for this renderer. This file is the **one and only**
place ChatGPT/GPT-Image quirks live (D-002); nothing renderer-specific leaks back into the core,
the taxonomy, or any preset.

---

## 1. Default renderer (the EVAL-016 fix)

**When the user names no renderer, the direction targets ChatGPT / GPT-Image, and the block says so
in one line.** No silent fallback to any other engine. This is the stated renderer of record for v1
(D-002); the seed's failure was defaulting to Nano Banana without saying so (EVAL-016, MODEL 0).

If the user *does* name another renderer, load that adapter (`adapters/<renderer>.md`) instead and
skip this one — but note those are `Starter` profiles (see §2). Never present another renderer's
block as if it were the tested default.

---

## 2. The `Tested` / `Starter` label convention  (defined here; reused by VD-CORE-012)

Every adapter file carries exactly one confidence label at the top, so customer-facing copy and the
engine both know how much to trust its quirks:

- **`Tested`** — verified against the renderer of record. The fixes are ones we rely on and confirm
  in use, not guesses. **ChatGPT / GPT-Image only in v1.** This file.
- **`Starter`** — documented but not yet verified on that renderer; a reasonable starting point that
  **"improves with use"** as real quirks are confirmed. All four VD-CORE-012 profiles
  (Midjourney, Gemini, Ideogram, FLUX) ship as `Starter`.

**No-overpromise (spec-contract §6):** never label a profile `Tested` it hasn't earned, and never
present a `Starter` profile as `Tested` in any customer-facing copy. A `Starter` profile promotes to
`Tested` only after its quirks are verified — the label is a claim, not decoration.

---

## 3. ChatGPT-specific phrasing fixes

These are quirks of **how GPT-Image reads a prompt**, distinct from *what* a direction pins down.
The universal principles — camera stated physically (AX-4), gaze anchored in-scene (AX-3),
background density/variety (AX-7), night stated as full dark sky (AX-6), borders stated as an intent
(AX-9) — already live in `engine/taxonomy.md` and apply on **every** renderer. **Do not restate them
here.** This section holds only what is specific to GPT-Image's phrasing behaviour:

| GPT-Image quirk | Fix at output |
|---|---|
| **Reads natural-language prose, not tag-salad or flags.** No `--ar`, `--no`, weight syntax, or comma-keyword lists. | Write the block as plain descriptive sentences. Anything you'd express as a Midjourney flag becomes a spoken clause. |
| **Aspect ratio is verbal, and snaps.** No ratio parameter; it renders to the nearest of its supported shapes (square 1:1, portrait 2:3, landscape 3:2). | State the ratio in words ("a square image", "a vertical 2:3 portrait"). Exotic ratios get snapped — pick the nearest supported shape deliberately rather than fighting it. |
| **Negation is weakly followed** — naming a thing to exclude often renders it. | Phrase the wanted state positively. For the AX-9 border default specifically, prefer "fills the entire frame, edge-to-edge full bleed" over "no border, no frame." |
| **Edits re-render the whole frame** — GPT-Image regenerates globally rather than inpainting locally, so unmentioned elements (identity, background, colour) drift. | In an edit block, restate the immutables verbatim and add an explicit "keep everything else exactly as in the source image, unchanged." This is the *output phrasing* that carries the preservation the edit modules already decided — it does not do preservation itself. |
| **In-image text is comparatively reliable when quoted and short.** | Put the exact words in "double quotes" and keep them brief; long or paragraph-length strings still degrade — don't promise them. |

Add a row only when a quirk is genuinely GPT-Image-specific *and* confirmed in use. If a fix turns
out to help on every renderer, it belongs in the taxonomy as a principle, not here.

---

## 4. Boundary (D-002)

- Nothing above changes triage, critique, constraint extraction, or the assembled direction — the
  adapter folds in **last**, at output only.
- Renderer-agnostic principles stay in `engine/taxonomy.md`; renderer quirks stay in `adapters/`.
  Keep the traffic one-directional: no taxonomy principle re-imported here, no GPT-Image quirk
  leaked back into the core or any preset.
