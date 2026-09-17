# Renderer guide

Visual Director writes direction; you paste its block into an **image generator** to make the
picture. Each supported generator has a **renderer profile** — a plain-text reference file inside the
bundle that adjusts only *how the finished block is phrased* for that model. This guide explains the
five profiles, what their confidence labels mean, and how the profiles work.

## The five renderers

These are the only renderers Visual Director supports. There is no other renderer profile, shipped
or planned-as-shipped, and none should be claimed.

| Renderer | Confidence | Bundle |
|---|---|---|
| **ChatGPT / GPT Image** — renderer of record, v1 default | **Tested** | Lite + Pro |
| Midjourney | Starter (*improves with use*) | Pro only |
| Google Gemini (image) | Starter (*improves with use*) | Pro only |
| Ideogram | Starter (*improves with use*) | Pro only |
| FLUX | Starter (*improves with use*) | Pro only |

**Default behaviour:** if you name no renderer, the block targets **ChatGPT / GPT Image** and says so
in one line. Name another renderer and the matching profile is used instead — but see the labels
below.

## Confidence labels — read this before trusting a profile

Every profile carries exactly one label so you always know how much to trust its quirks:

- **Tested** — verified against the renderer of record. The phrasing fixes are ones we rely on and
  confirm in use, not guesses. **Only ChatGPT / GPT Image is Tested in v1.**
- **Starter** — documented from well-known public behaviour but **not yet verified by us**. A
  reasonable starting point that **improves with use** as real quirks are confirmed. The four
  Starter profiles (Midjourney, Gemini, Ideogram, FLUX) ship in **Pro only**.

A **Starter profile is not a Tested one.** It won't be presented as tested anywhere, and it isn't a
promise of verified behaviour — it's a documented head start that gets sharper as it's used. Only
ChatGPT carries the Tested label today.

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

### Example: the ChatGPT / GPT Image profile (Tested)

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

The four Starter profiles document their own models' behaviour the same way, at Starter confidence.

## Choosing a renderer

- **Just starting, or want the most reliable results?** Use the default, ChatGPT / GPT Image — it's
  the Tested profile and needs no extra step.
- **Prefer Midjourney, Gemini, Ideogram, or FLUX?** Name it (Pro), and expect a Starter-quality head
  start that improves with use rather than verified-tested behaviour.
