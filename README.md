# software-design-philosophy

[![skills.sh](https://skills.sh/b/B87/software-design-skill)](https://skills.sh/B87/software-design-skill)

An [Agent Skill](https://agentskills.io) that gives AI coding agents a vocabulary and
diagnostic lens for software design quality — grounded in John Ousterhout's
*A Philosophy of Software Design*, complemented by Lean decision timing, Real Options,
and YAGNI. Use it to sharpen code reviews, architecture
discussions, module/interface decisions, and error-handling design by naming problems
precisely (shallow module, information leakage, pass-through method, temporal
decomposition…) instead of leaving feedback at "this feels off."


## What's inside

```text
software-design-philosophy/
├── SKILL.md                # core concepts + red-flags quick-reference table
└── references/
    ├── examples.md         # worked examples from the book (loaded on demand)
    └── decision-timing.md  # last responsible moment, Real Options, YAGNI, and sources
```

The skill lives in a subdirectory (not at the repo root) so the [skills CLI](https://skills.sh)
installs `references/` alongside `SKILL.md`. See [vercel-labs/skills#1517](https://github.com/vercel-labs/skills/issues/1517).

`SKILL.md` covers complexity symptoms and root causes, strategic vs. tactical
programming, deep vs. shallow modules, information hiding, general- vs.
special-purpose interfaces, layering, pulling complexity downward, defining errors out
of existence, "design it twice," comments, naming, consistency, and code obviousness.

For consequential choices, it also considers **when to commit**: decide now, make a
reversible choice, or defer while gathering evidence. `references/decision-timing.md`
explains how to identify a commitment trigger, account for delivery lead time, and avoid
speculative flexibility. Its worked scenarios cover all three actions and explain how
decision timing fits with strategic design and general-purpose interfaces.

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

If you prefer not to use the CLI, copy the `software-design-philosophy/` folder (not the
whole repo) into your agent's skills directory:

```bash
git clone https://github.com/B87/software-design-skill.git /tmp/software-design-skill
cp -r /tmp/software-design-skill/software-design-philosophy ~/.cursor/skills/
```

For agents without a skills folder, paste `SKILL.md` into project instructions or a
system prompt, and point the agent at `references/examples.md` when you want worked
examples pulled in.

## Credit

The core design vocabulary and book examples come from John Ousterhout's
*A Philosophy of Software Design*. Decision-timing guidance draws on Mary and Tom
Poppendieck's Lean software development work, Chris Matts and Olav Maassen's Real Options
writing, and Martin Fowler's explanation of YAGNI from Extreme Programming. Jeff Atwood's
[The Last Responsible Moment](https://blog.codinghorror.com/the-last-responsible-moment/)
provides an accessible introduction. See
[Decision timing](software-design-philosophy/references/decision-timing.md#sources) for
sources and the distinction between the authors' concepts and this skill's synthesis.

This repo is an unofficial, community-made companion for AI agents, not a replacement
for the original works. You should totally read the book!

## License

[MIT](./LICENSE)
