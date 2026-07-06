# software-design-philosophy

[![skills.sh](https://skills.sh/b/B87/software-design-skill)](https://skills.sh/B87/software-design-skill)

An [Agent Skill](https://agentskills.io) that gives AI coding agents a vocabulary and
diagnostic lens for software design quality — distilled from John Ousterhout's
*A Philosophy of Software Design*. Use it to sharpen code reviews, architecture
discussions, module/interface decisions, and error-handling design by naming problems
precisely (shallow module, information leakage, pass-through method, temporal
decomposition…) instead of leaving feedback at "this feels off."


## What's inside

```text
SKILL.md                    # core concepts + red-flags quick-reference table
references/examples.md      # worked examples from the book (loaded on demand)
```

`SKILL.md` covers complexity symptoms and root causes, strategic vs. tactical
programming, deep vs. shallow modules, information hiding, general- vs.
special-purpose interfaces, layering, pulling complexity downward, defining errors out
of existence, "design it twice," comments, naming, consistency, and code obviousness.

## Install (recommended)

Use the [skills.sh](https://skills.sh) CLI to install into your coding agent(s).

**Project scope** (committed with your repo, shared with the team):

```bash
npx skills add B87/software-design-skill -y
```

**Global scope** (available in every project on your machine):

```bash
npx skills add B87/software-design-skill -g -y
```

**Target a specific agent** (when you have several installed):

```bash
npx skills add B87/software-design-skill -a cursor -y
npx skills add B87/software-design-skill -a claude-code -g -y
```

The CLI auto-detects supported agents, symlinks (or copies) the skill into each
agent's skills directory, and wires up `references/` alongside `SKILL.md`. No build
step — restart your agent or start a new session if it doesn't pick up the skill
immediately.

Browse and search the ecosystem at [skills.sh](https://skills.sh), or run:

```bash
npx skills find software design
```

## Update

Check whether installed skills have newer versions on GitHub:

```bash
npx skills check
```

Update to the latest version:

```bash
# All installed skills
npx skills update -y

# This skill only (name from SKILL.md frontmatter)
npx skills update software-design-philosophy -y

# Global installs only
npx skills update -g -y
```

List what you have installed:

```bash
npx skills list
```

Remove if needed:

```bash
npx skills remove software-design-philosophy
```

## Manual install

If you prefer not to use the CLI, clone or copy this repository into your agent's
skills folder. The directory name should match the skill `name` in `SKILL.md`
(`software-design-philosophy`):

```bash
git clone https://github.com/B87/software-design-skill.git \
  ~/.cursor/skills/software-design-philosophy   # Cursor (global example)
```

For agents without a skills folder, paste `SKILL.md` into project instructions or a
system prompt, and point the agent at `references/examples.md` when you want worked
examples pulled in.

## Credit

All ideas here are John Ousterhout's, from *A Philosophy of Software Design*. This repo
is an unofficial, community-made distillation meant as a companion for AI agents — not
a replacement for reading the book. Actually, you should totally read the book!

## License

[MIT](./LICENSE)
