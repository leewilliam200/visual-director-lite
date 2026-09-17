# Install

Visual Director is a **Claude Skill** — a folder of plain-text files. There is nothing to compile,
no dependencies to fetch, no API keys, and no accounts or services to connect. Installing it means
placing the skill folder where Claude looks for skills, then reopening Claude.

## What you need

- **Claude with Skills support** — e.g. Claude Code, or a Claude app/environment that loads Skills
  from a `.claude/skills/` directory.
- **The bundle folder:**
  - **Lite** (free): the `visual-director` folder.
  - **Pro** (one-time $19 + update access): the `visual-director-pro` folder, purchased from
    [Gumroad](https://meridianadmin8.gumroad.com/l/visual-director-pro) — you download it there.

That's the entire prerequisite list. Because the bundle is plain text with no runtime connection,
you do not install renderer SDKs, set environment variables, or authenticate anything to use it.

## Steps

1. **Locate your Claude skills directory.**
   - Personal, available across all your projects: `~/.claude/skills/`
   - Project-scoped, only inside one repository: `<your-project>/.claude/skills/`

   If the `skills` directory doesn't exist yet, create it.

2. **Copy the bundle folder into it.** The folder must contain `SKILL.md` at its top level. A Pro
   install should look like:

   ```
   ~/.claude/skills/
   └── visual-director-pro/
       ├── SKILL.md
       └── references/
           ├── engine/
           ├── editing/
           ├── critique/
           ├── adapters/
           ├── presets/
           └── examples/
   ```

3. **Reopen / restart Claude** so it discovers the new skill.

4. **Confirm it loaded.** Ask Claude:

   > *Do you have a visual-director skill available?*

   It should acknowledge the skill. If not, see [troubleshooting.md](troubleshooting.md).

## Lite vs Pro at install time

- Install **one** bundle — you don't need both. Lite is the free hook (engine + cover preset +
  ChatGPT profile + basic critique + preservation awareness). Pro is the superset (full
  preservation, edit-state iteration, precise edits, deep diagnosis, thumbnail + illustration
  presets, and the four **Starter** renderer profiles).
- **Pro is a one-time purchase with ongoing update access — not a subscription.** Updates keep the
  renderer profiles current as models change; the core reasoning is built to not decay when they do.

## Uninstall

Delete the skill folder from your `.claude/skills/` directory and reopen Claude. Nothing else is
left behind — there is no state stored outside the folder.

Next: [quickstart.md](quickstart.md).
