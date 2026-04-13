# TanStack Start RSC Skill

Portable skill for **TanStack Start React Server Components**.

This repo is the **skill folder itself**. That is intentional. It means people can use it however they already work:

- install it with `npx skills`
- symlink or copy it into a local Claude Code or Codex skills directory
- zip it and upload it to Claude
- hand the repo or zip to another agent and tell it to install it

The skill is built for real TanStack Start RSC work, not for reciting docs:

- primitive choice: `renderServerComponent` vs `createCompositeComponent` vs low-level Flight APIs
- cache ownership: Router vs Query vs HTTP/server cache
- refresh wiring: `router.invalidate()` vs Query invalidation vs HTTP revalidation
- Composite Components and slot design
- SSR mode selection, loader boundaries, and stale API cleanup

---

## The fast answer

### Do I need to connect this repo to skills.sh?

No.

If this repo is on GitHub, people can install it directly with `npx skills add <owner>/<repo>`. The Skills CLI accepts GitHub shorthand, full GitHub URLs, direct paths inside repos, and local paths. `skills.sh` is the public discovery directory for the ecosystem, but it is **not required** for installation.

### Does this repo already work with `npx skills`?

Yes.

The CLI looks for valid `SKILL.md` files in the repo root as well as common skill directories. This repo keeps `SKILL.md` at the root, so `npx skills add CodingCossack/tanstack-start-rsc-skill` is enough once the repo is published.

---

## Pick the install path that matches how you work

Important distinction:

- `global` means global for a specific agent, such as Claude Code or Codex
- there is no single universal skill directory shared by every harness
- if you want one command that installs globally across multiple supported agents, use `npx skills` and target those agents explicitly

### 1. I want the easiest install and easiest updates

Use the Skills CLI.

From GitHub:

```bash
npx skills add CodingCossack/tanstack-start-rsc-skill -g -a claude-code -a codex -y
```

Useful variants:

```bash
# See what the repo contains before installing
npx skills add CodingCossack/tanstack-start-rsc-skill --list

# Install from a full GitHub URL instead of owner/repo shorthand
npx skills add https://github.com/CodingCossack/tanstack-start-rsc-skill

# Install from a local folder instead of GitHub
npx skills add ./tanstack-start-rsc
```

Update later:

```bash
npx skills update tanstack-start-rsc -g
```

Why use this path:

- single command
- agent-aware placement
- easy updates later
- works across Claude Code and Codex

The Skills CLI supports both Claude Code and Codex as install targets and supports project or global installs, symlink or copy, listing available skills, and updating installed skills.

If you want cross-agent global install in one command, use multiple `-a` flags:

```bash
npx skills add CodingCossack/tanstack-start-rsc-skill -g -a claude-code -a codex -a cursor -a opencode -y
```

### 2. I want to install it manually into a local project or global skills folder

Use the raw repo folder directly.

#### Codex - global

```bash
mkdir -p "$HOME/.codex/skills"
ln -s "$(pwd)" "$HOME/.codex/skills/tanstack-start-rsc"
```

#### Codex - project-scoped

```bash
mkdir -p /path/to/project/.agents/skills
ln -s "$(pwd)" /path/to/project/.agents/skills/tanstack-start-rsc
```

#### Claude Code - global

```bash
mkdir -p "$HOME/.claude/skills"
ln -s "$(pwd)" "$HOME/.claude/skills/tanstack-start-rsc"
```

#### Claude Code - project-scoped

```bash
mkdir -p /path/to/project/.claude/skills
ln -s "$(pwd)" /path/to/project/.claude/skills/tanstack-start-rsc
```

If you prefer copies instead of symlinks:

```bash
mkdir -p "$HOME/.codex/skills/tanstack-start-rsc"
rsync -a --delete --exclude '.git' --exclude 'dist' ./ "$HOME/.codex/skills/tanstack-start-rsc/"
```

```bash
mkdir -p "$HOME/.claude/skills/tanstack-start-rsc"
rsync -a --delete --exclude '.git' --exclude 'dist' ./ "$HOME/.claude/skills/tanstack-start-rsc/"
```

Official locations:

- Claude Code personal skills live in `~/.claude/skills/<skill-name>/SKILL.md`; project skills live in `.claude/skills/<skill-name>/SKILL.md`. Claude can invoke a skill directly with `/skill-name` or load it automatically when relevant.
- The Skills CLI currently maps Codex project installs to `.agents/skills/` and Codex global installs to `~/.codex/skills/`.

### 3. I want a zip I can upload or hand around

Build a clean zip from the repo root:

```bash
./scripts/build-zip.sh
```

Output:

```text
dist/tanstack-start-rsc-skill.zip
```

This zip is for humans and agents. It includes the actual skill content and excludes repo noise.

#### Claude web upload

