# Troubleshooting

Most issues are either the skill not being loaded, or a renderer behaving as documented. Work top to
bottom.

## Install / loading

**Claude doesn't seem to use the skill.**
- Confirm the folder sits directly under a skills directory — `~/.claude/skills/<bundle>/SKILL.md`
  (personal) or `<project>/.claude/skills/<bundle>/SKILL.md` (project-scoped). The `SKILL.md` must be
  at the top of the bundle folder, not nested deeper.
- Reopen / restart Claude after copying the folder — skills are discovered at startup.
- Ask directly: *"Do you have a visual-director skill available?"* If it says no, the path is wrong
  or Claude wasn't reopened.

**Nothing to install for renderers?**
- Correct. The bundle is plain text with no API, keys, or services. If you're looking for an API key
  step, there isn't one — you paste the block into the generator yourself.

## Output shape

**The direction came split across prose instead of one block.**
- The contract is always a single delimited, ready-to-paste block, with notes *before* it. Ask for
  "the block only" and copy everything inside the fence.

**It asked me to describe a person before continuing.**
- Working as intended. Visual Director ships **no** built-in human appearance and won't invent one.
  Describe the subject yourself (hair, clothing, age, etc.) and it folds your words in verbatim.

**It proposed a medium ("a photograph") I didn't ask for.**
- That's a *stated, overridable* default, not a silent assumption — reply with one word
  ("illustration", "painting") to switch. It won't bake a medium in.

## Render results

**My night scene renders as dusk / blue hour.**
- Keep the "full dark, no ambient sky" wording in the block. That phrasing exists specifically to
  stop renderers drifting to blue hour; don't soften it.

**An edit changed things I wanted kept (face, background, colour).**
- Make sure the immutables are restated in the block and it ends with "keep everything else exactly
  as in the source image, unchanged" — ChatGPT / GPT Image re-renders the whole frame, so the
  preservation has to be spelled out. Pro's full preservation system and edit-state ledger are built
  for exactly this; Lite gives awareness/warnings but not the full lock.

**A border or frame appeared around the image.**
- State the full-bleed intent positively ("fills the entire frame, edge-to-edge full bleed") rather
  than "no border" — negation is followed weakly. Pro includes artifact-repair guidance for cleaning
  up a frame that slipped through.

## Renderers

**I named Midjourney / Gemini / Ideogram / FLUX and results are uneven.**
- Those ship as **Starter** profiles (Pro only): documented head starts that **improve with use**,
  not verified-tested behaviour. Only **ChatGPT / GPT Image** is **Tested**. For the most reliable
  results, use the ChatGPT default. See [renderer-guide.md](renderer-guide.md).

**Can I use a renderer that isn't listed?**
- Only the five in the renderer guide have profiles. You can still paste a block into another
  generator, but there's no profile tuning its phrasing, so treat the result as unsupported.

## Scope

**I want product / e-commerce / packshot direction.**
- Visual Director has **no product-photography preset**. Product and e-commerce shots are handled by
  a separate sibling product, **Product Shot Director** — not by Visual Director. Don't expect a
  product preset here.

**Still stuck?**
- Re-read [quickstart.md](quickstart.md) to confirm the loop (Claude writes direction → you paste
  into a generator), and check the engine itself in this skill's `SKILL.md`.
