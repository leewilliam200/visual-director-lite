# Cover / series-art preset

Loaded at **Pipeline step 4** (direction assembly) for router shape **A** when the ask is a
cover / album / playlist / single artwork — and, when the ask names a recurring subject, alongside
`examples/`. **Ships in the free Lite bundle (D-004); it is the Lite hook.** It generalises the
seed's cover skill into a first-class preset.

This preset is an **extension of the universal taxonomy (`engine/taxonomy.md`), not a fork.** It
sets cover-sensible defaults and a priority order over `AX-0…AX-9`, and layers a few namespaced
`COVER.*` sub-axes on top. It uses the same axis IDs, in the same order, with the same meanings.

**MUST NOT (the preset's own guardrails, mirroring the interface contract):**
- **Never rename, drop, or renumber `AX-0…AX-9`.** Reference them; don't re-invent a rival scheme.
- **Never hardcode the medium.** `AX-0` stays the user's overridable choice. Do **not** bake in
  "cinematic photo", "35mm", "film stock", or any photo idiom — that was the seed's EVAL-012 sin.
  A cover is just as valid as a flat illustration, a painting, or a 3D render.
- **Never inject a renderer quirk (D-002).** Model-specific phrasing or artifact fixes (e.g. the
  ChatGPT white-border crop) live only in `adapters/`; fold them in at output, never here.
- **Never ship a human/appearance default (D-001).** No built-in subject. The seed's "Night drive
  girl" character does **not** carry over. If the ask implies a person and gives no appearance,
  stop and ask (→ `engine/safety.md`); do not invent one.
- **Series consistency comes from user-authored profiles (`examples/`), never a built-in subject.**

---

## 1. Priority order (what a cover foregrounds)

A cover lives or dies on three axes; specify them first and hardest, then fill the rest.

1. **`AX-1` Subject** — a cover reads as *one* thing at a glance. Pin the subject and its state
   before anything else. Appearance is user-authored only (D-001).
2. **`AX-5` Lighting / mood** — the cover's emotional register is carried mostly by light and
   palette. "chill", "warm", "moody", "hazy" resolve here (medium-dependent per `AX-0`).
3. **`AX-9` Format & framing** — the fixed envelope. A cover is a **square by default** (see §2),
   full-bleed, and usually reserves clear space for a title/wordmark (→ `COVER.text-safe`, §3).

The remaining axes (`AX-2, 3, 4, 6, 7, 8`) still apply and are filled to "specified", but they
serve the three above rather than leading. `AX-0` is resolved **first of all** (taxonomy rule 1)
because it reframes every other axis — but it is a *choice to surface*, not a default to bake.

## 2. Cover-sensible axis defaults

Defaults the preset **proposes and states** (EFFORT: state defaults out loud, one word to
override). A default is a starting point the user overrides in a word — never a silent assumption,
and never a substitute for the user's own fill.

| Axis | Cover default | Note |
|---|---|---|
| **AX-0** Medium | **none — surface it as an explicit, overridable choice** | Never inherit a photo idiom. If the ask names no medium, propose one *per request* and say it's overridable ("assuming a soft nighttime photo — say 'illustration'/'painting' to switch"); never write a medium into this preset. |
| **AX-1** Subject | none (user-authored) | Foreground it. No appearance/human default (D-001); recurring subjects come from `examples/`. |
| **AX-2** Pose/arrangement | subject-appropriate; **Anchor** to the scene | For an atmospheric cover with no figure, this is composition/arrangement of forms, not a stance. |
| **AX-3** Gaze/attention | N/A unless a figure/face is present | If present, **Anchor** to an in-scene reference, never absolute left/right (taxonomy AX-3). |
| **AX-4** Vantage | mid-distance, subject-filling | State physically (photo) or as a drawn vantage (flat/3D) per `AX-0`. Not "hero angle". |
| **AX-5** Lighting/mood | **carry the stated mood word here**; state source + direction + colour | Foregrounded. Say "warm low streetlight raking across the subject", not "moody". Flat media: stylised fills / limited palette per `AX-0`. |
| **AX-6** Time | **state it explicitly** ("full dark, no ambient sky") | Renderers drift to dusk/blue-hour; a "night" cover must say full dark outright (seed lesson). |
| **AX-7** Background | low competition; **Anchor** density to the subject | State what's actually there and cap its business so it never fights the subject or the text zone. |
| **AX-8** Focus/depth | hero reads first | Optical DoF (photo/3D) *or* detail hierarchy (flat) per `AX-0`; both are valid complete answers. |
| **AX-9** Format | **square 1:1, full-bleed, no border / no frame** | The cover default. State non-square only if the platform needs it (e.g. 3:4). Reserve text space via `COVER.text-safe` (§3), not by shrinking the art. |

## 3. Preset-local sub-axes (`COVER.*`)

Namespaced so they never collide with `AX-*`. Each layers on an existing axis; none replaces one.

- **`COVER.text-safe`** — reserved clear area for a later title / artist / wordmark overlay. Layers
  on **AX-9** (taxonomy: in-frame reserved space is a preset concern on AX-9, not a change to it).
  State *where* and *how much*: e.g. "keep the lower third low-contrast and uncluttered for a title;
  do not render any text." Default: **render no text** unless the user asks for lettering.
- **`COVER.series-key`** — the consistency contract for a multi-cover series (§4). Names which axes
  are **locked** (identical across every cover) versus **varied** (change per track/episode). Empty
  for a one-off cover.
- **`COVER.focal-anchor`** — where the single focal point sits, **Anchored** to the frame's reserved
  zones ("focal weight upper-centre, above the reserved title band"), so subject and text never
  collide. Layers on AX-2/AX-8.

---

## 4. Series consistency (user-authored, never built-in)

A series is *N* covers that read as one set. Consistency is **not** achieved by a built-in subject
(D-001) — it comes from a **user-authored profile in `examples/`** (VD-PACK-004) that the user
writes once and the engine reuses verbatim.

- The profile supplies the **locked** axes — typically `AX-0` (medium/finish), `AX-1` (the recurring
  subject *and its user-given appearance*), and often `AX-5`/palette. These are copied **verbatim**
  into every cover in the series; the engine does not re-specify or "improve" them (guards against
  identity drift and against inventing appearance).
- The user's per-cover ask supplies the **varied** axes — usually `AX-6` (time), `AX-7` (setting),
  `AX-2`/`AX-3` (what the subject is doing this time). `COVER.series-key` records the lock/vary split.
- If a series names a person and no profile yet exists, the engine does **not** invent one — it asks
  the user to author the profile first (→ `engine/safety.md`, D-001). No appearance is ever supplied
  by this preset.

The engine's contribution to consistency is *mechanical* (copy the locked axes unchanged, vary only
the named ones), not *creative* — exactly so the subject stays the user's, across the whole set.