Claude's custom skills flow accepts a ZIP containing the skill folder. The ZIP should contain the skill folder as its root, not loose files at the root.

#### Manual unzip install

```bash
unzip dist/tanstack-start-rsc-skill.zip -d "$HOME/.claude/skills"
# or
unzip dist/tanstack-start-rsc-skill.zip -d "$HOME/.codex/skills"
```

The only invariant that matters is the final path:

```text
$HOME/.claude/skills/tanstack-start-rsc/SKILL.md
# or
$HOME/.codex/skills/tanstack-start-rsc/SKILL.md
```

### 4. I want Codex or Claude to install it for me

That is valid too.

Minimal prompt:

```text
Install this TanStack Start RSC skill. Keep the folder name tanstack-start-rsc and make sure the final path ends in SKILL.md. For Codex use $HOME/.codex/skills/tanstack-start-rsc. For Claude Code use $HOME/.claude/skills/tanstack-start-rsc.
```

---

## Verify it loaded

### Claude Code

Direct invocation:

```text
/tanstack-start-rsc
```

Claude also sees the skill description up front and can load the full skill automatically when the task matches.

### Codex

Codex can load the skill explicitly or automatically depending on the client you use. A good direct prompt is:

```text
Use the tanstack-start-rsc skill. First classify this task as renderServerComponent vs createCompositeComponent vs low-level Flight API. Then identify the real cache owner and refresh owner. Then implement or refactor the smallest correct solution.
```

---

## What this skill actually helps with

Use it when you are working on TanStack Start code that involves:

- `@tanstack/react-start/rsc`
- `renderServerComponent`
- `createCompositeComponent`
- `<CompositeComponent />`
- `renderToReadableStream`, `createFromReadableStream`, `createFromFetch`
- TanStack Query plus RSC caching
- TanStack Router loaders, `loaderDeps`, `staleTime`, `ssr`
- stale RSC data, wrong invalidation, missing refresh wiring
- slot composition, render props, component slots
- migration away from stale names or outdated examples

It is opinionated about the mistakes that actually waste time:

- treating a loader as server-only
- putting secrets or DB access into an isomorphic loader
- using `createCompositeComponent` when `renderServerComponent` would do
- using `renderServerComponent` when slots are required
- treating route invalidation and query invalidation as interchangeable
- caching RSC values in Query without `structuralSharing: false`
- inspecting or cloning opaque slot content on the server
- using stale names or stale validation APIs from older examples

---

## Repository layout

```text
.
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
├── docs/
│   ├── architecture.md
│   ├── caching-refresh-ssr.md
│   ├── composite-components.md
│   ├── current-api-notes.md
│   ├── debugging-review.md
│   └── sources.md
├── examples/
│   ├── 01-renderable-route-loader.tsx
│   ├── 02-composite-slots.tsx
│   ├── 03-query-owned-rsc.tsx
│   ├── 04-selective-ssr-data-only.tsx
│   ├── 05-ssr-false-browser-loader.tsx
│   └── 06-low-level-flight-api-route.tsx
└── scripts/
    └── build-zip.sh
```

### File guide

- `SKILL.md` - agent entrypoint; fast activation surface, invariants, decision tree, review/debug routing
- `agents/openai.yaml` - optional Codex metadata and default prompt
- `docs/` - deeper reference material only when needed
- `examples/` - small copy-paste patterns for the highest-value implementation paths
- `scripts/build-zip.sh` - builds a clean share/import zip with the correct top-level folder

---

## Updating

Pick the update path that matches the install path:

- installed with `npx skills` -> `npx skills update tanstack-start-rsc` or `npx skills update -g`
- symlink install -> `git pull`
- copied install -> re-run your `rsync` command
- zip install -> rebuild the zip and re-upload or replace the extracted folder
- checked-in project copy -> update the files in that repo normally

---

## Why the repo is shaped like this

This repo keeps the skill **at the root** instead of hiding it in a packaging wrapper.

That gives you one canonical source that works for:

- direct GitHub install via `npx skills add owner/repo`
- local path install via `npx skills add ./path`
- manual symlink/copy installs
- zip upload to Claude
- raw inspection and editing

The README is for humans. `SKILL.md` is for agents.

---

## Relevant docs

- [TanStack Start React Server Components guide](https://tanstack.com/start/latest/docs/framework/react/guide/server-components)
- [TanStack blog: React Server Components](https://tanstack.com/blog/react-server-components)
- [Skills CLI / open skills ecosystem](https://github.com/vercel-labs/skills)
- [skills.sh directory](https://skills.sh)
- [Claude Code skills docs](https://code.claude.com/docs/en/skills)
- [Claude custom skills upload docs](https://support.claude.com/en/articles/12512180-use-skills-in-claude)
- [OpenAI Codex skills docs](https://developers.openai.com/codex/skills)
