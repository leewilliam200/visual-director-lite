# Complexity triage — the effort envelope

**Pipeline step 1**, and the load-router's mandatory first load ("Always load `engine/triage.md`
first", SKILL.md §load-router). Its job: read the opening request and set **how much effort it
gets** — the **EFFORT ENVELOPE** — before any other module loads. It classifies the request into
one of five tiers and, from the tier, fixes three dials: the **question budget**, the **verbosity
(VERB)**, and **how exhaustively the specificity taxonomy is filled**.

**This file sizes effort; it does not route files and it does not phrase for a renderer.**

- **It does not pick which references load.** That is SKILL.md §load-router (shapes A–G), resolved
  independently from the tier. A tier is not a shape: SIMPLE can be a new generation (A), an artifact
  repair (F), or a bare default-note; MODERATE spans generation, critique, iteration, and
  style-match. Triage says *how hard*, the router says *which files* — never the other way round.
- **It names no renderer and folds in no phrasing (D-002).** Effort classification is
  renderer-agnostic: the tier is the same whether the output later targets ChatGPT or any Starter
  profile. Renderer choice and quirks are the adapter's, at output only (`adapters/<renderer>.md`).
- **It consumes no upstream module.** Triage is the first step; its only input is the raw request.
  Everything else in the pipeline reads *its* verdict, not the reverse.

The failure this module exists to prevent is **mis-sized effort**: ballooning a one-line ask into a
questionnaire, padding an expert's precise brief with noise (or overriding it), or under-powering a
preservation edit where collateral change is the costly failure.

---

## 1. The five tiers (one effort axis)

Every opening request gets exactly one tier. Four are **effort tiers** — they size the work of
producing direction. The fifth, **EDGE**, is a **disposition tier**: the request's core is a hard
safety guard, so triage sizes no direction and defers wholly to `engine/safety.md` (router shape E).

| Tier | The request looks like | What the tier means |
|---|---|---|
| **SIMPLE** | short, low-stakes, one deliverable; beginner-vague; a small mechanical fix; a bare default to state | Answer crisply. State labelled defaults, fill the few load-bearing axes, keep it to a line or two. Do **not** balloon it. |
| **MODERATE** | a full new generation, a critique/diagnosis, a bounded iteration, a style-match, or a surfaced conflict | The ordinary full job: sweep every applicable axis, reason where the shape calls for it, proceed on stated assumptions. |
| **COMPLEX** | an expert paragraph with exact, already-pinned terms (camera/lens/lighting, precise craft language) | The information is already present. The effort is **restraint**: carry the user's pins unchanged, fill only genuine gaps, add nothing. |
| **HIGH-PRECISION EDIT** | a source image + "change X, keep the rest"; preservation-heavy; add-an-element-without-disturbing-the-scene | Maximum preservation rigor. The taxonomy becomes a **completeness scan for locks**, not a generation spec; the value is a complete PRESERVE list, not prose. |
| **EDGE** | a request whose disposition is a hard guard — a real, named person's likeness (and equivalents) | **No effort envelope.** Defer to `engine/safety.md`; assemble no direction until the guard resolves the ask into a legitimate one. |

**Tier ≠ safety-gate.** A tier sizes effort; a safety gate (SKILL.md shape E) resolves a gap or
guard, and the two are independent. A SIMPLE ask can still be vague enough to trip safety §1
(EVAL-001), and a MODERATE ask can be the surfaced contradiction (EVAL-008) — they keep their
effort tier while the router gates them through safety first. **EDGE is the one tier that is itself a
deferral**, because a real-person likeness has no direction to size (EVAL-017); it is not the label
for "any safety-gated request".

---

## 2. The effort envelope — three dials

The tier fixes three dials. They are stated in axis-count / question-count / verbosity terms only —
renderer-agnostic, and about *amount*, never about *which files* or *which model*.

| Tier | Question budget (EFFORT) | Verbosity (VERB) | Taxonomy fill (COMP) |
|---|---|---|---|
| **SIMPLE** | 0–2, and prefer 0: state a labelled default the user overrides in a line, don't ask. | Short — a line or two. Never expand a one-liner into paragraphs. | The few **load-bearing** axes concretely; the rest carried as labelled defaults or N/A. Not an exhaustive sweep. |
| **MODERATE** | ≤ ~3 targeted questions, each on a genuinely load-bearing gap; otherwise proceed on stated assumptions. | Proportionate: cover the applicable axes (+ a short diagnose-lite where the shape calls for it). No padding. | **Every applicable axis** filled; inapplicable axes explicitly N/A. The normal full sweep. |
| **COMPLEX** | 0–1: the expert has specified; asking is noise. Ask only on a real, blocking gap. | Match the input's density — **do not pad**, and **do not override** the user's own precise terms (carry them verbatim). | All axes, but most are already user-pinned; triage budgets to **carry them unchanged** and fill only the gaps. |
| **HIGH-PRECISION EDIT** | 0–2: the target is usually named ("only her jacket"). Ask only if the target or intent is ambiguous (then it may be a safety/conflict case). | Precise and structural. Effort goes into lock **completeness**, not prose length. | Every axis considered as a **potential lock** — the scan that makes the PRESERVE list complete — not filled as a fresh generation spec. |
| **EDGE** | n/a — the guard's question is safety's to pose. | n/a — a brief guard/decline before any block (safety §output-discipline). | none — no direction assembled until the ask is resolved. |

