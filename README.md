# TanStack Start RSC Skill

TanStack Start React Server Components skill for Codex, Claude Code, Cursor, and other AI coding agents working with React Flight, `renderServerComponent`, `createCompositeComponent`, loader-owned RSCs, query-owned RSCs, SSR modes, and refresh invalidation.

Portable raw skill for **TanStack Start React Server Components**.

This repository is intentionally the **skill folder itself**. Clone it, copy it into a project, symlink it globally, zip it for upload, or hand it to another agent to install. No wrapper package is required.

`README.md` is here for the public GitHub repo: discovery, install instructions, and human context. The actual skill is `SKILL.md` plus the supporting `docs/`, `examples/`, and optional `agents/` files. If you package the skill for install, `README.md` can be omitted.

The skill is built for actual TanStack Start RSC work:

- choosing the right primitive: `renderServerComponent` vs `createCompositeComponent` vs low-level Flight APIs
- choosing the real cache owner: Router vs Query vs HTTP/server cache
- wiring the real refresh path: `router.invalidate()` vs Query invalidation vs HTTP revalidation
- Composite Components and slot composition
- SSR mode selection, loader boundaries, and stale API cleanup

It is not a docs dump.

---

## What is in the repo

```text
.
├── .gitignore
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

- `README.md` — GitHub-facing documentation for humans; not required for the installed skill itself
- `SKILL.md` — the agent entrypoint; fast activation surface, invariants, decision tree, review/debug routing
- `agents/openai.yaml` — optional Codex metadata and default prompt
- `docs/` — deeper reference material only when needed
- `examples/` — small copy-paste patterns for the highest-value implementation paths
- `scripts/build-zip.sh` — builds a clean share/import zip with the correct top-level folder

---

## Install — choose the path that fits your workflow

There is no single correct install path. Use the one that matches how you work.

### 1. Symlink the repo

Best when you want `git pull` updates to show up immediately.

#### Codex — global

```bash
mkdir -p "$HOME/.codex/skills"
ln -s "$(pwd)" "$HOME/.codex/skills/tanstack-start-rsc"
```

#### Codex — project-scoped

```bash
mkdir -p /path/to/project/.codex/skills
ln -s "$(pwd)" /path/to/project/.codex/skills/tanstack-start-rsc
```

#### Claude Code — global

```bash
mkdir -p "$HOME/.claude/skills"
ln -s "$(pwd)" "$HOME/.claude/skills/tanstack-start-rsc"
```

#### Claude Code — project-scoped

```bash
mkdir -p /path/to/project/.claude/skills
ln -s "$(pwd)" /path/to/project/.claude/skills/tanstack-start-rsc
```

### 2. Copy the repo

Best when you do not want symlinks.

#### Codex — global

```bash
mkdir -p "$HOME/.codex/skills/tanstack-start-rsc"
rsync -a --delete --exclude '.git' --exclude 'dist' ./ "$HOME/.codex/skills/tanstack-start-rsc/"
```

#### Codex — project-scoped

```bash
mkdir -p /path/to/project/.codex/skills/tanstack-start-rsc
rsync -a --delete --exclude '.git' --exclude 'dist' ./ /path/to/project/.codex/skills/tanstack-start-rsc/
```

#### Claude Code — global

```bash
mkdir -p "$HOME/.claude/skills/tanstack-start-rsc"
rsync -a --delete --exclude '.git' --exclude 'dist' ./ "$HOME/.claude/skills/tanstack-start-rsc/"
```

#### Claude Code — project-scoped

```bash
mkdir -p /path/to/project/.claude/skills/tanstack-start-rsc
rsync -a --delete --exclude '.git' --exclude 'dist' ./ /path/to/project/.claude/skills/tanstack-start-rsc/
```

### 3. Build a clean zip

Best when you want a single-file artifact for upload, sharing, or agent-assisted install.

From the repo root:

```bash
./scripts/build-zip.sh
```

Output:

```text
dist/tanstack-start-rsc-skill.zip
```

That zip is intentionally cleaner than a raw source archive:

- top-level folder is `tanstack-start-rsc/`
- includes `SKILL.md`, `agents/`, `docs/`, and `examples/`
- excludes repo-only files such as `README.md`, `.git/`, `.github/`, `dist/`, and `scripts/`

Manual equivalent:

```bash
cd ..
zip -r tanstack-start-rsc-skill.zip tanstack-start-rsc \
  -x 'tanstack-start-rsc/.git/*' \
     'tanstack-start-rsc/.github/*' \
     'tanstack-start-rsc/dist/*' \
     'tanstack-start-rsc/scripts/*' \
     'tanstack-start-rsc/README.md' \
     'tanstack-start-rsc/.DS_Store'
