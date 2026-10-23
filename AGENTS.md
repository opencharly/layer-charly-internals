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

Edit those skills in the repo named above, never here, and never in the
generated `marketplace/<family>/skills/<name>/SKILL.md` projection. Both
destination repos are DIRECT corpus sources (pinned in
`marketplace/candy/charly-marketplace/charly.yml`), so the address does not
change. For eight of the nine the published body does not change either; the
exception is `generate-source`, whose `sdk` copy already carries corrections the
carrier copy predates (K-wave's `charly/*` deletions), so the next corpus
regeneration publishes the newer text.

## What still lives here, and why

Eleven skills keep their body in `.agents/skills/` because moving them now would
leave their prose unreachable from the corpus:

- `agents`, `cutover-policy`, `root-cause-analyzer` — the body ALREADY exists in
  the umbrella (`opencharly/opencharly`), but the umbrella is not a corpus
  source, so this repo is the only source the published corpus reads them from.
  They move in the same change that pins the umbrella in the corpus refs list,
  not before it.
- `egress`, `go`, `go-quality`, `marketplace`, `plugin`, `vm-deploy-target` —
  each has a measured destination repo (`plugin-egress`, `charly`, `charly`,
  `plugin-marketplace`, `charly`, `plugin-deploy-vm`) but that repo does not
  author the skill yet, so the body here is the only copy in the org. Each moves
  when its destination's shelf lands.
- `git-workflow`, `repo-setup` — filed as not routable
  (opencharly/opencharly#611): their config home is `opencharly/.github`, which
  is not a submodule and not a corpus source.

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
