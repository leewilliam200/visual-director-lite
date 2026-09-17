# Quickstart

By the end of this page you'll have produced a ready-to-paste direction block, and you'll know how
to edit an existing image and build a consistent series. No prior context needed.

**Prerequisite:** the skill is installed and Claude has been reopened — see [install.md](install.md).

Remember the split: **Visual Director writes the direction; the image generator makes the picture.**
You paste its block into ChatGPT / GPT Image (the default) or another supported renderer.

---

## 1. Your first block — a cover from scratch

Ask Claude, in plain words:

> **Make a chill playlist cover, night vibe.**

For a simple ask, Visual Director stays in scale — it fills the key axes, states its assumptions out
loud, asks at most a question or two, and **invents no person you didn't describe**. You'll get a
short note, then one delimited block:

```
A calm, moody nighttime photograph for a chill playlist cover. A quiet empty street
after dark: warm amber streetlight pooling low in the frame against cool, deep-blue
night air. Full dark sky, no dusk or blue-hour glow. A few scattered warm-lit windows
far in the background, softly out of focus, spaced apart so they never compete with
the foreground. Soft, hazy, relaxed atmosphere.

Focal weight upper-centre; keep the lower third low-contrast and uncluttered for a
title later — render no text. Square 1:1 aspect ratio, edge-to-edge full bleed, no
border, no frame.

(Renderer unspecified → targeting ChatGPT. Medium is a proposed default — say the word
to switch to illustration/painting. No person included by default.)
```

**Copy everything inside the fence** and paste it into ChatGPT / GPT Image. That's your first
render.

A few things to notice, because they're the whole point:
- It **proposed** a medium ("photograph") and told you it's overridable in one word — it didn't bake
  one in silently.
- It said **full dark, no dusk** on purpose: renderers drift to blue hour, so a night ask has to say
  so outright.
- It **added no person.** If you want one, you describe them (next step).

---

## 2. Add a subject — *you* author the appearance

Visual Director ships **no** built-in human appearance and will never invent one (this is a hard
rule, D-001). To put a person on the cover, describe them yourself:

> **Add the subject: a woman with close-cropped silver hair, a dark green field jacket, mid-30s,
> looking off toward the streetlight.**

It folds your description in **verbatim** and anchors the gaze into the scene (toward the
streetlight, not "camera-left"). The appearance is entirely yours; the engine supplies none of it.

---

## 3. Edit an existing image without breaking the rest

Have an image already? Paste or attach it and name the one thing to change, e.g.:

> **Here's my cover. Change the jacket to burnt orange — keep everything else identical.**

Visual Director treats this as an **edit**, not a new generation: it identifies what must stay
(the immutables) and changes only the target. Because ChatGPT / GPT Image re-renders the whole
frame, the edit block restates the immutables and adds an explicit "keep everything else exactly as
in the source image, unchanged" — that phrasing is what stops the identity, background and colour
from drifting.

- On **Lite**, you get preservation **awareness**: it flags what's mutable vs immutable and warns you
  where an edit may cause collateral change.
- On **Pro**, you get the **full preservation system** — constraint extraction plus
  identity/lighting/geometry/background/composition continuity — and the edit-state ledger below.

---

## 4. Iterate as a delta (Pro)

Your next round of feedback is treated as a *delta* on the last result, not a fresh regeneration.
After the edit above:

> **Bit too bright — knock the orange back one stop, nothing else changes.**

Pro's edit-state ledger tracks ORIGINAL → EDIT → feedback, so it adjusts only what you named and
carries the rest forward unchanged, instead of starting over and losing ground each time.

---

## 5. Build a consistent series (Pro)

A series is several covers that read as one set. Consistency comes from a **profile you author once**
(under the bundle's `examples/`), never from a built-in character. You write the recurring subject
and its appearance; the engine copies those locked axes into every cover **verbatim** and varies
only what each new ask names (time, setting, what the subject is doing).

> **Third cover in my series — same character as before, this one at a rainy bus stop at dawn.**

The engine's job here is *mechanical*: copy the locked axes unchanged, vary only the named ones — so
the subject stays exactly the one you authored across the whole set.

---

## Where to go next

- Using a renderer other than ChatGPT? Read [renderer-guide.md](renderer-guide.md) first — the other
  four are **Starter** profiles.
- Something not behaving? [troubleshooting.md](troubleshooting.md).
- Want the full editing/iteration method (precise edits, an edit-state ledger, deep diagnosis)? That's
  **Pro** — [meridianadmin8.gumroad.com/l/visual-director-pro](https://meridianadmin8.gumroad.com/l/visual-director-pro?utm_source=lite-readme).
