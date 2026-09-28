# AGENTS.md — layer-charly-internals

Standalone candy repo for the `charly-internals` concept candy — it ships no
install content and owns the `internals` family of `skill:` entities that
document charly's internal architecture and contributor workflow. The entities
live in `charly.yml` at the repo root; `candy/plugin-marketplace` regenerates the
standalone opencharly/marketplace corpus from them.

Canonical files:

- `charly.yml` — the `charly-internals:` concept candy entity plus 20 `skill:`
  entities (`git-workflow`, `go`, `go-quality`, `plugin`, `skills`,
  `marketplace`, `repo-setup`, `agents`, `capabilities`, `install-plan`,
  `egress`, `cutover-policy`, `disposable`, `generate-source`,
  `cloud-init-renderer`, `libvirt-renderer`, `local-infra`, `ovmf`,
  `root-cause-analyzer`, `vm-deploy-target`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:skills` — the owning skill for this repo's family (skill
  maintenance: where documentation belongs, the projection contract). Load
  before editing the concept candy or any `skill:` entity.
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
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate.
- The candy is documentation-only: its `plan:` is a `true` no-op plus a `true`
  `check:` that the skill entities are present and generated into the
  marketplace. There is no live bed.

## Modify this repo

- The `skill:` entities are the projected usage source. Edit them here, never the
  generated `SKILL.md` in the marketplace corpus; a corpus regeneration
  (`charly marketplace generate`) projects them.
- When charly's internals or contributor workflow changes, update the matching
  `skill:` entity in the same change so the corpus does not drift.
- Keep the concept candy's `plan:` no-op; it exists only to carry the entities.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
