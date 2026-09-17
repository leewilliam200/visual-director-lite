# Renderer guide

Visual Director writes direction; you paste its block into an **image generator** to make the
picture. Each supported generator has a **renderer profile** — a plain-text reference file inside the
bundle that adjusts only *how the finished block is phrased* for that model. This guide explains the
five profiles and how they work.

## The five renderers

These are the only renderers Visual Director supports.

| Renderer | Bundle |
|---|---|
| **ChatGPT / GPT Image** — the default (renderer of record) | Lite + Pro |
| Midjourney | Pro |
| Google Gemini (image) | Pro |
| Ideogram | Pro |
| FLUX | Pro |

**Tuned for ChatGPT / GPT Image.** It's the default and the renderer the profiles are verified
against. If you name no renderer, the block targets **ChatGPT / GPT Image** and says so in one line.

**Midjourney, Gemini, Ideogram and FLUX** (Pro) are included as **starting points that improve with
use** — each is documented from that model's well-known public behaviour and gets sharper as real
quirks are confirmed in practice. They're an honest head start, not a claim of verified behaviour, so
treat their phrasing as a strong first draft you refine to taste.

## How the profiles work

- **Plain text, no integration.** A profile is a reference file inside the bundle. There is **no
  API, no runtime connection, no code, and no keys** — Visual Director does not talk to any renderer.
  You copy its text block and paste it into the generator yourself.
- **Phrasing only, folded in last.** A profile changes only *how the block is worded* for a given
  model. It never changes what the engine decided — not the triage, not the critique, not what an
  edit preserves. It's applied at output, as the final step.
- **Universal principles live elsewhere.** Things that help on *every* renderer (state the camera
  physically, anchor the gaze in-scene, say "full dark" for night, state a border as an intent) are
  part of the engine's shared taxonomy, not a per-renderer quirk. A profile holds only what is
  genuinely specific to that one model.

### Example: what the ChatGPT / GPT Image profile does

To give a sense of what a profile actually does, ChatGPT / GPT Image gets fixes like:

- Write **natural-language prose**, not tag-salad — no `--ar`, `--no`, or weight flags; a Midjourney
  flag becomes a spoken clause.
- **State aspect ratio in words** ("a square image", "a vertical 2:3 portrait"); it snaps to the
  nearest supported shape.
- **Phrase the wanted state positively** — negation is followed weakly, so "fills the frame,
  edge-to-edge full bleed" beats "no border".
- On an **edit**, restate the immutables and add "keep everything else exactly as in the source
  image, unchanged", because GPT Image re-renders the whole frame rather than inpainting locally.
- **In-image text** is more reliable when short and in "double quotes"; long strings still degrade.

The other four profiles document their own models' behaviour the same way.

## Choosing a renderer

- **Just starting, or want the most reliable results?** Use the default, ChatGPT / GPT Image — it
  needs no extra step and it's the renderer the profiles are tuned against.
- **Prefer Midjourney, Gemini, Ideogram, or FLUX?** Name it (Pro) — you'll get a documented head
  start that improves with use.
