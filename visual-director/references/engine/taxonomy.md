# Universal specificity taxonomy

Loaded at Pipeline step 4 (direction assembly). This is the **published interface** every
preset extends — the fixed set of named axes a direction is specified against. Vagueness is the
number-one cause of wrong renders and back-and-forth; a direction is "specified" when every
*applicable* axis below is either filled concretely or explicitly marked N/A.

**This file is medium- and renderer-agnostic on purpose.** The axes describe *what* to pin down,
never *how a particular model phrases it*. No renderer name, phrasing fix, or model quirk belongs
here — those live only in `adapters/` (D-002). No built-in human/appearance content belongs here —
subjects are user-authored only (D-001); see `engine/safety.md`.

## Two rules that run through every axis

1. **Medium first (AX-0).** Resolve the medium/rendering approach *before* reading the other
   axes, because it changes what each axis means. In a photograph AX-4 is a literal camera; in a
   flat illustration it is a drawn vantage/projection. Never default to a photographic idiom
   ("cinematic photo", "film stock", "shot on…") — that is a choice on AX-0, not a baseline.
2. **Anchor abstract → concrete in-scene reference.** Whenever an axis wants a *direction* or a
   *relation* (where something looks, where light comes from, where the vantage sits), do not
   state it in absolute or abstract terms — anchor it to something visible in the scene. Renderers
   read "to the left" / "front angle" / "moody light" inconsistently; they read "toward the open
   door", "from the window behind the subject", "vantage low and facing the stairs" reliably.
   This heuristic is repeated per-axis below as **Anchor:**.

## How presets consume this (interface contract)

A preset (cover / thumbnail / illustration, and the three VD-PACK bundles) **extends** these axes;
it does not replace them. A preset MAY:
- set sensible **defaults** or a **priority order** for these axes (e.g. a cover foregrounds
  AX-1/AX-5/AX-9; a thumbnail foregrounds AX-9 legibility);