```

If you want a zip for Claude upload or a clean share artifact, prefer the custom build zip over GitHub's auto-generated source zip.

### 4. Install from the zip

Use the zip however you prefer:

- upload it in Claude's custom skill UI
- unzip it into `~/.claude/skills` or `.claude/skills`
- unzip it into `~/.codex/skills` or `.codex/skills`
- hand it to Claude or Codex and tell it where to install it

Examples:

```bash
unzip dist/tanstack-start-rsc-skill.zip -d "$HOME/.claude/skills"
# or
unzip dist/tanstack-start-rsc-skill.zip -d "$HOME/.codex/skills"
```

The only invariant that matters is the extracted path:

```text
$HOME/.claude/skills/tanstack-start-rsc/SKILL.md
# or
$HOME/.codex/skills/tanstack-start-rsc/SKILL.md
```

### 5. Ask another agent to install it for you

Best when you want Claude or Codex to do the file placement.

Minimal prompt:

```text
Install this TanStack Start RSC skill. Keep the folder name tanstack-start-rsc and make sure the final path ends in SKILL.md. For Codex use $HOME/.codex/skills/tanstack-start-rsc. For Claude Code use $HOME/.claude/skills/tanstack-start-rsc.
```

That works whether you hand it the repo folder or the generated zip.

---

## Updating

Pick the update model that matches the install path.

- **symlink install** — pull the repo and you are done
- **copied install** — re-run the `rsync` command or replace the installed folder
- **zip install** — rebuild the zip with `./scripts/build-zip.sh`, then re-upload or replace the extracted folder
- **project-scoped checked-in copy** — update the files in that project and commit them normally

---

## Using the skill

### Direct invocation

If your tool exposes slash-command skill invocation:

```text
/tanstack-start-rsc
```

### Minimal prompt

```text
Use the tanstack-start-rsc skill. Treat TanStack Start RSC as fetchable Flight payloads, not a framework-owned server tree. Pick the smallest correct primitive, identify cache and refresh ownership, and fix any stale names or invalidation mistakes before making changes.
```

### Review / debug prompt

```text
Use the tanstack-start-rsc skill. Audit this route and any related server functions for primitive choice, cache owner, refresh owner, missing loaderDeps, Query RSC misuse, opaque slot misuse, SSR mode mistakes, and stale API names. Then propose the smallest correct refactor.
```

---

## Why the repo is shaped this way

This repo keeps the skill **at the root** instead of burying it in a packaging wrapper.

That gives you one canonical source that works for all of these flows without restructuring:

- clone and symlink
- clone and copy
- zip and upload
- zip and ask an agent to install it
- inspect and edit the raw skill files directly

The README is for the GitHub repo and human readers. `SKILL.md` is for agents. The deeper material is split out so the entrypoint stays lean and trigger-friendly.

---

## Official references

- [TanStack Start React Server Components guide](https://tanstack.com/start/latest/docs/framework/react/guide/server-components)
- [TanStack blog: React Server Components](https://tanstack.com/blog/react-server-components)
- [Claude Code skills docs](https://code.claude.com/docs/en/skills)
- [Claude custom skills upload docs](https://support.claude.com/en/articles/12512180-use-skills-in-claude)
- [OpenAI Codex skills docs](https://developers.openai.com/codex/skills)

Raw source links are also tracked in `docs/sources.md`.
