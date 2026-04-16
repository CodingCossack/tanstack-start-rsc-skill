# TanStack Start RSC Skill

TanStack Start React Server Components skill for `renderServerComponent`,
`createCompositeComponent`, Composite Components, React Flight, TanStack
Router, TanStack Query, cache ownership, `router.invalidate()`, SSR modes, and
RSC refresh patterns.

This skill helps implement, review, debug, and refactor TanStack Start RSC
apps by choosing the right RSC primitive, assigning Router vs Query ownership,
wiring refresh correctly, designing slot-based Composite Components, and fixing
stale or outdated API usage.

## What this TanStack Start skill actually does

- chooses between `renderServerComponent`, `createCompositeComponent`, and
  low-level Flight APIs based on the real composition need
- clarifies cache ownership across TanStack Router, TanStack Query, and
  HTTP or server caching
- maps refresh ownership to the correct mechanism such as
  `router.invalidate()`, query invalidation, or HTTP revalidation
- handles Composite Components, client slots, render props, and opaque slot
  payload rules
- reviews SSR modes including standard SSR, `ssr: 'data-only'`, and `ssr: false`
- flags stale APIs, outdated examples, invalid loader assumptions, and RSC cache
  mistakes such as missing `structuralSharing: false`
- supports migrations from stale or Next App Router-shaped mental models to
  current TanStack Start RSC patterns

## Best use cases

- TanStack Start React Server Components implementation
- debugging stale RSC data and broken refresh flows
- choosing Router vs Query ownership for RSC payloads
- Composite Components and slot design
- reviewing loader boundaries, `loaderDeps`, `staleTime`, and SSR configuration
- migrating outdated TanStack Start RSC examples to current APIs

## Outputs

- correct RSC primitive choice
- cache owner and refresh owner decisions
- safer server/client boundary design
- concrete implementation and refactor guidance
- framework-specific fixes instead of generic React Server Components advice

## Core capability areas

The core value is in the architecture, cache, and debugging guidance behind the
skill:

- `docs/architecture.md` for the TanStack Start RSC mental model
- `docs/caching-refresh-ssr.md` for cache ownership, refresh paths, and SSR
  modes
- `docs/composite-components.md` for Composite Components and slot rules
- `docs/current-api-notes.md` for stale API cleanup and current naming
- `docs/debugging-review.md` for debugging and review workflows
- `examples/` for copy-paste patterns covering the highest-value RSC paths

## Fast install

The install folder name stays `tanstack-start-rsc`.

```bash
npx skills add CodingCossack/tanstack-start-rsc-skill -g -a claude-code -a codex -y
```

Useful variants:

```bash
# See what the repo contains before installing
npx skills add CodingCossack/tanstack-start-rsc-skill --list

# Install from a full GitHub URL
npx skills add https://github.com/CodingCossack/tanstack-start-rsc-skill

# Install from a local folder
npx skills add ./tanstack-start-rsc
```

Update later:

```bash
npx skills update tanstack-start-rsc -g
```

## Manual install

### Codex global

```bash
mkdir -p "$HOME/.codex/skills"
ln -s "$(pwd)" "$HOME/.codex/skills/tanstack-start-rsc"
```

### Codex project-scoped

```bash
mkdir -p /path/to/project/.agents/skills
ln -s "$(pwd)" /path/to/project/.agents/skills/tanstack-start-rsc
```

### Claude Code global

```bash
mkdir -p "$HOME/.claude/skills"
ln -s "$(pwd)" "$HOME/.claude/skills/tanstack-start-rsc"
```

### Claude Code project-scoped

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

## Zip packaging

Build a clean zip from the repo root:

```bash
./scripts/build-zip.sh
```

Output:

```text
dist/tanstack-start-rsc-skill.zip
```

Claude custom skill upload expects the skill folder inside the ZIP, not loose
files at the ZIP root.

Manual unzip install:

```bash
unzip dist/tanstack-start-rsc-skill.zip -d "$HOME/.claude/skills"
# or
unzip dist/tanstack-start-rsc-skill.zip -d "$HOME/.codex/skills"
```

The final path should end in:

```text
$HOME/.claude/skills/tanstack-start-rsc/SKILL.md
# or
$HOME/.codex/skills/tanstack-start-rsc/SKILL.md
```

## Verify it loaded

### Claude Code

Direct invocation:

```text
/tanstack-start-rsc
```

### Codex

Good direct prompt:

```text
Use the tanstack-start-rsc skill. First classify this task as renderServerComponent vs createCompositeComponent vs low-level Flight API. Then identify the real cache owner and refresh owner. Then implement or refactor the smallest correct solution.
```

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
