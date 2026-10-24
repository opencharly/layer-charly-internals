# AGENTS.md — layer-charly-internals

**This repo is RETIRED.** It authors no skill: every one of the `internals`
family's skills is authored in the repo that owns the code, plugin, or config it
describes (umbrella `AGENTS.md` Part II #5), and the last two — `git-workflow` and
`repo-setup`, whose config home `opencharly/.github` cannot author skills — are
DRAINED to the umbrella, which is a DIRECT corpus source and publishes them
(opencharly/opencharly#611). The repo is archived once its drain tag is minted;
the destination map below is kept so a reader who lands here knows where the
prose went.

## Destination map — where each `internals` skill is authored now

A skill is authored in the repo that owns the code, plugin, or config it
describes (umbrella `AGENTS.md` Part II #5). The address does not change when the
body moves: every row still resolves as `/charly-internals:<name>` in the
corpus.

| Skill | Authored in | Body |
|---|---|---|
| `capabilities` | `opencharly/sdk` | `.agents/skills/capabilities/` |
| `cloud-init-renderer` | `opencharly/sdk` | `.agents/skills/cloud-init-renderer/` |
| `generate-source` | `opencharly/sdk` | `.agents/skills/generate-source/` |
| `libvirt-renderer` | `opencharly/sdk` | `.agents/skills/libvirt-renderer/` |
| `local-infra` | `opencharly/sdk` | `.agents/skills/local-infra/` |
| `ovmf` | `opencharly/sdk` | `.agents/skills/ovmf/` |
| `skills` | `opencharly/sdk` | `.agents/skills/skills/` |
| `disposable` | `opencharly/spec` | `.agents/skills/disposable/` |
| `install-plan` | `opencharly/spec` | `.agents/skills/install-plan/` |
| `agents` | `opencharly/opencharly` | `.agents/skills/agents/` + `references/` |
| `cutover-policy` | `opencharly/opencharly` | `.agents/skills/cutover-policy/` |
| `git-workflow` | `opencharly/opencharly` | `.agents/skills/git-workflow/` + `references/` |
| `repo-setup` | `opencharly/opencharly` | `.agents/skills/repo-setup/` |
| `root-cause-analyzer` | `opencharly/opencharly` | `.agents/skills/root-cause-analyzer/` |
| `go` | `opencharly/charly` | `.agents/skills/go/` + `references/` |
| `go-quality` | `opencharly/charly` | `.agents/skills/go-quality/` |
| `plugin` | `opencharly/charly` | `.agents/skills/plugin/` + `references/` |
| `egress` | `opencharly/plugin-egress` | `.agents/skills/egress/` |
| `marketplace` | `opencharly/plugin-marketplace` | `.agents/skills/marketplace/` |
| `vm-deploy-target` | `opencharly/plugin-deploy-vm` | `.agents/skills/vm-deploy-target/` |

Edit those skills in the repo named above, never here, and never in the
generated `marketplace/<family>/skills/<name>/SKILL.md` projection. Every moved
body was copied byte-identically (verified by `sha256` against this repo's
`main` at the time of the move), and the address does not change: each still
resolves as `/charly-internals:<name>`. One exception to byte-identity is
`agents/references/hooks-and-lifecycle.md`, whose umbrella copy was CORRECTED
after the move (opencharly/opencharly#617 stopped it restating the coordination
closed set) — the umbrella's text is the authored one, and the copy removed here
was its stale twin.

**Every one of the twenty is now DRAINED**, in two waves: the first
(`opencharly/layer-charly-internals#142`) moved twelve to `sdk`, `spec`, the
umbrella, `charly` and three plugin repos; the second moves the last two
(`git-workflow`, `repo-setup`) to the umbrella.

**What the corpus serves now, and what is still owed.** `sdk`, `spec`,
`opencharly/opencharly`, `charly`, `plugin-egress`, `plugin-marketplace` and
`plugin-deploy-vm` are all DIRECT sources in
`marketplace/candy/charly-marketplace/charly.yml`, so their eighteen skills are
published from the repos that author them. The two this change drains are NOT yet
published from the umbrella: that refs list still pins this repo at
`v2026.297.0301`, whose tree holds both. Dropping this ref while advancing the
umbrella pin to a tag that carries them is the refs leg — one PR in
`opencharly/marketplace`, landing after this repo's drain tag. Until it lands the
corpus keeps serving this repo's copies, so nothing a harness loads changes in
the drain itself.

## Load these skills first (R0)

- `/charly-internals:skills` — skill maintenance and the authored-content
  contract (authored in `opencharly/sdk`). Load before editing a skill or any
  routing metadata.
- `/charly-internals:git-workflow` — the landing mechanics (feat/ branch,
  PR-only, the fresh validator, CalVer-at-merge). Load before any git/`gh`
  action.
- `/charly-internals:marketplace` — the corpus regeneration and drift gate. Load
  before any regeneration or refs-list change.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse, and every `skill:` node's owning candy must resolve a body. Before
  running it, verify `command -v charly` resolves to the `bin/charly` built from
  the active charly source worktree, and verify `charly version` matches that
  source. Run it with `CHARLY_PLUGIN_ONLY=1` so a worktree build does not scan
  incompatible packaged plugins from `/usr/lib/charly/plugins`. Never validate
  with the shared host install.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate.
- The candy is documentation-only: its `plan:` is a `true` no-op, and its
  `check:` step is a `true` no-op as well. Neither asserts anything — the candy
  owns no `skill:` node now, so there is no body for a check to resolve, and the
  structural gate is `charly box validate`. There is no live bed.

## Modify this repo

- **Don't.** It is retired; a change belongs in the repo that now authors the
  skill (the table above).
- A `skill:` node and its body are removed or added together: a node whose owning
  candy has no body fails `charly box validate`. That is exactly the arm this
  repo's own drain proved, per name.
- The concept candy keeps a `true` no-op `plan:` and a `true` no-op `check:` so
  the manifest still parses and validates; neither step is an assertion.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
