# charly-internals

The `charly-internals` family — the contributor-facing internals skills.

The `charly-internals` candy is a **concept candy**: it ships no install content
and owns the `internals` family of skills that document charly's internal
architecture and contributor workflow. It currently carries twenty skills:
`git-workflow`, `go`, `go-quality`, `plugin`, `skills`, `marketplace`,
`repo-setup`, `agents`, `capabilities`, `install-plan`, `egress`,
`cutover-policy`, `disposable`, `generate-source`, `cloud-init-renderer`,
`libvirt-renderer`, `local-infra`, `ovmf`, `root-cause-analyzer`, and
`vm-deploy-target`. Each skill's prose is authored with the skill itself, and
`charly.yml` carries the routing metadata its consumers read;
`candy/plugin-marketplace` regenerates the standalone
[opencharly/marketplace](https://github.com/opencharly/marketplace) corpus from
the two together, so the skills are authored here and projected there.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `charly-internals` (concept candy) |
| Install content | none — a `true` no-op `plan:` |
| Owns | the 20 skills of the `internals` family |
| Projected to | `marketplace/internals/skills/` |
| Service / port | none |

The rest of the `internals` family is owned by sibling repos:
`layer-charly-internals-extra` (`strict-policy`, `vm-spec`, and the six agent
definitions) and `plugin-nerdctl` (`plugin-nerdctl`).

## How to use it

This repo is consumed as a **skill source**, not as an image layer. Edit a
skill's prose with the skill — the harness resolves it by name, and
`charly.yml` holds only the routing metadata; the marketplace regeneration
projects the two into `/charly-internals:*` pages. To reference the repo directly, compose it in a
box. A box is a `candy:` node that carries the box's `base:` image and a nested
`candy:` list of layer refs (the nested `candy:` is the composition list; the
outer `candy:` is the box body):

```yaml
my-box:
  candy:                  # the box body (an IMAGE is a `candy:` node carrying `base:`)
    base: fedora          # the box's base image
    candy:                # the box's composition list
      - '@github.com/opencharly/layer-charly-internals:v2026.271.1802'
```

## Layout

- `charly.yml` — the `charly-internals:` concept candy entity plus the routing
  metadata for its family's 20 skills (`git-workflow`, `go`, `go-quality`, `plugin`, `skills`,
  `marketplace`, `repo-setup`, `agents`, `capabilities`, `install-plan`,
  `egress`, `cutover-policy`, `disposable`, `generate-source`,
  `cloud-init-renderer`, `libvirt-renderer`, `local-infra`, `ovmf`,
  `root-cause-analyzer`, `vm-deploy-target`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skills: `/charly-internals:git-workflow`,
  `/charly-internals:skills`, `/charly-internals:marketplace`
- Authoring reference: `/charly-image:layer`
- Overflow skills: `opencharly/layer-charly-internals-extra`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