Two dials carry the named rubric failures. **VERB-short on SIMPLE** stops the balloon (EVAL-001).
**VERB-no-pad + carry-unchanged on COMPLEX** stops the engine drowning or overriding an expert
(EVAL-002, HALLU). The taxonomy fill supports **COMP** without dictating it — step 4 does the
filling; triage only sets how exhaustive it should be.

---

## 3. Classifying — the decision procedure

Read the opening request and settle the tier in this order; stop at the first that fits.

1. **Real named person's likeness? → EDGE.** A request to depict an identifiable real individual has
   no direction to size — defer to `engine/safety.md` (§4). This is the only tier that is a
   deferral; it is *not* triggered by mere vagueness or contradiction (those keep an effort tier).
2. **Source image present + preservation is the point? → HIGH-PRECISION EDIT.** "Change X, keep the
   rest", a single-element edit, or an added element that must not disturb the locked scene. Collateral
   change is the costly failure, so this tier buys maximum lock rigor — *unless* it is a **follow-up on
   an edit already made this thread**, which is a bounded delta, not a fresh extraction → **MODERATE**
   (see below). A *new* source image is always a fresh HIGH-PRECISION extraction.
3. **Expert, already-pinned, dense generation? → COMPLEX.** Exact craft terms across many axes. The
   tier is about **input density and the restraint it demands**, distinct from HIGH-PRECISION (which
   is about preservation stakes on an *edit*). A dense expert edit can be both — size the preservation
   as HIGH-PRECISION and still carry the expert's pins unchanged.
4. **A full generation / critique / iteration / style-match / surfaced conflict? → MODERATE.** The
   default working tier once EDGE, HIGH-PRECISION, and COMPLEX are ruled out.
5. **Small, low-stakes, one-shot, or a bare mechanical/default answer? → SIMPLE.** Beginner-vague
   one-liners, an artifact-repair fix, an impossible one-liner, a "no renderer named" default.

**Iteration is MODERATE, not HIGH-PRECISION.** A follow-up delta re-checks against an *already
snapshotted* PRESERVE/CHANGE list (`editing/edit-state.md`) rather than doing a fresh full
extraction — the heavy preservation effort was spent on the prior EDIT. The delta is bounded and
incremental, so it sizes MODERATE (EVAL-007). The first edit that *builds* the ledger is
HIGH-PRECISION; the follow-ups that *consume* it are MODERATE.

