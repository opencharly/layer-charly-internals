# AGENTS.md — layer-charly-internals

Standalone candy repo for the `charly-internals` concept candy. It ships no
install content. It authors only the `internals` skills that still need it here;
every skill whose mechanism lives in another repo and whose home can publish it
has moved beside that mechanism, and the map below says where. `charly.yml` at
the repo root carries routing metadata only, and `candy/plugin-marketplace`
regenerates the standalone opencharly/marketplace corpus from that metadata plus
each skill's own file.

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
resolves as `/charly-internals:<name>`. The one exception to byte-identity is
`agents/references/hooks-and-lifecycle.md`, whose umbrella copy was CORRECTED
after the move (opencharly/opencharly#617 stopped it restating the coordination
closed set) — the umbrella's text is the authored one, and the copy removed here
was its stale twin.

**Two groups, two different corpus states — do not conflate them.**

- `sdk`, `spec` and `opencharly/opencharly` are DIRECT corpus sources (pinned in
  `marketplace/candy/charly-marketplace/charly.yml`), so their skills are
  already published FROM those repos. The umbrella is pinned at
  `v2026.296.2254` (opencharly/marketplace#567), so its three — `agents`,
  `cutover-policy`, `root-cause-analyzer` — are published from the umbrella's
  own bodies, not from this repo's copies. That is why this repo drains them.
- `charly`, `plugin-egress`, `plugin-marketplace` and `plugin-deploy-vm` are NOT
  corpus sources — none of the four has a refs-list entry. Their six skills are
  therefore still SERVED from this repo's pinned tag
  (`layer-charly-internals:v2026.296.1907`), whose tree holds all of them;
  nothing a harness loads changes until the refs leg pins those four repos.
  That leg must also settle a duplicate: `charly-version` is authored in `charly`
  (since charly#1036, `family: core`) AND in `pod-charly-core` (owner
  `charly-core`, the copy the corpus serves today, with identical content), so
  adding `charly` as a source must settle which repo owns that address.

**Advancing this repo's refs-list pin past a drain drops skills, and today it
does so silently.** This repo's pin (`v2026.296.1907`) is the only CORPUS source
that still publishes the six skills the first drain moved out — `egress`, `go`,
`go-quality`, `marketplace`, `plugin`, `vm-deploy-target` — because none of the
repos that now author them is a refs-list entry. Advancing the pin past a drain
therefore stops serving all six, and three of them cannot be recovered at all
yet: `sdk/candywalk`'s `acceptedRef` refuses `github.com/opencharly/charly` by
design, so `charly` cannot be a corpus source until that changes. The pin stays
where it is until then.

## What still lives here, and why

Two skills keep their body in `.agents/skills/`:

- `git-workflow`, `repo-setup` — filed as not routable
  (opencharly/opencharly#611): their config home is `opencharly/.github`, which
  is not a submodule and not a corpus source.

`agents`, `cutover-policy` and `root-cause-analyzer` are DRAINED: the umbrella is
a corpus source, so it both authors and publishes them, and this repo's copies
were only a second copy. This repo therefore cannot be archived yet — it still
holds the two above.

## Load these skills first (R0)

- `/charly-internals:skills` — skill maintenance and the authored-content
  contract (authored in `opencharly/sdk`). Load before editing a skill, this
  file, or any routing metadata.
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
  `check:` step is a `true` no-op as well. Neither asserts that the skill bodies
  exist — the body-existence gate is `charly box validate`, which resolves every
  `skill:` node's owner and file. There is no live bed.

## Modify this repo

- Add or remove a `skill:` node only together with its body: a node whose owning
  candy has no body fails `charly box validate`.
- Keep the concept candy's `plan:` a no-op; it exists only to carry the
  remaining skills' routing metadata.
- When a skill still authored here gains a reachable home, delete its tree and
  its node in the same change and update the map above.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
