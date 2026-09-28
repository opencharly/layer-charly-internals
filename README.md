# charly-internals

The `charly-internals` family — the contributor-facing internals skills.

The `charly-internals` candy is a **concept candy**: it ships no install content
and owns the `internals` family of `skill:` entities that document charly's
internal architecture and contributor workflow. It currently carries twenty
entities: `git-workflow`, `go`, `go-quality`, `plugin`, `skills`, `marketplace`,
`repo-setup`, `agents`, `capabilities`, `install-plan`, `egress`,
`cutover-policy`, `disposable`, `generate-source`, `cloud-init-renderer`,
`libvirt-renderer`, `local-infra`, `ovmf`, `root-cause-analyzer`, and
`vm-deploy-target`. `candy/plugin-marketplace` regenerates the standalone
[opencharly/marketplace](https://github.com/opencharly/marketplace) corpus from
these entities, so the skills are authored here and projected there.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `charly-internals` (concept candy) |
| Install content | none — a `true` no-op `plan:` |
| Owns | 20 `skill:` entities in the `internals` family |
| Projected to | `marketplace/internals/skills/` |
| Service / port | none |

The rest of the `internals` family is owned by sibling repos:
`layer-charly-internals-extra` (`strict-policy`, `vm-spec`, and the six agent
entities) and `plugin-nerdctl` (`plugin-nerdctl`).

## How to use it

This repo is consumed as a **skill source**, not as an image layer. Edit the
`skill:` entities in `charly.yml`; the marketplace regeneration projects them
into `/charly-internals:*` pages. To reference the repo directly, compose it in a
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

- `charly.yml` — the `charly-internals:` concept candy entity plus 20 `skill:`
  entities (`git-workflow`, `go`, `go-quality`, `plugin`, `skills`,
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
- Overflow entities: `opencharly/layer-charly-internals-extra`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