---

## 5. Effort & output (Lite behaviour)

- **Vague beginner ask** (EVAL-001 family): stay in tier — do **not** balloon a one-line request.
  Fill the key axes with the stated defaults above, ask **at most one or two** questions (and only
  where a default can't stand — e.g. is there a person?), and emit one short block. Never invent a
  human to fill AX-1.
- **Output contract (VD-FR-009):** any notes/questions go *before* the block; the block itself is
  one delimited, ready-to-paste prompt containing everything needed to reproduce the cover. If the
  renderer is unstated it targets ChatGPT with a one-line note (→ `adapters/chatgpt.md` folds in at
  output only).

---

## 6. Dry-runs

Both use the same `AX-0…AX-9` set — proof the preset extends the taxonomy rather than forking it.

### EVAL-001 — vague "chill night cover" (the Lite hook)

*Ask:* "Make a chill playlist cover, night vibe." (beginner, vague, no subject named.)

Engine behaviour — states defaults, asks little, invents no human:
- **AX-0** none baked → *proposes* "a soft, moody nighttime photograph" **and says it's overridable**
  (one word switches to illustration/painting). · **AX-1** no person named → keep it atmospheric (a
  quiet night scene), **do not invent a figure** (D-001); one optional question: "want a person on
  it? if so, describe them." · **AX-2** N/A (no figure) · **AX-3** N/A · **AX-4** mid-distance,
  looking across the scene · **AX-5** *(foreground)* warm streetlight glow low in frame against cool
  dark air · **AX-6** full dark, no ambient dusk sky (stated outright) · **AX-7** sparse, low
  competition — a few scattered warm-lit windows, not a dense skyline · **AX-8** hero plane soft-lit,
  rest recessive · **AX-9** *(foreground)* square 1:1, full-bleed, no border/frame · **COVER.text-safe**
  lower third kept low-contrast for a title; no text rendered.

Questions (before the block): *Medium: assuming a moody night photo — reply "illustration" or
"painting" to switch. Want a person on the cover? If so, describe them (I won't invent one).*

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

### EVAL-003 — user-authored recurring subject across a series

*Ask:* "Third cover in my series — same character as before, this one at a rainy bus stop at dawn."
A user profile already exists at `examples/<subject>.md` (author-written; supplies the subject and
its appearance).

Engine behaviour — copies the locked axes from the profile verbatim, varies only what the ask names:
- **`COVER.series-key`** locked = {AX-0 medium/finish, AX-1 subject + appearance, AX-5 signature
  palette}; varied = {AX-6 time, AX-7 setting, AX-2/AX-3 what she's doing}.
- **AX-0** *from profile, verbatim* (e.g. the user's stated medium/finish) — **not** re-chosen here ·
  **AX-1** *from profile, verbatim* — the recurring subject and the appearance **the user authored**;
  the preset supplies **no** appearance of its own (D-001) · **AX-2** leaning under the shelter, this
  cover's action · **AX-3** *(Anchor)* gaze out past the shelter toward the empty road, away from
  viewer · **AX-4** mid-distance across the pavement · **AX-5** *from profile palette* + this cover's
  cool wet dawn light · **AX-6** *(varied)* first grey dawn, overcast, raining · **AX-7** *(varied)* a
  bus-stop shelter, wet pavement, low competition behind the figure's shoulder · **AX-8** figure reads
  first, rain and street recessive · **AX-9** square 1:1, full-bleed, no border · **COVER.text-safe**
  upper band reserved for the series wordmark, matching covers 1–2; no text rendered.

Note (before the block): *Subject, medium and palette copied unchanged from `examples/<subject>.md`
to keep the series consistent; only time, setting and action change for cover 3. No appearance was
added by me — it all comes from your profile.*

```
[AX-0, AX-1 and the signature palette are inserted verbatim from examples/<subject>.md —
the user-authored medium, recurring subject, and appearance. No appearance is supplied
by this preset.]

...the recurring subject, standing under a bus-stop shelter at first grey dawn, leaning
against the shelter's post, gazing out past it toward the empty wet road, away from the
viewer. Overcast dawn light, fine rain, cool and quiet; the series' signature palette
carried through. Wet pavement and the shelter frame recede behind her shoulder, low
competition so she reads first.

Keep an uncluttered upper band for the series wordmark (matching covers 1 and 2); render
no text. Square 1:1 aspect ratio, full bleed, no border, no frame.

(Renderer unspecified → targeting ChatGPT. Locked axes copied unchanged from the user's
profile; only time, setting and action vary for this cover.)
```