- add **preset-local sub-axes** layered on top (e.g. a thumbnail's headline/safe-margin), namespaced
  to the preset so they never collide with the axis IDs here.

A preset MUST NOT: rename, drop, or renumber the axes below; hardcode a medium (that is AX-0, the
user's call); inject a renderer quirk (D-002) or a human/appearance default (D-001). The axis IDs
(`AX-0`…`AX-9`) are the stable contract — reference them, don't fork them.

---

## AX-0 — Medium & rendering approach

The frame for everything else. Fix the depiction *kind* and its stylistic register explicitly.
- **Kind**: e.g. photograph, painterly/painted, flat/vector illustration, line art, 3D render,
  collage, mixed. State it; do not assume.
- **Register / finish**: level of realism vs stylisation, texture/grain vs clean flats, rendering
  fidelity. For flat work this is where "flat colour fills, minimal shading, bold outlines" lives.
- **Consequence for later axes**: AX-4 becomes a drawn vantage/projection, AX-5 may be depicted
  light *or* stylised/absent shading, AX-8 may be detail-hierarchy rather than optical blur.

*Fill even when "obvious".* An unstated medium is where the engine silently defaults to photo —
the seed's EVAL-012 failure. A flat-illustration ask is fully directable through AX-1…AX-9 once
AX-0 says so.

## AX-1 — Subject

What the image is *of*, at the level of identity and salient attributes — not appearance defaults.
- Subject may be a person, creature, object, product, place, typographic/graphic element, or an
  abstract form. Name it and its state (what it *is* and what, if anything, it is doing).
- **Appearance is user-authored only.** Do not supply ethnicity, body, face, age, or an
  "attractive/idealised" look the user did not give (D-001). If the request implies a human and
  gives no appearance, stop and ask — do not invent one (→ `engine/safety.md`).
- Recurring/defined subjects come from user-authored profiles (`examples/`), never from a built-in.

## AX-2 — Pose & body language

The subject's posture, orientation, and the effort/energy it reads as.
- For a figure: stance, what limbs/hands are doing, tension vs. ease (relaxed/heavy vs. alert/upright).
- For an object/product: orientation, how it sits or is held, whether upright/tilted/in-use.
- For abstract/graphic forms: arrangement, balance, implied motion.
- **Anchor:** relate posture to the scene, not to the frame — "leaning back against the railing",
  not "leaning left". Mark N/A only when the subject has no meaningful orientation.

## AX-3 — Gaze & attention

Where the subject's attention is directed — the single most mis-rendered relation.
- Applies to anything with an implied "front" or focus: a face/eyes, an animal, even a product's
  hero face or a form's implied thrust.
- **Anchor (mandatory):** never absolute left/right and never "to the side" — anchor to an
  in-scene reference: "looking away from the open door", "gaze toward the light source",
  "facing the horizon behind them". Models read absolute direction inconsistently; an in-scene
  anchor is stable.
- Also state the gaze's relation to the viewer/camera: toward it, past it, or away.
- Mark N/A for subjects with no attention (pure still life, non-representational pattern).

## AX-4 — Vantage & camera position

Where the scene is viewed *from*, stated physically — not as an abstract label.
- **Photographic media:** where the camera physically sits and what it faces — "camera low and
  near the front fender, facing back toward the driver", not "front angle". (Abstract angle words
  like "front/side/hero" are the seed's single biggest recurring failure — replace them.)
- **Non-camera media (flat/vector/illustration, 3D):** the drawn vantage or projection — front-on
  elevation, three-quarter, top-down/plan, isometric, worm's-eye. Same requirement: state it
  concretely.
- **Anchor:** locate the vantage relative to the subject and setting ("above and behind, looking
  down the stairwell"), not relative to the frame edges.

## AX-5 — Lighting

How the scene is lit or how light/shade is depicted.
- State **source**, **direction**, and **colour** explicitly — "warm low sun raking across the
  subject", not "moody lighting".
- **Medium-dependent (per AX-0):** in photo/painterly/3D this is modelled light. In flat/vector
  work it may be stylised — flat fills with no modelled light, a single hard shadow shape, or a
  limited palette standing in for time/mood. State which; "no modelled light, flat fills" is a
  valid, complete answer for flat work.
- **Anchor:** give direction by an in-scene source or landmark — "lit from the window behind them",
  "glow rising from the object itself" — not "from the left".

## AX-6 — Time & temporal setting

The temporal condition, stated explicitly even when other details imply it.
- Photographic/representational: time of day, season, weather, era. Renderers drift toward
  dusk/blue-hour unless told — so say "flat midday overcast" or "full dark, no ambient sky" outright.
- Stylised/flat/abstract: the temporal *cue* the palette or motif should carry (a warm daytime
  palette, a night motif), even if not literal light.
- Mark N/A only for genuinely timeless/abstract pieces where no temporal reading is wanted.

## AX-7 — Background & setting

What surrounds the subject and how much it competes for attention.
- State **what is actually there** (not "blurred background"), its **density/variety**, and how much
  it should **compete with the subject** — empty/negative-space vs. dense/busy, and any specific
  environment, architecture, or motif.
- Medium reading: a photographic environment, a painted setting, or — for flat work — the field
  treatment: solid colour field, simple geometric backdrop, patterned plane.
- **Anchor:** place background elements relative to the subject ("shelves receding behind the
  figure's right shoulder"), not by frame position.

## AX-8 — Focus & depth

What is sharp/emphasised versus soft/recessive, and how depth is conveyed.
- **Optical media (photo/3D):** depth of field — what plane is in focus, what falls off, how shallow.
- **Flat/graphic media:** there may be no optical blur; convey emphasis instead through **detail
  hierarchy** (more detail on the hero, flatter on the rest), overlap, scale, or layered flat
  planes. "No depth-of-field; flat planes, hero most detailed" is a complete answer.
- Purpose is the same across media: direct the eye to the subject.

## AX-9 — Format & framing

The output envelope.
- **Aspect ratio / dimensions**, **crop tightness** (how much of the subject and headroom), and
  **edge treatment** — full-bleed vs. intended margin.
- **Borders/frames:** state the intent explicitly. Many renderers add unwanted frames/margins by
  default, so "no border, no frame, edge-to-edge full bleed" is the usual instruction — but it is a
  *default to state*, never a renderer-specific fix (any model-specific border quirk lives in
  `adapters/`).
- Any in-frame reserved space (e.g. clear area for later text) is a preset-local concern layered
  on this axis, not a change to it.

---

## Dry-runs (medium-agnostic proof)

Both use the same axis set; only the *fills* differ. This is the check that the taxonomy is not
photo-only and not cover-only.

**EVAL-002 — expert, photographic generation** (dense expert input; must not override the user):
- AX-0 photograph, high realism, fine grain · AX-1 user's stated subject/attributes verbatim (no
  added appearance) · AX-2 posture as given · AX-3 gaze anchored to the stated in-scene reference ·
  AX-4 the user's exact camera placement kept as-is (physical, already concrete) · AX-5 the user's
  named source/direction/colour · AX-6 the stated time · AX-7 stated environment/density · AX-8 the
  user's focus/DoF terms · AX-9 stated aspect/crop, borders per intent. Where the expert already
  pinned an axis, the taxonomy's job is to *carry it unchanged*, not re-specify it (guards HALLU/VERB).

**EVAL-012 — flat / non-photographic illustration** (must be directable without a photo idiom):
- AX-0 **flat vector illustration, bold outlines, flat colour fills, minimal shading** — this one
  axis removes the photo idiom · AX-1 subject as an illustrated form · AX-2 pose/arrangement ·
  AX-3 attention anchored in-scene (if any) · AX-4 **front-on elevation / isometric** drawn vantage
  (no "camera") · AX-5 **no modelled light, flat fills** (or a single stylised shadow) · AX-6 palette
  carries the temporal cue · AX-7 **solid/simple colour field**, low competition · AX-8 **no DoF;
  detail hierarchy** — hero most detailed · AX-9 aspect/crop, full-bleed, no frame. Nothing here
  reaches for "cinematic photo" or "35mm"; the flat ask is fully specified.
