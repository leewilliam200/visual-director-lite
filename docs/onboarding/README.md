# Visual Director — Onboarding

Everything a new user needs to install Visual Director and produce their first direction block.
Start at the top and go down; each guide stands on its own.

1. **[install.md](install.md)** — get the skill into Claude, per environment. No API keys, no build.
2. **[quickstart.md](quickstart.md)** — produce your first ready-to-paste block, then edit and
   iterate. Runnable end-to-end with no prior context.
3. **[renderer-guide.md](renderer-guide.md)** — the five supported renderers, what **Tested** vs
   **Starter** means, and how the plain-text profiles work.
4. **[troubleshooting.md](troubleshooting.md)** — common snags and how to clear them.

## The one thing to understand first

Visual Director **writes direction; it does not render images.** The loop is:

```
You describe what you want  →  Claude (with Visual Director) hands you ONE ready-to-paste block
                            →  You paste that block into an image generator (ChatGPT / GPT Image
                               by default)  →  the generator makes the picture.
```

Visual Director runs inside Claude as a plain-text skill. The picture is made by whichever image
generator you paste the block into. Keep that split in mind and the rest follows.

## Reminders that hold across every guide

- **You author the subject.** Visual Director ships **no** built-in human appearance and will never
  invent one. If an ask implies a person and you gave no description, it stops and asks you to
  describe them. Any person in these guides' examples is one *you* would have described.
- **One block out.** The output is always a single delimited block. Notes and questions come before
  it; the block itself is only what you paste.
- **ChatGPT is the default.** Name no renderer and the block targets ChatGPT / GPT Image, stated in
  one line. The other four renderers are Pro-only **Starter** profiles.