**When two tiers seem to fit, take the more rigorous** (HIGH-PRECISION over MODERATE, COMPLEX's
restraint over MODERATE's sweep). Under-effort on preservation and over-effort on an expert are both
failures; when unsure, bias toward the tier whose failure is cheaper to correct.

---

## 4. Boundaries (what triage must not do)

- **Never pick a reference file or a router shape.** Emit the tier + envelope; SKILL.md §load-router
  resolves the shape (A–G) and the loads. A tier maps to no single shape.
- **Never name a renderer or fold in phrasing (D-002).** The envelope is model-agnostic; the default
  renderer note and every quirk are the adapter's, at output.
- **Never author subject, appearance, or scene content.** Triage sizes effort; it does not fill a
  gap. A load-bearing human-appearance gap or a contradiction is handed to `engine/safety.md`
  (D-001), not resolved here.
- **Never re-tier mid-pipeline to justify padding.** The tier is set once from the opening request;
  a later delta is re-tiered only because a *new* turn arrives (and iteration stays MODERATE).
- **Consume nothing upstream.** Triage is step 1; if it reads another module's output, the pipeline
  is mis-ordered.

---

## 5. Worked dry-runs — all 17 opening lines (the DoD)

Each line gets a tier and the envelope that tier fixes. Tiers follow the evaluation catalogue's Tier
column (`docs/qa/evaluation-scenarios.md`); the envelope is triage's contribution.

| EVAL | Opening request (paraphrased) | Tier | Effort envelope (Q budget · VERB · axis fill) |
|---|---|---|---|
| 001 | "Make a chill playlist cover, night vibe." (beginner, vague) | **SIMPLE** | 0–2 Qs, **prefer stating defaults** (1:1, night palette — overridable) · VERB **short** · key axes only (COMP). Do not balloon. |
| 002 | Expert paragraph, exact camera/lens/lighting | **COMPLEX** | 0–1 Q · VERB **no pad**, **carry the expert's terms verbatim, don't override** (HALLU) · all axes, most already pinned → fill gaps only. |
| 003 | New generation, cover preset, recurring user-defined character | **MODERATE** | ≤3 Qs · proportionate VERB · full applicable-axis sweep; subject from the user-authored profile, no built-in human (D-001). |
| 004 | Thumbnail: "curiosity without misleading" | **MODERATE** | ≤3 Qs · proportionate VERB · full sweep incl. thumbnail legibility axes + a diagnose-lite on the curiosity/honesty line. |
| 005 | Image + "don't change anything except her jacket" | **HIGH-PRECISION EDIT** | 0–2 Qs (target named) · precise/structural VERB · every axis scanned as a **potential lock** → complete PRESERVE list (PRES/NOCHG). |
| 006 | "Add falling snow" to an existing scene | **HIGH-PRECISION EDIT** | 0–2 Qs · structural VERB · lock-completeness scan incl. the atmospheric-vs-accumulated surface check. |
| 007 | After EDIT-1: "good, but more flakes beside the tree only" | **MODERATE** | ≤3 Qs · tight VERB · **bounded delta** against the open ledger snapshot — not a fresh extraction (iteration sizes MODERATE, not HIGH-PRECISION). |
| 008 | "Keep it identical but make it totally different" | **MODERATE** | tier holds while the router gates it to safety §2 (contradiction). Effort of the *resolved* ask is a moderate edit or generation; no direction until it resolves. |
| 009 | "Remove the reflection the whole shot is about" (impossible) | **SIMPLE** | small ask; router gates to safety §3 (explain trade-off, promise nothing false). No padded direction. |
| 010 | "Something about this composition feels wrong." | **MODERATE** | ≤3 Qs · proportionate VERB · diagnosis (what/why/change/keep) then a full-sweep direction. |
| 011 | "Make this feel more expensive." (vague quality) | **MODERATE** | ≤3 Qs · proportionate VERB · translate vague→concrete across the axes; diagnose-lite. |
| 012 | Illustration / flat (non-photo) request | **MODERATE** | ≤3 Qs · proportionate VERB · full sweep with **AX-0 set to flat** so no photo idiom is forced (ADAPT/COMP). |
| 013 | Product: "new background, product stays identical" | **HIGH-PRECISION EDIT** | 0–2 Qs · structural VERB · lock-completeness scan (identity/logo/shape/colour) → complete PRESERVE list. |
| 014 | Style matching: "match the look of that reference" | **MODERATE** | ≤3 Qs · proportionate VERB · full sweep extracting **style** axes, not the reference's subject. |
| 015 | Artifact repair: render came back with a white border | **SIMPLE** | 0–1 Q · short VERB · minimal — a single mechanical fix; no full sweep. |
| 016 | No renderer named | **SIMPLE** | 0 Qs · short VERB · state the default at output (adapter's fold, MODEL) — triage just sizes it small; it does not name the renderer itself. |
| 017 | Requests a real named person's likeness | **EDGE** | **no envelope** — defer to `engine/safety.md` §4; no direction assembled until the ask is authored legitimately. |

Counts: SIMPLE 001/009/015/016 · MODERATE 003/004/007/008/010/011/012/014 · COMPLEX 002 ·
HIGH-PRECISION 005/006/013 · EDGE 017. Seventeen lines, one tier and one envelope each.

---

## 6. Hard rules

- **Size only.** Emit a tier + envelope; never a file list, a shape, or a renderer (SKILL.md
  §load-router owns routing; `adapters/` own phrasing — D-002).
- **Short stays short.** SIMPLE keeps VERB short and prefers a labelled default over a question — the
  balloon is a failure (EVAL-001).
- **Do not out-talk the expert.** COMPLEX carries the user's precise terms unchanged and adds no
  padding or override (EVAL-002, HALLU/VERB).
- **Preservation is the rigorous tier.** HIGH-PRECISION spends its effort on lock completeness, not
  prose; under-locking is the costly failure the moat exists to prevent.
- **Iteration ≠ fresh edit.** A follow-up on an open ledger is a bounded MODERATE delta, not a
  HIGH-PRECISION re-extraction (EVAL-007).
- **Author nothing to fill a gap.** Load-bearing gaps and guards go to `engine/safety.md`
  (D-001); triage withholds, it does not guess.
